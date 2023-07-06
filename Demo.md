
# style-commponments 案例

1. ## 如何使用媒体查询@media实现响应式布局

 在 styled-components 中，你可以使用 `css` 函数结合 `@media` 媒体查询来实现响应式布局。下面是一个示例代码，展示了如何在 styled-components 中使用媒体查询：

```jsx
import styled, { css } from 'styled-components';

const Container = styled.div`
  width: 100%;
  height: 100%;

  /* 响应式布局 */
  @media (max-width: 768px) {
    background-color: red;
  }

  @media (min-width: 769px) and (max-width: 1024px) {
    background-color: green;
  }

  @media (min-width: 1025px) {
    background-color: blue;
  }
`;

const App = () => {
  return <Container>Content</Container>;
};
```

 在上述代码中，我们定义了一个名为 `Container` 的组件，它是一个 `div` 元素，具有全屏的宽度和高度。然后，我们使用 `@media` 媒体查询在不同的屏幕宽度范围内为 `Container` 组件设置不同的背景颜色。

 在示例中，当屏幕宽度小于等于 768px 时，背景颜色为红色；当屏幕宽度介于 769px 和 1024px 之间时，背景颜色为绿色；当屏幕宽度大于等于 1025px 时，背景颜色为蓝色。

 你可以根据自己的需求，在媒体查询中设置不同的样式规则，以实现响应式布局。
