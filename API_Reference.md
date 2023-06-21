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











----------------
