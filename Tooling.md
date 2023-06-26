# 工具
## **Babel Plugin**

这个插件增加了对服务器端渲染的支持，对样式的最小化，以及一个更好的调试体验。

### **使用**

先安装babel-plugin:

`npm install --save-dev babel-plugin-styled-components`

然后把它添加到你的babel配置中，像这样:

>NOTE
>你的`.babelrc`文件中的插件调用顺序很重要。如果您在您的babel配置中使用env属性，那么把这个插件放到plugins数组中就不够了。相反，它需要被放入每个env的plugins数组中，以保持它被首先执行。更多信息请看[<u>这里<u/>](https://github.com/styled-components/babel-plugin-styled-components/issues/78)

```jsx
{
  "plugins": ["babel-plugin-styled-components"]
}
```
## **服务端渲染**

通过为每个样式化组件添加一个唯一的标识符，这个插件可以避免因客户端和服务器端生成不同的类而导致的校验和不匹配。如果你不使用这个插件并试图在服务器端渲染样式化组件，React会在重新渲染过程中发出HTML属性不匹配的警告。




