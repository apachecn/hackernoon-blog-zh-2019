# 如何在盖茨比博客上添加帖子草稿

> 原文：<https://medium.com/hackernoon/how-to-add-draft-posts-to-a-gatsby-blog-3f6c64ec0c06>

![](img/6edd11de214bdf415ffb6b4b5bf375fe.png)

我需要在我用 Gatsby 构建的博客中从生产中排除特定的帖子，并在开发期间将它们标记为草稿。我将描述如何实现这个目标，这样你也可以在不到 5 分钟的时间内完成。

这篇文章是《10 比盖茨比更好》系列文章的一部分，在这里我分享了我个人调整盖茨比的经验。

# 最终结果

正在开发的职位列表:

![](img/e71de2ee0f30a51d444fea8f084c917c.png)

生产岗位列表:

![](img/6e315eedbdaea03ab7a8370b08b3a0bc.png)

# 要求

*   显示开发中的所有帖子
*   在制作中隐藏草稿文章
*   在开发中以不同方式呈现已发布和草稿帖子
*   如果 post 的`date`在构建时间之后，则将 post 标记为自动发布
*   在`md`文件中手工标记发布。

我找到了一个[插件](https://www.gatsbyjs.org/packages/gatsby-plugin-draft/)，但无法实现我需要的一切。我的开源解决方案可以在这里找到:

[盖茨比外挂发布](https://github.com/rg4real/gatsby-plugin-released)

它允许您通过 config 添加发布功能，并跳过本文的部分内容。此外，您还可以使用其他选项。请访问插件自述文件了解更多信息。

# 概观

**第一步。向 GraphQL 添加新字段**

我们将添加一个名为`released`的字段，基于:

*   `date`减价文件中的字段
*   `released`来自 markdown 文件的字段
*   `process.env.NODE_ENV`
*   `timezone`
*   构建时间(构建发生的时刻)

此外，我们将添加一个名为`releasedNotForced.`的字段，类似于`released`，但忽略了`process.env.NODE_ENV`。

这一步被抽象成 [gatsby-plugin-released](https://github.com/rg4real/gatsby-plugin-released)

第二步。更新 GraphQL 查询以尊重 `**released**` **值**

我们应该将草稿排除在构建和显示之外。

第三步。更新组件以不同方式呈现草稿。

有一个很好的视觉区别并且感觉良好。:)

# 执行

# 第一步。向 GraphQL 添加新字段

这一步的目标是通过 GraphQL 向`node.frontmatter.fields`添加要使用的字段。Gatsby 为此提供了一个特殊的 API。我们需要的是修改`gatsby-node.js`文件。

我们将添加两个字段:

*   `released`字段
*   `releasedNotForced`字段，其行为与`released`相似，但忽略了`process.env.NODE_ENV`变量。

# 为什么添加两个字段？

以防你想知道。

在开发模式下，我们可能希望在不编辑任何 GraphQL 查询的情况下强制呈现帖子。是在`process.env.NODE_ENV`的基础上强制`released`为`true`来完成的。因此，在开发模式中，我们失去了我们可能希望在组件代码中使用的原始值，以便在草稿和发布的帖子之间有一个直观的区别。

保持这一点的方法是总是在 markdown 文件中设置`released`字段。但是让这个值基于`date`自动计算出来真是太好了。

这就是为什么我添加了一个`releasedNotForced`属性——在强制`released`为真的同时保留该值。

记住，如果你不想弄乱你的配置，就使用[这个插件](https://github.com/rg4real/gatsby-plugin-released)。

下面是我们用来计算`releasedNotForced`值的函数。

```
// gatsby-node.js
const moment = require('moment-timezone')
const getValue = ({ node, options }) => {
  const { fieldName, timezone } = options
  if (!node.frontmatter) {
    return false
  } if (node.frontmatter.hasOwnProperty(fieldName)) {
    return node.frontmatter[fieldName]
  } if (!node.frontmatter.date) {
    return false
  } const dateNode = moment.tz(node.frontmatter.date, timezone)
  const dateNow = moment().tz(timezone)
  const value = dateNow.isSameOrAfter(dateNode) return value
}
```

然后我们给`node.frontmatter.fields`添加`released`和`releasedNotForced`字段。我们需要的是使用`onCreateNode`功能。

```
// gatsby-node.js
const onCreateNode = ({ node, actions }) => {
  const MD_TYPE = 'MarkdownRemark'
  const options = {
    fieldName: 'released',
    fieldNameNotForced: 'releasedNotForced',
    timezone: 'UTC',
    force: process.env.NODE_ENV === 'development',
  }
  const { createNodeField } = actions
  const { fieldName, fieldNameNotForced } = options // Skip modifications for non-markdown files
  if (node.internal.type !== MD_TYPE) {
    return
  } const value = getValue({ node, options }) createNodeField({
    node,
    name: fieldName,
    value: options.force === true ? true : value,
  })
  createNodeField({
    node,
    name: fieldNameNotForced,
    value,
  })
}
```

# 第二步。更新 GraphQL 查询以考虑`released`值

我们需要从文件`gatsby-node.js`的构建步骤中排除草稿，并考虑来自博客页面(如`index.js.`)的`released`值

在这两种情况下，查询看起来都像这样。注意一个过滤器属性。

```
const query = graphql(
  `
    {
      allMarkdownRemark(
        sort: { fields: [frontmatter___date], order: DESC }
        filter: { fields: { released: { eq: true } } }
      ) {
        edges {
          node {
            id
          }
        }
      }
    }
  `
)
```

# 第三步。更新组件以不同方式呈现草稿

这一步完全取决于您的组件树。关键是通过 GraphQL 查询请求必要的字段。

```
const query = graphql`
  query {
    allMarkdownRemark(
      sort: { fields: [frontmatter___date], order: DESC }
      filter: { fields: { released: { eq: true } } }
    ) {
      edges {
        node {
          id
          fields {
            slug
            released
            releasedNotForced
          }
        }
      }
    }
  }
`
```

# 结论

因此，我们有一个用于生产和开发的单一 GraphQL 查询，`released`字段是自动计算的，草稿帖子可以不同地呈现。酷！

注意了！我的 [10x better Gatsby](https://blog.swingpulse.com/10x-better-gatsby) 系列有草稿！:)所以去看看吧，敬请期待！

## P.S .需要更多价值？厉害！