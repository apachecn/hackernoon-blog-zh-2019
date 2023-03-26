# 具有 AuthorizationPolicyProvider 的 ASP.NET 核心中基于权限的授权

> 原文：<https://medium.com/hackernoon/permission-based-authorization-asp-net-core-with-authorizationpolicyprovider-af4933d575ee>

![](img/bd110bfa0621f4ac8b0e472f3d3af989.png)

Photo by [Oluwaseun Duncan](https://www.pexels.com/@duncanoluwaseun) on [Pexels](http://pexels.com)

有多种方法可以实现动态的基于许可的授权；在这篇文章中，我想实现自定义授权策略提供者，以简化 ASP.NET 核心中基于权限的授权机制。

# 介绍

根据 ASP.NET 核心中的授权基础结构，您可以使用以下代码段来应用基于声明的授权和自定义权限声明类型:

```
services.AddAuthorization(options =>
{
    options.AddPolicy("View Projects",
        policy => policy.RequireClaim(CustomClaimTypes.Permission, "projects.view"));
});
```

你可以如下使用它:

```
[Authorize("View Projects")]
public IActionResult Index(int siteId)
{
    return View();
}
```

这种方法是集成的，非常简单，你不需要做任何定制；但是，在一个真实的项目或企业范围内，很难将所有权限都定义为基于声明的策略。幸运的是，[ASP.NET 核心支持实现自定义授权策略提供者](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/iauthorizationpolicyprovider?view=aspnetcore-2.1)并在 DI 系统中注册。它的用途之一是:

> 使用大范围的策略(例如，针对不同的房间号或年龄)，因此为每个单独的授权策略添加一个 AuthorizationOptions 没有意义。添加策略调用。

# 实施授权策略提供者

为此，我们可以实现 AuthorizationPolicyProvider 或从 DefaultAuthorizationPolicyProvider 继承，后者在 DI 系统中注册为默认提供者。

```
public class AuthorizationPolicyProvider : DefaultAuthorizationPolicyProvider
{
    public AuthorizationPolicyProvider(IOptions<AuthorizationOptions> options)
        : base(options)
    {
    }

    public override Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (!policyName.StartsWith(PermissionAuthorizeAttribute.PolicyPrefix, StringComparison.OrdinalIgnoreCase))
        {
            return base.GetPolicyAsync(policyName);
        }

        var permissionNames = policyName.Substring(PermissionAuthorizeAttribute.PolicyPrefix.Length).Split(',');

        var policy = new AuthorizationPolicyBuilder()
            .RequireClaim(CustomClaimTypes.Permission, permissionNames)
            .Build();

        return Task.FromResult(policy);
    }
}
```

在这个实现中，GetPolicyAsync 负责根据 policyName 查找并返回一个策略。但是，我们可以通过覆盖策略并使用 AuthorizationPolicyBuilder 的实例来自动化定义策略的过程。在 GetPolicyAsync 方法的主体中，首先检查收到的 policyName 是否以“PERMISSION:”开头；然后用'，'字符分割 policyName 以检索权限名称。最后，用检索到的权限定义策略并返回它。

现在，要用 default registered 替换这个实现，请在启动时使用以下代码:

```
services.AddSingleton<IAuthorizationPolicyProvider, AuthorizationPolicyProvider>();
```

# 实现 PermissionAuthorizeAttribute

最后一步，我们需要实现自定义 AuthorizeAttribute 来操作策略属性，并将权限名称作为逗号分隔的字符串存储在该属性中。

```
public class PermissionAuthorizeAttribute : AuthorizeAttribute
{
    internal const string PolicyPrefix = "PERMISSION:";/// <summary>
    /// Creates a new instance of <see cref="AuthorizeAttribute"/> class.
    /// </summary>
    /// <param name="permissions">A list of permissions to authorize</param>
    public PermissionAuthorizeAttribute(params string[] permissions)
    {
        Policy = $"{PolicyPrefix}{string.Join(",", permissions)}";
    }
}
```

要使用它:

```
[PermissionAuthorize(PermissionNames.Projects_View)]
public IActionResult Get(FilteredQueryModel query)
{
   //...
}
[PermissionAuthorize(PermissionNames.Projects_Create)]
public IActionResult Post(ProjectModel model)
{
   //...
}
```

# 结论

使用本文中介绍的方法，您可以简单地在 ASP.NET 核心项目中应用动态许可授权，并且只需进行最少的定制。

此外，您可以在 DNTFrameworkCore 存储库中找到完整的实现[。](https://github.com/rabbal/DNTFrameworkCore/tree/master/src/DNTFrameworkCore.Web/Authorization)