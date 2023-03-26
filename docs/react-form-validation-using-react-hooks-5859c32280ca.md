# 使用 React 挂钩进行 React 表单验证

> 原文：<https://medium.com/hackernoon/react-form-validation-using-react-hooks-5859c32280ca>

![](img/58cf628218cbfe970a38e309514b1126.png)

Photo by [Efe Kurnaz](https://unsplash.com/@efekurnaz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/hooks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[**React 16.8**](https://reactjs.org/docs/hooks-intro.html) 更新在他们的库中引入了一个新特性，叫做**钩子**。Hooks 是 React 库中最具革命性的更新之一。它给了你更多的灵活性来编写有状态的组件，而不用编写类。俗话说“**写得更少，做得更多**”在 React 应用程序中，您将避免许多样板代码。

所以在这篇文章中，我将和你分享我是如何使用 React 钩子创建自己的表单验证的。

在我们开始之前，可以在我的 [GitHub 概要文件](https://github.com/llauderesv/react-form-validation)中下载完整的资源库。如果你有什么要分享或评论的，请随意修改代码。

好的，首先让我们创建一个名为 **useForm.js** 的文件。在这个文件中，我们应该放置表单验证挂钩来分离关注点。我们遵循 React 库中的惯例，即每个使用钩子的功能组件都以**“use”**关键字开头，后跟它们的用途，这样就可以很容易地确定它们能做什么。

在这个文件中，让我们首先导入 React 库中需要的所有钩子。

```
import { useState, useEffect, useCallback } from 'react';
```

之后，让我们创建一个名为**的函数，使用表单**，有 3 个参数。

```
import { useState, useEffect, useCallback } from 'react';

function useForm(stateSchema, validationSchema = {}, callback) {
  const [state, setState] = useState(stateSchema);
  const [disable, setDisable] = useState(true);
  const [isDirty, setIsDirty] = useState(false);

  // Disable button in initial render.
  useEffect(() => {
    setDisable(true);
  }, []);

  // For every changed in our state this will be fired
  // To be able to disable the button
  useEffect(() => {
    if (isDirty) {
      setDisable(validateState());
    }
  }, [state, isDirty]);

  // Used to disable submit button if there's an error in state
  // or the required field in state has no value.
  // Wrapped in useCallback to cached the function to avoid intensive memory leaked
  // in every re-render in component
  const validateState = useCallback(() => {
    const hasErrorInState = Object.keys(validationSchema).some(key => {
      const isInputFieldRequired = validationSchema[key].required;
      const stateValue = state[key].value; // state value
      const stateError = state[key].error; // state error

      return (isInputFieldRequired && !stateValue) || stateError;
    });

    return hasErrorInState;
  }, [state, validationSchema]);

  // Used to handle every changes in every input
  const handleOnChange = useCallback(
    event => {
      setIsDirty(true);

      const name = event.target.name;
      const value = event.target.value;

      let error = '';
      if (validationSchema[name].required) {
        if (!value) {
          error = 'This is required field.';
        }
      }

      if (
        validationSchema[name].validator !== null &&
        typeof validationSchema[name].validator === 'object'
      ) {
        if (value && !validationSchema[name].validator.regEx.test(value)) {
          error = validationSchema[name].validator.error;
        }
      }

      setState(prevState => ({
        ...prevState,
        [name]: { value, error },
      }));
    },
    [validationSchema]
  );

  const handleOnSubmit = useCallback(
    event => {
      event.preventDefault();

      // Make sure that validateState returns false
      // Before calling the submit callback function
      if (!validateState()) {
        callback(state);
      }
    },
    [state]
  );

  return { state, disable, handleOnChange, handleOnSubmit };
}

export default useForm;
```

我们的 useForm 函数中的第一个参数是 **stateSchema** 。在这个参数中，我们应该在表单中定义我们的状态。

**示例:**

```
 // Define your state schema
  const stateSchema = {
    fname: { value: '', error: '' },
    lname: { value: '', error: '' },
    tags: { value: '', error: '' },
  };
```

我们的 stateSchema 对象包含了表单中的所有字段。正如你在这里看到的，我们有一个 **fname** 和一个包含 **value** 和 **error 属性的 object value。**值是我们在输入字段中输入的值，错误是我们输入字段中的错误(如果有的话)。

第二个参数是 **validationSchema** 。在这个参数中，我们定义了自己在表单中验证状态的规则。

**例如:**

```
 // Define your validationStateSchema
  // Note: validationStateSchema and stateSchema property
  // should be the same in-order validation works!
  const validationStateSchema = {
    fname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid first name format.',
      },
    },
    lname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid last name format.',
      },
    },
    tags: {
      required: true,
      validator: {
        regEx: /^(,?\w{3,})+$/,
        error: 'Invalid tag format.',
      },
    },
  };
```

正如您已经注意到的，这里的 **stateSchema** 属性和 **validationSchema** 属性是相同的。因为我们需要将 stateSchema 中的验证映射到我们在 stateSchema 中定义的验证字段(例如:fname、lname 和 tags 等)。

第三个参数是我们提交表单时执行的回调函数。通常我们把处理表单提交的 api 放在这里。

```
function onSubmitForm(state) {
  alert(JSON.stringify(state, null, 2));
}
```

我们的 useForm 函数返回值，例如( **state，disable，handleOnChange，handleOnSubmit** )

```
return { state, disable, handleOnChange, handleOnSubmit };
```

**state** 返回表单中当前状态的对象。

**disable** 返回一个布尔值，以便在状态出错时禁用提交按钮

**handleOnChange** 返回一个函数来设置我们表单状态中的值。

**handleOnSubmit** 返回一个函数来提交表单中的值。

所以我们现在已经创建好了我们的 useForm 钩子，现在我们可以把它挂在我们的组件中了。在此之前，让我们首先创建我们的表单组件。

```
import React from 'react';
import useForm from './useForm';function Form() {
  // Define your state schema
  const stateSchema = {
    fname: { value: '', error: '' },
    lname: { value: '', error: '' },
    tags: { value: '', error: '' },
  };// Define your validationStateSchema
  // Note: validationStateSchema and stateSchema property
  // should be the same in-order validation works!
  const validationStateSchema = {
    fname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid first name format.',
      },
    },
    lname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid last name format.',
      },
    },
    tags: {
      required: true,
      validator: {
        regEx: /^(,?\w{3,})+$/,
        error: 'Invalid tag format.',
      },
    },
  };function onSubmitForm(state) {
    alert(JSON.stringify(state, null, 2));
  }const { state, handleOnChange, handleOnSubmit, disable } = useForm(
    stateSchema,
    validationStateSchema,
    onSubmitForm
  );const errorStyle = {
    color: 'red',
    fontSize: '13px',
  };return (
    <div>
      <form onSubmit={handleOnSubmit}>
        <div>
          <label htmlFor="fname">
            First name:
            <input
              type="text"
              name="fname"
              value={state.fname.value}
              onChange={handleOnChange}
            />
          </label>
          {state.fname.error && <p style={errorStyle}>{state.fname.error}</p>}
        </div><div>
          <label htmlFor="lname">
            Last name:
            <input
              type="text"
              name="lname"
              value={state.lname.value}
              onChange={handleOnChange}
            />
          </label>
          {state.lname.error && <p style={errorStyle}>{state.lname.error}</p>}
        </div><div>
          <label htmlFor="tags">
            Tags:
            <input
              type="text"
              name="tags"
              value={state.tags.value}
              onChange={handleOnChange}
            />
          </label>
          {state.tags.error && <p style={errorStyle}>{state.tags.error}</p>}
        </div><input type="submit" name="submit" disabled={disable} />
      </form>
    </div>
  );
}export default Form;
```

在表单组件中，我们使用了之前创建的名为 useForm 的钩子，并传递了所有必需的参数来使用它。

我们的状态模式:

```
 // Define your state schema
  const stateSchema = {
    fname: { value: '', error: '' },
    lname: { value: '', error: '' },
    tags: { value: '', error: '' },
  };
```

**我们的验证方案:**

```
// Define your validationStateSchema
  // Note: validationStateSchema and stateSchema property
  // should be the same in-order validation works!
  const validationStateSchema = {
    fname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid first name format.',
      },
    },
    lname: {
      required: true,
      validator: {
        regEx: /^[a-zA-Z]+$/,
        error: 'Invalid last name format.',
      },
    },
    tags: {
      required: true,
      validator: {
        regEx: /^(,?\w{3,})+$/,
        error: 'Invalid tag format.',
      },
    },
  };
```

我们在提交表单时的回调函数:

```
function onSubmitForm(state) {
    alert(JSON.stringify(state, null, 2));
}
```

传递了 useForm 中所有必需的参数以使用它。

```
const { state, handleOnChange, handleOnSubmit, disable } = useForm(
    stateSchema,
    validationStateSchema,
    onSubmitForm
);
```

记住我们的 useForm 钩子返回一个值，比如 **state，handleOnChange，handleOnSubmit，disable。**

现在，我们可以在表单组件中使用它了。

```
return (
    <div>
      <form onSubmit={handleOnSubmit}>
        <div>
          <label htmlFor="fname">
            First name:
            <input
              type="text"
              name="fname"
              value={state.fname.value}
              onChange={handleOnChange}
            />
          </label>
          {state.fname.error && <p style={errorStyle}>{state.fname.error}</p>}
        </div><div>
          <label htmlFor="lname">
            Last name:
            <input
              type="text"
              name="lname"
              value={state.lname.value}
              onChange={handleOnChange}
            />
          </label>
          {state.lname.error && <p style={errorStyle}>{state.lname.error}</p>}
        </div><div>
          <label htmlFor="tags">
            Tags:
            <input
              type="text"
              name="tags"
              value={state.tags.value}
              onChange={handleOnChange}
            />
          </label>
          {state.tags.error && <p style={errorStyle}>{state.tags.error}</p>}
        </div><input type="submit" name="submit" disabled={disable} />
      </form>
    </div>
  );
}
```

之后，让我们在 App.js 中导入表单组件。

```
import React from 'react';
import Form from './Form';import logo from './logo.svg';
import './App.css';function App() {
  return (
    <div className="App">
      <div className="App-header">
        <img className="App-logo" src={logo} alt="react-logo" />
        <p>React Form Validation using React Hooks.</p>
      </div>
      <Form />
    </div>
  );
}export default App;
```

这是我们用表单组件创建的 useForm 挂钩的最终输出。

using useForm hooks in Form component

希望有帮助😃

如果你喜欢读这篇文章，请为我鼓掌👏 👏 👏

谢谢你。

# “不要做一个 JavaScript 庸才。”

在推特上关注我[***https://twitter.com/llaudevc/***](https://twitter.com/llaudevc/followers)