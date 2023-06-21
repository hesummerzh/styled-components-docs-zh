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
你可以在<u>入门部分</u>(https://styled-components.com/docs/basics#getting-started)看到这个方法的介绍

`标签模板`

这就是您传递给样式化调用的内容——带标记的模板文本。这是 ES6语言的一个特性。您可以在<u>“标记模板文字”</u>(https://styled-components.com/docs/advanced#tagged-template-literals)部分了解更多关于它们的信息


----------------
