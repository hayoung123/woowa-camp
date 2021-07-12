## webpack, bable - vanilla JS + Sass + img + svg

## loader

웹팩은 이미지, 폰트 등 모든 것을 모듈로 관리하지만 JS밖에 알지 못한다. loader를 이용해 JS외의 파일들을 webpack이 이해하도록 만든다.

- test : 로딩할 파일 (css,sass,ts...)
- use: 적용한 loader

## css

- style-loader: css를 `<style>` 태그로 출력
- css-loader

Module.rules에 추가

```javascript
 {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
 },
```

## sass

- node-sass
- sass-loader

Module.rules에 추가

```js
{
  test: /\.scss$/,
    loader:  ["style-loader", "css-loader","sass-loader'"],
}
```

## img & svg

- url-loader : 이미지를 base64 문자열로 변환 뒤 코드에 인라인으로 추가
- file-loader: 사이지가 큰 이미지일 때 사용

Module.rules에 추가

### img

```javascript
 {
   test: /\.(png|jp(e*)g)$/,
     loader: 'url-loader',
       options: {
         limit: 8000, //8000kb보다 작으면 url-loader가 담당
           name: 'images/[hash]-[name].[ext]'
       }
 }
```

### svg

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

### extract-text-plugin

파일을 분리하는 플러그인

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
