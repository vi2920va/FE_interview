# FrontEnd - Webpack

## 설명

Webpack이란 자바스크립트를 구성하는 HTML,CSS,JavaScript,Image 등을 모두 모듈로 보고 이를 조합하여 하나의 결과물로 만드는 모듈 번들러입니다.

- 여러 개의 js파일을 로딩하더라도 자바스크립트 변수 유효범위는 기본적으로 전역 스코프를 갖고 있기 때문에 전역변수 충돌이 발생할 수 있습니다. 이러한 문제를 웹팩의 모듈화를 통해 해결할 수 있습니다.
- 웹팩은 여러 개의 파일을 하나로 묶어주기 때문에 HTTP요청 횟수를 줄여 웹 애플리케이션의 성능을 높여줍니다.
- 웹팩의 Code Splitting의 기능을 사용하면 원하는 시점에 해당 모듈을 로딩하여 사용이 가능합니다.

### 1. JavaScript Modules  

자바스크립트에서는 **프로그램을 기능별로 쪼개놓은 것을 모듈**이라고 한다. ES6부터는 script의 `module` 타입을 공식적으로 인정해주기 시작해서 자바스크립트에서 모듈을 사용하는 것이 가능해졌다. module로 지정된 파일은 import와 export를 통해 다른 모듈을 불러오거나 현재 모듈을 내보내는 것이 가능합니다.

module 타입으로 지정된 스크립트는 모두 **defer타입으로 동작**합니다. 그러므로 자연스럽게 의존성 문제도 해결할 있습니다. ([defer에 대한 설명](https://developer.mozilla.org/ko/docs/Web/HTML/Element/script#%ED%8A%B9%EC%84%B1))

`index.js`에서 `sum.js`와 `matrix.js` 모듈 두 개를 import했다면, index.js를 실행할 때 서버에서 `index.js`, `sum.js`, `matrix.js` 이렇게 세 개의 모듈을 각각 로드해옵니다.

😀 장점

- 각 모듈끼리 서로 분리된 채로 코드를 작성할 수 있다. 변수 중복에 대한 걱정을 할 필요가 없습니다.
- 의존성 문제를 해결할 수 있습니다.

😕 단점

- 하나의 파일에서 너무 많은 모듈을 불러와서 사용할 경우 요청에 대한 리소스가 많이 발생하게 됩니다.

### 2. Webpack 

웹팩은 **모듈 번들러**이다. 위에서 말한 index.js와 sum.js 그리고 matrix.js 모듈을 한 데 모아 하나의 모듈로 번들링 작업을 하게 됩니다. 정확히는 **dependency graph라고하는 의존성 그래프를 그려서 하나의 bundle을 생성**해줍니다.

**하나의 파일 덩어리만 불러오면 되기 때문에 요청에 대한 리소스가 줄어든다**. 그 외에도 alias를 사용한 import 경로 단순하게 표현하기나 Lazy Loading 등 여러 가지 기능을 지원합니다.

### 3. Webpack Configuration (설정 파일)

(참고 - Webpack4.0.0부터는 별도의 설정 파일이 없어도 프로젝트를 번들링할 수는 있다. 그러나 사소한 부분까지 직접 설정하기 위해서는 설정 파일을 추가하는 것이 좋습니다.)

webpack 설정 파일의 구성요소는 아래와 같습니다.

#### 3-1. Entry

dependency graph가 시작되는 모듈을 의미한다. 위의 예시에서는 `index.js` 모듈부터 시작해서 나머지 모듈들을 불러왔으므로 `index.js`가 entry가 됩니다.

```js
module.exports = {
  entry: './path/to/my/entry/index.js',
};
```

#### 2. Output

번들링되어 나오는 결과물 어느 파일에 저장할지, 이름은 뭐라고 지을지를 결정한다. 저장 경로와 이름은 각각 `path`와 `filename` 속성에 지정해주면 됩니다.

```js
const path = require('path');

module.exports = {
  ...,
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.bundle.js',
  },
};
```

filename은 직접 지정해주는 대신 청크의 이름이나 id 또는 hash값으로 자동 생성되도록 설정할 수도 있습니다. ([output.filename에 관한 내용](https://webpack.js.org/configuration/output/#outputfilename)) Hash값으로 번들 이름을 설정해주는 이유는 스크립트 파일이 캐싱되어 변경 이전의 파일에서 업데이트되지 않는 문제를 해결하기 위함이다. 매번 새로운 해시값으로 파일명을 만들어 캐싱되는 문제를 해결할 수 있습니다.

#### 3. Loaders

웹팩은 기본적으로 자바스크립트와 JSON파일만 이해할 수 있다. Loader는 다른 종류의 파일(html, css 등)도 모듈로 변환해준다. 우리가 React에서 이미지나 css파일을 단순히 `import '{path}.css'`와 같이 로딩하는 것처럼 Loader를 사용해 파일을 모듈화할 수 있다. [여기](https://webpack.js.org/loaders/)에 다양한 Loader의 종류가 있다.

Loader의 속성은 두 가지가 있습니다.

- `test` 속성 - 어떤 파일이 변환되어야 하는가 입니다.
- `use` 속성 - 파일을 변환하기 위해 어떤 Loader가 사용되어야 하는지 입니다.

```js
const path = require('path');

module.exports = {
  ...,
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }], // txt파일을 raw-loader를 이용해 변환하겠습니다.
  },
};
```

#### 4. Plugins

웹팩의 기능을 더욱 극대화시켜주는 역할을 한다. 가령 번들링 최적화, asset 관리나 환경변수 주입 등에 사용될 수 있습니다.

아래 예시는 웹팩 플러그인 중 하나인 `HtmlWebpackPlugin`에 관한 예시다. 이 플러그인은 번들을 생성해서 자동으로 HTML파일을 만들어준다. 플러그인을 불러올 때는 `require`를 이용해 가져옵니다.

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

📌 그 외 `SourceMapDevToolPlugin`도 유용하게 사용할 수 있다. 이 플러그인은 개발 환경에서 사용하는 것인데, 웹팩의 단점을 굳이 꼽자면 하나의 파일로 번들링이 되어 디버깅이 어렵다는 것이다. Source map은 번들링된 파일이 실제 어떤 모듈의 어느 코드에 맵핑되는지 알려주는 디버깅 도구입니다.

#### 5. Mode

개발 환경인지 프로덕션 환경인지 설정해줄 수 있다. 어떤 환경인지에 따라 다른 웹팩 설정파일을 생성해서 사용할 수 있습니다.

## 요약

- 웹팩이란 자바스크립트 모듈 번들러입니다.
- 파일단위의 모듈관리가 가능합니다.
- 서버와의 통신을 줄일 수 있습니다.
- 모듈이 필요한 시점에 로딩이 가능합니다.
- 자바스크립트 모듈은 각 모듈을 개별적으로 로딩하기 때문에 파일을 서버에 요청해 불러오는 리소스가 크다.
