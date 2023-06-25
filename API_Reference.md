
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



----------------
