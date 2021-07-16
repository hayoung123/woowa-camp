

# Webpackdd으로 vanilla JS + Sass + img + svg 번들링하기



## webpack.config.js 전체 코드

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve('./dist'),
  },
  devServer: {
    port: 9000,
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.(png|jp(e*)g|svg)$/,
        loader: 'file-loader',
        options: {
          publicPath: './dist/',
          name: '[name].[ext]?[hash]',
        },
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Project Demo',
      minify: {
        collapseWhitespace: true,
      },
      hash: true,
      template: './public/index.html',
    }),
  ],
};

```



## babel.config.js 전체코드

> npm i -D @babel/preset-env core-js

- [preset-env](https://babeljs.io/docs/en/babel-preset-env)

필수적인 플러그인들의 모음

- [core-js](https://babeljs.io/docs/en/babel-core)

폴리필 사용 

```javascript
{
  presets: [
    [
      '@babel/preset-env',
      {
        targets: '>1%, not dead',
        corejs: { version: '3.8', proposals: true },
      },
    ],
  ];
}

```



* 웹팩을 활용해서 바벨을 실행하고 바벨을 직접 cli로 실행하지 않으려면 `babel-cli`는 설치할 필요가 없다.

## webpack.config.js 시작하기 전에


> npm i -D webpack webpack-cli



## webpack.config.js 살펴보기

### mode & entry & output

- mode

  webpack에 내장된 최적 기능을 사용할 수 있다. 

  mode를 config에서 설정 or  `webpack --mode=development`로 cli에서 실행할 수 있다. 

  - production (default)

  `DefinePlugin`의 `process.env.NODE_ENV`를 `production`으로 설정

  - development

  `DefinePlugin`의 `process.env.NODE_ENV`를 `development`으로 설정

  - none

  기본 최적화 옵션에서 제외

- entry

번들처리를 시작할 지점

- output

웹팩을 돌리고 난 결과물의 파일 경로

```javascript
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve('./dist'),
  },
  ...
}
```



### devServer

개발 단계에서 변경이 일어날 때 마다 번들링 돼 변경사항을 바로 반영해주는 것 `webpack serve` 하면 개발 서버가 실행된다. 


> npm i -D webpack-dev-server


- package.json의 script

```json
 "scripts": {
    "dev": "webpack serve"
  },
```

### devtool

개발자 도구의 소스탭의 보여지는 것을 설정할 수 있다. 

[소스맵 옵션 비교](https://webpack.js.org/configuration/devtool/#devtool)

### loader

웹팩은 이미지, 폰트 등 모든 것을 모듈로 관리하지만 JS밖에 알지 못한다. loader를 이용해 JS외의 파일들을 webpack이 이해하도록 만든다.

- test : 로딩할 파일 (css,sass,ts...)
- use: 적용한 loader

```javascript
module.exports = {
  ...
  module:{
    rules:[
      {
        test:~~~
        use: ['XXX-loader','XXX-loader,...']
      }
    ]
  }
}
```

### css loader

>  npm i -D style-loader css-loader

- style-loader: css를 `<style>` 태그로 출력
- css-loader

Module.rules에 추가

```javascript
 {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
 },
```

### sass/scss loader

> npm i -D node-sass sass-loader

Module.rules에 추가

```js
{
    test: /\.scss$/,
    loader:  ["style-loader", "css-loader","sass-loader'"],
}
```

### js - babel loader

> npm i -D babel-loader

Module.rules에 추가

```js
{
    test: /\.js$/,
    exclude: /node_modules/,
    loader: 'babel-loader',
},
```

### img & svg loader

> npm i -D url-loader file-loader

- url-loader : 이미지를 base64 문자열로 변환 뒤 코드에 인라인으로 추가
- file-loader: 사이지가 큰 이미지일 때 사용

Module.rules에 추가

- img

```javascript
 {
     test: /\.(png|jp(e*)g)$/,
     loader: 'url-loader',
     options: {
    	 limit: 8000, //8kb보다 작으면 url-loader가 담당
     	  name: 'images/[hash]-[name].[ext]'
    	}
 }
```

- svg

```javascript
{
    test: /\.svg$/,
    loader: 'file-loader'
},
```

## webpack plugins

로더는 파일 단위로 해석하고 변환하는 과정에서의 처리라면 **plugins**는 완료된 결과물을 처리하는 것이다.

### html-webpack-plugin


bundle한 css,js를 html에 직접 추가해줘야 되지만 html-webpack-plugin를 사용하면 이 과정을 자동화 할 수 있다.

> npm i -D html-webpack-plugin


```javascript
module.exports = {
  ...,
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Project Demo', 
      minify: {
        collapseWhitespace: true, //엔터 같은 것들 삭제
      },
      hash: true, //js파일 등을 해쉬 설정
      template: './public/index.html', //저장될 위치 및 파일
    }),
  ],
}  

```



### extract-text-plugin

파일을 분리하는 플러그인

> npm i -D extract-text-plugin


Ex) sass파일이 너무 커질 경우 따로 분리해서 번들링하는 것도 방법이다.

- 기존 extract-text-plugin 추가 전

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
    ],
  },
};
```

=>

- extract-text-plugin 적용

```javaScript
module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: ["css-loader", "sass-loader"],
        }),
      },
    ],
  },
  plugins: [new ExtractTextPlugin("style.css")],
}
```



## 참고 자료

- [webpack-sass](https://webpack.js.org/loaders/sass-loader/)

- [김정환 블로그 - 웹팩의 기본개념](https://jeonghwan-kim.github.io/js/2017/05/15/webpack.html)

- [html + css](https://medium.com/@shlee1353/%EC%9B%B9%ED%8C%A9-%EC%9E%85%EB%AC%B8-%EA%B0%80%EC%9D%B4%EB%93%9C%ED%8E%B8-html-css-%EC%82%AC%EC%9A%A9%EA%B8%B0-75d9fb6062e6)
