# API 参考

## Primary

`styled`

这是默认的导出，用来创建 `styled.tagname` 底层辅助方法.

|  参数   | 描述  | 
| :---:  | :---: | 
| component / tagname  | 要么是一个有效的react组件，要么是一个类似`“ div”`的标签名 |

返回一个函数，该函数接收一个标记的模板字面，并将其变成一个`StyledComponent`

```jsx
// import styled from 'styled-components'

const Button = styled.button`
  background: #BF4F74;
  border-radius: 3px;
  border: none;
  color: white;
`

const TomatoButton = styled(Button)`
  background: tomato;
`

render(
  <>
    <Button>I'm purple.</Button>
    <br />
    <TomatoButton>I'm red.</TomatoButton>
  </>
)
```
你可以在[<u>入门部分</u>](https://styled-components.com/docs/basics#getting-started)看到这个方法的介绍

`标签模板`

这就是您传递给样式化调用的内容带标记的模板文本。这是 ES6语言的一个特性。您可以在[<u>“标记模板文字”</u>](https://styled-components.com/docs/advanced#tagged-template-literals)部分了解更多关于它们的信息.

|  输入   | 描述  | 
| :---:  | :---: | 
| `Rule`  | 任何 CSS 规则(string) |
| `插补`  | 它既可以是字符串也可以是函数。字符串按原样与规则组合。函数将接收样式化组件的道具作为第一个也是唯一的参数 |

阅读更多关于如何根据props调整style的信息，请参见根据[<u>props<u/>](https://styled-components.com/docs/basics#adapting-based-on-props)调整style部分。

传递到内部函数中的属性附加了一个特殊的属性，`主题`，该属性由更高级别的 `ThemeProvider` 组件注入。有关这方面的更多信息，请查看[<u>主题部分</u>](https://styled-components.com/docs/advanced#theming)
```jsx
// import styled from 'styled-components'

const padding = '3em'

const Section = styled.section<{ $background?: string; }>`
  color: white;

  /* Pass variables as inputs */
  padding: ${padding};

  /* Adjust the background from the properties */
  background: ${props => props.$background};
`

render(
  <Section $background="royalblue">
    ✨ Magic
  </Section>
)
```
你也可以直接从插值或输入对象中返回对象，它们将被视为内联样式。然而，这是很不可取的，因为CSS语法支持伪选择器、媒体查询、嵌套等，而对象语法则不支持.

`风格化组件`

一个有风格的React组件。当你调用styled.tagname或styled(Component)的样式时，会返回。

这个组件可以接受任何prop。如果它是一个有效的属性，它会把它传给HTML节点，否则它只把它传给插值函数。(见标签模板字面)

你可以毫无问题地将一个任意的类名传递给一个风格化组件，它将被应用在风格化调用所定义的样式旁边。(例如：<MyStyledComp className="bootstrap__btn" />)

**.attrs** 

这是一个可链接的方法，附加一些props到一个样式化的组件。第一个也是唯一的参数是一个对象，它将被合并到组件的其余道具中。`attrs`对象接受以下值:
|  值   | 描述  | 
| :---:  | :---: | 
| `Prop值`  | 它们可以是任何类型的，除了函数。它们将保持静态，并将合并到现有的组件props中 |
| `Prop工厂`  | 一个函数接收传入组件的props并计算出一个值，然后将其合并到现有的组件props中 |

返回另一个`风格化组件`

``jsx
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

`"as"` 多态 prop








----------------
