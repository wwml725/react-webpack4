## 安装相关依赖
- 这个包，我们只是一个react的开发环境，如果项目用到react的相关插件、或者redux等等。需要自行添加。

#### 1、webpack
```
npm install webpack webpack-cli webpack-dev-server html-webpack-plugin clean-webpack-plugin --save-dev
```
#### 2、Babel转义器
````
npm install babel-loader @babel/core @babel/preset-env @babel/preset-react --save-dev
````
babel转义所必须插件
```
npm install @babel/plugin-proposal-class-properties --save-dev
```

#### 3、react
```
npm install react react-dom --save
```
## 配置webpack.config.js
```
const path = require('path');
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CleanWebpackPlugin = require("clean-webpack-plugin");
module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'

    },
    mode: 'development',
    module: {
        rules: [

            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        //在这里配置，或者在根目录使用.babelrc文件
                        //在这个项目中为了学习，就都用了
                        presets: ["@babel/preset-env","@babel/preset-react"],
                        plugins: ["@babel/plugin-proposal-class-properties"]
                    }
                }
            },
            {test: /\.css$/, use: ['style-loader', 'css-loader']},
            {test: /\.(png|svg|jpg|gif)$/, use: ['file-loader']},
            {test: /\.(woff|woff2|eot|ttf|otf)$/, use: ['file-loader']},
            {test: /\.(csv|tsv)$/, use: ['csv-loader']},
            {test: /\.xml$/, use: ['xml-loader']},
            {
                test: /\.less$/,
                use: [
                    {loader: "style-loader"},
                    {loader: "css-loader"},
                    {loader: "less-loader"}
                ]
            }
        ],
    },
    devtool: 'inline-source-map',
    devServer: {
        contentBase: './dist'
    },

    plugins: [
     /*   new HtmlWebpackPlugin({
            title: "王伟-webpack"
        }),*/

     //在react中上面的方式，不能挂在元素。报错
        new HtmlWebpackPlugin({
            template: 'index.html'
        }),
        new CleanWebpackPlugin(["dist"])
    ]
}
```
## package.json
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack-dev-server  --open",
    "watch": "webpack --watch",
    "server":"node server/server.js"
  },

```