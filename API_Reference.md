
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

** "forwardedAs" prop **  `v4.3`

如果您选择使用样式化() HOC 包装另一个组件，该组件也接受`“ as”`道具，那么使用`“ forwardedAs”`将所需的prop传递给包装的组件

** Transient props **  `v4.3`






----------------
