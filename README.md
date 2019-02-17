# spa-by-vuejs-and-firebase

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

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
