
# style-commponments 案例

1. ## 如何使用媒体查询@media实现响应式布局

 + 在 styled-components 中，你可以使用 `css` 函数结合 `@media` 媒体查询来实现响应式布局。下面是一个示例代码，展示了如何在 styled-components 中使用媒体查询：

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
 
+ 在styled-components中，你可以使用媒体查询（`@media`）来实现响应式布局。styled-components提供了`css`辅助函数，它允许你在样式中使用媒体查询。

以下是使用媒体查询实现响应式布局的示例：

```jsx
import styled, { css } from 'styled-components';

const Container = styled.div`
  width: 100%;

  ${props =>
    props.media &&
    css`
      @media (max-width: 768px) {
        width: 50%;
      }
    `}
`;

const MyComponent = () => {
  return (
    <Container media>
      {/* 响应式布局样式在此应用 */}
      ...
    </Container>
  );
};
```

在上面的示例中，我们定义了一个名为`Container`的styled-component，并在其中使用了媒体查询。在`Container`组件的样式中，我们通过`${props => ...}`语法，根据`props.media`的值来决定是否应用媒体查询样式。

在媒体查询样式中，我们使用`@media`规则，并指定了一个最大宽度为768px的条件。在满足媒体查询条件时，容器的宽度会变为50%。

在使用`Container`组件时，我们通过向`media`属性传递一个真值来触发响应式布局样式的应用。这样，当屏幕宽度小于等于768px时，容器的宽度将变为50%。

通过这种方式，你可以根据不同的屏幕尺寸应用不同的样式，并实现响应式布局。
+ 在 styled-components 中，你可以使用 `css` 功能来应用媒体查询 `@media` 来实现响应式布局。以下是一个示例：

```jsx
import styled, { css } from 'styled-components';

const Container = styled.div`
  width: 100%;

  ${props => props.theme.mediaQueries.desktop`
    width: 960px;
  `}

  ${props => props.theme.mediaQueries.tablet`
    width: 768px;
  `}

  ${props => props.theme.mediaQueries.mobile`
    width: 100%;
  `}
`;
```

在上面的代码中，我们定义了一个名为 `Container` 的 styled-component。它具有一个初始的 `width` 属性设置为 `100%`。然后，我们使用 `${props => props.theme.mediaQueries...}` 的语法来应用不同的媒体查询样式。

在 `props.theme.mediaQueries` 中，`desktop`，`tablet`，`mobile` 是自定义的媒体查询断点。你可以根据自己的需求定义不同的断点。

例如，假设你的主题对象（`props.theme`）中有以下定义：

```jsx
const theme = {
  mediaQueries: {
    desktop: (...args) => css`
      @media (min-width: 1024px) {
        ${css(...args)}
      }
    `,
    tablet: (...args) => css`
      @media (max-width: 1023px) and (min-width: 768px) {
        ${css(...args)}
      }
    `,
    mobile: (...args) => css`
      @media (max-width: 767px) {
        ${css(...args)}
      }
    `,
  },
};
```

上述主题对象中定义了三个媒体查询断点：`desktop`、`tablet` 和 `mobile`。每个断点都接受一个参数，并在对应的媒体查询条件下将样式应用到组件。

通过使用 `${props => props.theme.mediaQueries...}` 的语法，你可以根据不同的断点应用特定的样式。例如，在上面的示例中，当屏幕宽度大于等于 `1024px` 时，`Container` 组件的宽度将变为 `960px`，在宽度介于 `768px` 和 `1023px` 之间时，宽度将变为 `768px`，在宽度小于等于 `767px` 时，宽度将变为 `100%`。

这样，你就可以根据不同的媒体查询断点来实现响应式布局。记得在使用之前定义对应的媒体查询断点，并将其添加到你的主题对象中。
