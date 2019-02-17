# Deploy

```bash
$ yarn build
$ firebase deploy
```

# vue-loader

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 用語

- SFC
  - Single File Component

## 注意点（自力で解決した点）

- To implement CSS(SASS) file

```bash
$ yarn add node-sass sass-loader
```

- `webpack.config.js`

```javascript
-        test: /\.css$/,
+        test: /\.(sa|sc|c)ss$/,
```

- About packages version
  - "webpack-dev-server" 3 causes an error
  - "css-loader" 2 causes an error