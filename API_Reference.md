# API参考
## **主要的**

**.attrs** 

这是一个可链接的方法，附加一些props到一个样式化的组件。第一个也是唯一的参数是一个对象，它将被合并到组件的其余道具中。`attrs`对象接受以下值:
|  值   | 描述  | 
| :---:  | :---: | 
| `Prop值`  | 它们可以是任何类型的，除了函数。它们将保持静态，并将合并到现有的组件props中 |
| `Prop工厂`  | 一个函数接收传入组件的props并计算出一个值，然后将其合并到现有的组件props中 |

返回另一个`风格化组件`

```jsx
// import styled from 'styled-components'

const Input = styled.input.attrs(props => ({
  type: 'text',
  size: props.$small ? 5 : undefined,
}))<{ $padding?: string; $small?: boolean; }>`
  border-radius: 3px;
  border: 1px solid #BF4F74;
  display: block;
  margin: 0 0 1em;
  padding: ${props => props.$padding};

  ::placeholder {
    color: #BF4F74;
  }
`

render(
  <>
    <Input $small placeholder="Small" />
    <Input placeholder="Normal" />
    <Input $padding="2em" placeholder="Padded" />
  </>
)
```

在[<u>附加额外props<u/>](https://styled-components.com/docs/basics#attaching-additional-props)部分了解更多关于这个构造函数的信息

`"as"` 多态 prop `v4`

如果您希望保留所有样式应用到组件，但只是切换最终呈现的内容(不管是不同的 HTML 标记还是不同的自定义组件) ，那么您可以在运行时使用`“ as”`道具来实现这一点

```jsx
// import styled from "styled-components";

 Component = styled.div
  color: red;
`;

render(
  <Component
    as="button"

  >
    Hello World!
  </Component>
)

```

这种东西在像导航栏这样的用例中非常有用，其中一些项目应该是连接，一些仅仅是按钮，但是所有的样式都是相同的.

**"forwardedAs" prop**  `v4.3`

如果您选择使用样式化() HOC 包装另一个组件，该组件也接受`“ as”`道具，那么使用`“ forwardedAs”`将所需的prop传递给包装的组件

**临时props**  `v4.3`

可以通过在属性名称前加上美元符号 $ 来将其变为瞬态 prop，从而防止被 styled components 消耗的 prop 被传递到底层的 React 节点或渲染到 DOM 元素中。

在这个示例中，$draggable不像draggable那样被渲染到DOM中。

```jsx
const Comp = styled.div`
  color: ${props =>
    props.$draggable || 'black'};
`;

render(
  <Comp $draggable="red" draggable="true">
    Drag me!
  </Comp>
);
```

在这个示例中，$draggable 属性被设置为 "red"，但它并不会被传递给底层的 <div> 元素。相反，它被 styled-components 消耗并用于设置文本颜色。因此，文本颜色将是红色。

而 draggable="true" 属性是一个普通的 prop，并没有被标记为瞬态 prop，因此它会被传递到底层的 <div> 元素中。



**shouldForwardProp** `v5.1`

这是一种比临时props更动态、细粒度的过滤机制。在多个高阶组件被组合在一起并且它们恰好共享相同的prop名称的情况下，这种机制非常方便。`shouldForwardProp`的工作方式类似于`Array.filter`
的谓词回调函数。未通过测试的prop不会传递到底层组件中，就像临时prop一样。

请记住，就像在这个示例中一样，其他可链式调用的方法应该在`.withConfig`之后执行。

示例代码如下：
```jsx
const Comp = styled('div').withConfig({
  shouldForwardProp: (prop) =>
      !['hidden'].includes(prop),
}).attrs({ className: 'foo' })`
  color: red;
  &.foo {
    text-decoration: underline;
  }
`;

render(
  <Comp hidden>
    Drag Me!
  </Comp>
);
```

在示例代码中，无论传递 'hidden' 属性还是没有传递任何属性，效果都是一样的，都会显示红色文本颜色和下划线文本装饰的 `<div>` 元素。这是因为 `shouldForwardProp` 函数只起到了过滤数据的作用，而并没有影响最终的渲染结果。

实际上，两次运行结果相同的原因是使用了 `.attrs` 方法。该方法为组件添加属性，并将其应用于底层的 `<div>` 元素。在示例代码中，通过 `.attrs({ className: 'foo' })` 将一个名为 'foo' 的类名属性添加到了组件上。

因此，无论是否传递 'hidden' 属性，都会显示红色文本颜色和下划线文本装饰的 `<div>` 元素。这是由于样式定义中的颜色设置和 'foo' 类名的样式规则都会应用于底层的 `<div>` 元素。

要注意的是，`shouldForwardProp` 函数可以用于过滤不希望传递给底层组件的属性，但在这个特定示例中，它并没有影响最终的渲染结果。真正影响渲染结果的是定义的样式和添加的属性（通过 `.attrs` 方法）。

> 可选地，`shouldForwardProp` 可以接受第二个参数，该参数提供对默认验证器函数的访问。该函数可以用作备用选项，并且它也像谓词一样工作，基于已知的 HTML 属性进行过滤。

**`ThemeProvider`**

一个用于主题设计的辅助组件。通过上下文API，将主题注入到组件树中它下面的所有样式的组件中。查看[<u>主题设计<u/>](https://styled-components.com/docs/advanced#theming)部分。
|  props   | 描述  | 
| :---:  | :---: | 
| `theme`  | 一个对象，它将作为主题被注入到提供者下面的风格化组件的所有插值中 |

```jsx
// import styled, { ThemeProvider } from 'styled-components'

const Box = styled.div`
  color: ${props => props.theme.color};
`

render(
  <ThemeProvider theme={{ color: 'mediumseagreen' }}>
    <Box>I'm mediumseagreen!</Box>
  </ThemeProvider>
)
```
**`CSS` prop**  `v4`

有时你不想为了应用一点样式而创建一个额外的组件。`css` prop是一种方便的方式，可以在你的组件上进行迭代，而不需要解决固定的组件边界。它既适用于普通的HTML标签，也适用于组件，并支持任何风格化组件所支持的一切，包括基于prop、主题和自定义组件的适应。

>NOTE
>要启用对`css`道具的支持，你必须使用[<u>Babel插件<u/>](https://styled-components.com/docs/tooling#babel-plugin)

```jsx
<div
  css={`
    background: papayawhip;
    color: ${props => props.theme.colors.text};
  `}
/>
<Button
  css="padding: 0.5em 1em;"
/>
```

在css引擎下，Babel插件将任何带有`css`道具的元素变成一个样式化的组件。例如，上面的代码变成:
```jsx
import styled from 'styled-components';


const StyledDiv = styled.div`
  background: papayawhip;
  color: ${props => props.theme.colors.text};
`


const StyledButton = styled(Button)`
  padding: 0.5em 1em;
`


<StyledDiv />
<StyledButton />
```
请注意，你甚至不需要添加导入，Babel插件会自动完成！（除非使用Babel macro，见下文）！

**使用Babel macro**

你可以在`create-react-app`中使用[<u>Babel macro<u/>](https://styled-components.com/docs/tooling#babel-macro)来实现这个功能。不幸的是，Babel macro 只在导入时运行，所以**导入不能自动添加**。如果你手动添加导入到macro中，上面的代码可以完美地工作:

```jsx
import styled from 'styled-components/macro'


<div
  css={`
    background: papayawhip;
    color: ${props => props.theme.colors.text};
  `}
/>
<Button
  css="padding: 0.5em 1em;"
/>
```

**使用TypeScript**
为了防止任意元素的`css`prop出现TypeScript错误，请安装`@types/styled-components`，并在你的项目中添加以下导入内容一次:
```jsx
import {} from 'styled-components/cssprop'
```
参考 (https://github.com/DefinitelyTyped/DefinitelyTyped/issues/31245#issuecomment-446011384 ) 获取更多信息


## Helpers
**`createGlobalStyle`** `v4`
一个辅助函数，用于生成一个特殊的`StyledComponent`，处理全局样式。通常情况下，有样式的组件会被自动地划分到一个本地的CSS类，因此与其他组件隔离。在`createGlobalStyle`的情况下，这个限制被移除，像CSS重置或基本样式表的东西可以被应用。

|  参数   | 描述  | 
| :---:  | :---: | 
| 标记模板  | 一个带有你的CSS和插值的标记的模板文字。|

返回一个不接受子代的`StyledComponent`。把它放在React树的顶端，全局样式将在组件被 "渲染 "时被注入。

```jsx
import { createGlobalStyle } from 'styled-components'


const GlobalStyle = createGlobalStyle<{ $whiteColor?: boolean; }>`
  body {
    color: ${props => (props.$whiteColor ? 'white' : 'black')};
  }
`


// later in your app


<React.Fragment>
  <GlobalStyle $whiteColor />
  <Navigation /> {/* example of other top-level stuff */}
</React.Fragment>
```
由于`GlobalStyle`组件是一个风格化的组件，意味着如果提供的话它也可以从[<u>`<ThemeProvider>`<u/>](https://styled-components.com/docs/api#themeprovider)组件获得主题

```jsx
import { createGlobalStyle, ThemeProvider } from 'styled-components'


const GlobalStyle = createGlobalStyle<{ $whiteColor?: boolean; }>`
  body {
    color: ${props => (props.$whiteColor ? 'white' : 'black')};
    font-family: ${props => props.theme.fontFamily};
  }
`


// later in your app


<ThemeProvider theme={{ fontFamily: 'Helvetica Neue' }}>
  <React.Fragment>
    <Navigation /> {/* example of other top-level stuff */}
    <GlobalStyle $whiteColor />
  </React.Fragment>
</ThemeProvider>
```

**`css`**

一个辅助函数，用于从一个带有插值的模板字头中生成CSS。如果你返回一个带有插值功能的模板字，因为在JavaScript中标签模板字的工作方式,你需要使用这个函数.

如果你插值的是一个字符串，你不需要使用这个，只有当你插值的是一个函数时才需要使用这个。

|  参数   | 描述  | 
| :---:  | :---: | 
| 标记模板  | 一个带有你的CSS和插值的标记的模板文字。|

Returns an array of interpolations, which is a flattened data structure that you can pass as an interpolation itself.
返回一个插值数组，它是一个扁平化的数据结构，你可以把它作为插值本身来传递。

```jsx
import styled, { css } from 'styled-components'


interface ComponentProps {
  $complex?: boolean;
  $whiteColor?: boolean;
}


const complexMixin = css<ComponentProps>`
  color: ${props => (props.$whiteColor ? 'white' : 'black')};
`


const StyledComp = styled.div<ComponentProps>`
  /* This is an example of a nested interpolation */
  ${props => (props.$complex ? complexMixin : 'color: blue;')};
`
```
如果你不写css，你的函数就会变成`toString()`，你就不会得到你所期望的结果。

**`keyframes`**
一个为动画创建关键帧的辅助方法
|  参数   | 描述  | 
| :---:  | :---: | 
| 标记模板  | 一个有标签的模板字面，里面有你的关键帧|

返回一个`keyframes`模型，在你的动画声明中使用。如果你想获得生成的动画名称，你可以在返回的模型上使用`getName()`API。

>NOTE
>在styled-components v3及以下版本中，`keyframes`帮助器直接返回动画名称，而不是用`getName`方法返回一个对象。

```jsx
import styled, { keyframes } from 'styled-components'


const fadeIn = keyframes`
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
`


const FadeInButton = styled.button`
  animation: 1s ${fadeIn} ease-out;
`
```

如果你把你的风格规则组成一个局部，确保使用`css`帮助器。

```jsx
import styled, { css, keyframes } from 'styled-components'


interface AnimationProps {
  $animationLength: number;
}


const pulse = keyframes`
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
`


const animation = props =>
  css<AnimationProps>`
    ${pulse} ${props.$animationLength} infinite alternate;
  `


const PulseButton = styled.button<AnimationProps>`
  animation: ${animation};
`
```
你可以在[<u>动画<u/>](https://styled-components.com/docs/basics#animations)部分了解更多关于使用风格化组件的动画的信息。





----------------
