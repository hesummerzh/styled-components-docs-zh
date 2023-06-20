
# 基础

## 动机<div id="Motivation"/>

styled-components是我们思考如何加强CSS在React组件系统中的造型的结果。通过专注于一个单一的用例，我们成功地优化了开发人员的体验以及终端用户的输出.

除了为开发者提供更好的体验外，风格化组件还提供：

- **Automatic critical CSS:** styled-components可以跟踪哪些组件在页面上被渲染，并且完全自动地注入它们的样式，而不是其他。结合代码拆分，这意味着你的用户加载最少的必要代码.
- **解决了 class name 冲突:** styled-components为你的样式生成了独特的类名。你永远不必担心重复、重叠或拼写错误的问题.
- **更容易删除CSS:** 你可能很难知道一个类名是否在你的代码库中的某个地方被使用。styled-components让它变得很明显，因为每一点样式都与一个特定的组件相联系。如果该组件未被使用（工具可以检测到）并被删除，它的所有样式也会被删除.
- **简单的动态样式:** 根据一个组件的道具或全局主题来调整其造型是简单而直观的，而不必手动管理几十个类.
- **无痛维护:** 你不必在不同的文件中寻找影响你的组件的样式，所以无论你的代码库有多大，维护都是小菜一碟。
- **自动提供前缀:** 按照当前的标准编写你的CSS，剩下的就交给styled-components处理.

通过 styled-components 绑定样式到组件,开发者可以在编写熟知的 CSS 同时也获得上述全部的益处.

## 安装<div id="Installation"/>

从 npm 安装 styled-components :
```
npm install --save styled-components
```

>强烈推荐使用 styled-components 的 [babel 插件](https://www.styled-components.com/docs/tooling#babel-plugin) (当然这不是必须的).它提供了许多益处,比如更清晰的类名,SSR 兼容性,更小的包等等.

如果没有使用模块管理工具或者包管理工具,也可以使用官方托管在 unpkg CDN 上的构建版本.只需在HTML文件底部添加以下`<script>`标签:
```html
<script src="https://unpkg.com/styled-components/dist/styled-components.min.js"></script>
```
添加 styled-components 之后就可以访问全局的 `window.styled` 变量.
```javascript
const Component = window.styled.div`
  color: red;
`
```
>注意
>
>这用使用方式需要页面在 styled-components script 之前引入 [react CDN bundles](https://reactjs.org/docs/cdn-links.html) 

## 入门<div id="GettingStarted"/>

`styled-components` 通过标记的模板字符来设置组件样式.

它删除了组件和样式之间的映射。这意味着，当你定义你的样式时，你实际上是在创建一个正常的React组件，并将你的样式附加到它上面.

以下的例子创建了两个简单的附加了样式的组件, 一个`Wrapper`和一个`Title`:

```jsx
// 创建一个 Title 组件,它将渲染一个附加了样式的 <h1> 标签
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// 创建一个 Wrapper 组件,它将渲染一个附加了样式的 <section> 标签
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// 就像使用常规 React 组件一样使用 Title 和 Wrapper 
render(
  <Wrapper>
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```

>注意
>
> styled-components 会为我们自动创建 CSS 前缀

## 基于属性的适配
我们可以将 props 以插值的方式传递给`styled component`,以调整组件样式.

下面这个 `Button` 组件持有一个可以改变`color`的`primary`属性. 将其设置为 ture 时,组件的`background-color`和`color`会交换.

```jsx
const Button = styled.button`
  /* Adapt the colors based on primary prop */
  background: ${props => props.primary ? "palevioletred" : "white"};
  color: ${props => props.primary ? "white" : "palevioletred"};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

render(
  <div>
    <Button>Normal</Button>
    <Button primary>Primary</Button>
  </div>
);
```

## 样式继承
很多时候，你可能想使用一个组件，但为了一个单独的案例而稍微改变它。现在，你可以传入一个插值函数，根据一些道具来改变它们，但这对于重写一次样式来说是相当费力的。

要轻松地制作一个继承了另一个组件的样式的新组件，只需用styled()构造函数将其包裹起来。在这里，我们使用上一节中的按钮，并创建一个特殊的按钮，用一些与颜色有关的样式来扩展它:

```jsx
// 上一节创建的没有插值的 Button 组件
const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// 一个继承 Button 的新组件, 重载了一部分样式
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);
```

我们可以看到，新的TomatoButton仍然类似于Button，而我们只增加了两条新的规则。

在某些情况下，你可能想改变风格化组件渲染的标签或组件。例如，这在构建导航栏时很常见，在导航栏中混合了锚链接和按钮，但它们的样式应该是相同的.

在这种情况下,我们也有替代办法(escape hatch). 我们可以使用多态 ["as" polymorphic prop](https://www.styled-components.com/docs/api#as-polymorphic-prop) 动态的在不改变样式的情况下改变元素:

```jsx
const Button = styled.button`
  display: inline-block;
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <Button as="a" href="/">Link with Button styles</Button>
    <TomatoButton as="a" href="/">Link with Tomato Button styles</TomatoButton>
  </div>
);
```

这在自定义组件中也能完全正常工作:
```jsx
const Button = styled.button`
  display: inline-block;
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

const ReversedButton = props => <button {...props} children={props.children.split('').reverse()} />

render(
  <div>
    <Button>Normal Button</Button>
    <Button as={ReversedButton}>Custom Button with Normal Button styles</Button>
  </div>
);
```

## 给任何组件添加样式
`styled`方法在你自己的或任何第三方的组件上都能完美地工作，只要他们将传递的className道具附加到DOM元素上即可.
> 注意
>
>在 react-native 中，请使用 style 而不是 className.

```jsx
// 下面是给 react-router-dom  Link 组件添加样式的示例
const Link = ({ className, children }) => (
  <a className={className}>
    {children}
  </a>
);

const StyledLink = styled(Link)`
  color: palevioletred;
  font-weight: bold;
`;

render(
  <div>
    <Link>Unstyled, boring Link</Link>
    <br />
    <StyledLink>Styled, exciting Link</StyledLink>
  </div>
);
```

>注意
>
>也可以传递标签给`styled()`, 比如:` styled("div")`. 实际上`styled.tagname`的方式就是 styled(tagname)`的别名.

## 属性传递
如果添加样式的目标是 DOM 元素 (如`styled.div`), `styled-components `会传递已知的 HTML 属性给 DOM. 如果是一个自定义的 React 组件 (如`styled(MyComponent)`), `styled-components` 会传递全部 `props`.

以下示例展示Input组件的所有props是如何被传递到被安装的DOM节点的，就像React元素一样.

```jsx
// 创建一个给<input>标签添加若干样式的 Input 组件 
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: ${props => props.inputColor || "palevioletred"};
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

// 渲染两个样式化的 text input,一个标准颜色,一个自定义颜色
render(
  <div>
    <Input defaultValue="@probablyup" type="text" />
    <Input defaultValue="@geelen" type="text" inputColor="rebeccapurple" />
  </div>
);
```

注意, `inputColor prop`并没有被传递给 DOM, 但是`type`和`defaultValue` 都被传递了. `styled-components`足够智能,会自动过滤掉所有非标准 attribute.

<div id="ComingfromCSS">

## 来自CSS

### styled-components 如何在组件中工作?
如果你熟悉在组件中导入 CSS(例如 CSSModules),那么下面的写法你一定不陌生:
```jsx
import React from 'react'
import styles from './styles.css'

export default class Counter extends React.Component {
  state = { count: 0 }

  increment = () => this.setState({ count: this.state.count + 1 })
  decrement = () => this.setState({ count: this.state.count - 1 })

  render() {
    return (
      <div className={styles.counter}>
        <p className={styles.paragraph}>{this.state.count}</p>
        <button className={styles.button} onClick={this.increment}>
          +
        </button>
        <button className={styles.button} onClick={this.decrement}>
          -
        </button>
      </div>
    )
  }
}
```
由于 Styled Component 是 HTML 元素和作用在元素上的样式规则的组合, 我们可以这样编写`Counter`:
```jsx
import React from 'react'
import styled from 'styled-components'

const StyledCounter = styled.div`
  /* ... */
`
const Paragraph = styled.p`
  /* ... */
`
const Button = styled.button`
  /* ... */
`

export default class Counter extends React.Component {
  state = { count: 0 }

  increment = () => this.setState({ count: this.state.count + 1 })
  decrement = () => this.setState({ count: this.state.count - 1 })

  render() {
    return (
      <StyledCounter>
        <Paragraph>{this.state.count}</Paragraph>
        <Button onClick={this.increment}>+</Button>
        <Button onClick={this.decrement}>-</Button>
      </StyledCounter>
    )
  }
}
```

注意,我们在`StyledCounter`添加了"Styled"前缀,这样组件`Counter` 和`StyledCounter` 不会明明冲突,而且可以在 React Developer Tools 和 Web Inspector 中轻松识别.

### 在 render 方法之外定义 Styled Components 

在渲染方法之外定义你的样式化组件是很重要的，否则它将在每一次的渲染过程中被重新创建。在渲染方法中定义样式化组件会阻碍缓存并大大降低渲染速度，因此应该避免这样做.

推荐通过以下方式创建 styled components :
```jsx
const StyledWrapper = styled.div`
  /* ... */
`

const Wrapper = ({ message }) => {
  return <StyledWrapper>{message}</StyledWrapper>
}
```
而不是:
```jsx
const Wrapper = ({ message }) => {
  // WARNING: 别这么干,会很慢!!!
  const StyledWrapper = styled.div`
    /* ... */
  `

  return <StyledWrapper>{message}</StyledWrapper>
}
```

**推荐阅读**:[Talia Marcassa](https://twitter.com/talialongname) 写了一篇很精彩的有关styled-components实际应用的文章,包含许多实用的见解以及与其它方案的比较[Styled Components: To Use or Not to Use?](https://medium.com/building-crowdriff/styled-components-to-use-or-not-to-use-a6bb4a7ffc21)

### 伪元素,伪类选择器和嵌套
`styled-component` 所使用的预处理器[stylis](https://github.com/thysultan/stylis.js)支持自动嵌套的 scss-like 语法,示例如下:

我们使用的预处理器stylis支持类似scss的语法来自动嵌套样式。
通过这种预处理，stylis支持一些高级选择器模式:

```jsx
const Thing = styled.div`
  color: blue;
`
```
伪元素和伪类无需进一步细化,而是自动附加到了组件:
```jsx
const Thing = styled.button`
  color: blue;

  ::before {
    content: '🚀';
  }

  :hover {
    color: red;
  }
`

render(
  <Thing>Hello world!</Thing>
)
```

对于更复杂的选择器,可以使用与号(&)来指向主组件.以下是一些示例:

```jsx
const Thing = styled.div.attrs({ tabIndex: 0 })`
  color: blue;

  &:hover {
    color: red; // <Thing> when hovered
  }

  & ~ & {
    background: tomato; // <Thing> as a sibling of <Thing>, but maybe not directly next to it
  }

  & + & {
    background: lime; // <Thing> next to <Thing>
  }

  &.something {
    background: orange; // <Thing> tagged with an additional CSS class ".something"
  }

  .something-else & {
    border: 1px solid; // <Thing> inside another element labeled ".something-else"
  }
`

render(
  <React.Fragment>
    <Thing>Hello world!</Thing>
    <Thing>How ya doing?</Thing>
    <Thing className="something">The sun is shining...</Thing>
    <div>Pretty nice day today.</div>
    <Thing>Don't you think?</Thing>
    <div className="something-else">
      <Thing>Splendid.</Thing>
    </div>
  </React.Fragment>
)
```

如果只写选择器而不带&,则指向组件的子节点.

```jsx
const Thing = styled.div`
  color: blue;

  .something {
    border: 1px solid; // an element labeled ".something" inside <Thing>
    display: block;
  }
`

render(
  <Thing>
    <label htmlFor="foo-button" className="something">Mystery button</label>
    <button id="foo-button">What do I do?</button>
  </Thing>
)
```
最后,&可以用于增加组件的差异性;在处理混用 styled-components 和纯 CSS 导致的样式冲突时这将会非常有用:

```jsx
const Thing = styled.div`
  && {
    color: blue;
  }
`

const GlobalStyle = createGlobalStyle`
  div${Thing} {
    color: red;
  }
`

render(
  <React.Fragment>
    <GlobalStyle />
    <Thing>
      I'm blue, da ba dee da ba daa
    </Thing>
  </React.Fragment>
)
```

## 附加额外的属性 (v2)
为了避免仅为传递一些props来渲染组件或元素而使用不必要的wrapper, 可以使用 [`.attrs` constructor](https://www.styled-components.com/docs/api#attrs). 通过它可以添加额外的 props 或 attributes 到组件.

举例来说,可以通过这种方式给元素添加静态 props,或者传递第三方 prop 给组件(比如传递`activeClassName`给 React Router 的 `Link`). 此外也可以将dynamic props 添加到组件. `.attrs` 对象也接收函数,返回值也将合并进 props.

示例如下:
```jsx
const Input = styled.input.attrs({
  //  static props
  type: "password",

  //  dynamic props
  margin: props => props.size || "1em",
  padding: props => props.size || "1em"
})`
  color: palevioletred;
  font-size: 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;

  /* dynamically computed props */
  margin: ${props => props.margin};
  padding: ${props => props.padding};
`;

render(
  <div>
    <Input placeholder="A small text input" size="1em" />
    <br />
    <Input placeholder="A bigger text input" size="2em" />
  </div>
);
```

正如所见,我们可以在插值中访问新创建的 props,type attribute也正确的传递给了元素.

## 动画
虽然使用`@keyframes`的 CSS 动画不限于单个组件,但我们仍希望它们不是全局的(以避免冲突). 这就是为什么 styled-components 导出 `keyframes helper` 的原因: 它将生成一个可以在 APP 应用的唯一实例:
```jsx
// Create the keyframes
const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
`;

// Here we create a component that will rotate everything we pass in over two seconds
const Rotate = styled.div`
  display: inline-block;
  animation: ${rotate} 2s linear infinite;
  padding: 2rem 1rem;
  font-size: 1.2rem;
`;
```
>注意
>
>`react-native`不支持 keyframes. 请参考[ReactNative.Animated API](https://stackoverflow.com/questions/50891046/rotate-an-svg-in-react-native/50891225#50891225).

Keyframes are lazily injected when they're used, which is how they can be code-splitted, so you have to use the [`css helper` ](https://www.styled-components.com/docs/api#css)for shared style fragments:

```jsx
const rotate = keyframes``

// ❌ This will throw an error!
const styles = `
  animation: ${rotate} 2s linear infinite;
`;

// ✅ This will work as intended
const styles = css`
  animation: ${rotate} 2s linear infinite;
`
```
>NOTE
>
>This used to work in v3 and below where we didn't code-split keyframes. If you're upgrading from v3, make sure that all your shared style fragments are using the css helper!

## ReactNative
`styled-components` 可以在 React-Native 中以同样的方式使用. 示例:[ Snack by Expo](https://snack.expo.io/@danielmschmidt/styled-components).
```jsx
import React from 'react'
import styled from 'styled-components/native'

const StyledView = styled.View`
  background-color: papayawhip;
`

const StyledText = styled.Text`
  color: palevioletred;
`

class MyReactNativeComponent extends React.Component {
  render() {
    return (
      <StyledView>
        <StyledText>Hello World!</StyledText>
      </StyledView>
    )
  }
}
```

同时也支持复杂样式 (like `transform`)和简写(如 `margin`) 感谢 [css-to-react-native](https://github.com/styled-components/css-to-react-native) !
>注意
>
>`flex`的工作方式类似于 CSS 简写, 而不是 React Native 中的`flex`用法. 设置 `flex: 1` 则会设置 `flexShrink`为1.

Imagine how you'd write the property in React Native, guess how you'd transfer it to CSS, and you're probably right:

```jsx
const RotatedBox = styled.View`
  transform: rotate(90deg);
  text-shadow-offset: 10px 5px;
  font-variant: small-caps;
  margin: 5px 7px 2px;
`
```

与 web-version 不同, React Native 不支持 `keyframes` 和 `createGlobalStyle` .使用媒体查询或是嵌套 CSS 也会报警.

>NOTE
>
>v2 支持百分比. 为了实现这一目标,需要为所有简写强制指定单位. 如果要迁移到v2, [a codemod is available](https://github.com/styled-components/styled-components-native-code-mod).

### Simpler usage with the metro bundler
If you'd prefer to just import `styled-components` instead of `styled-components/native`, you can add a [resolverMainFields configuration](https://facebook.github.io/metro/docs/en/configuration.html#resolver-options) that includes "`react-native`". This used to be supported in metro by default (and currently does work in haul) but appears to have been removed at some point.
