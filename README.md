# webpack-basic-env
웹팩 기본 설정 실습
  
https://www.youtube.com/watch?v=rbmUFHZt3sg&t=364s  
김정환님의 '웹팩(Webpack) 최소설정으로 프론트엔드 개발 환경 만들기' 영상을 참고하여 만든 실습 프로젝트입니다.
  
   
# 실습 내용 정리  
https://www.notion.so/Webpack-a91feb0dc5d4432f91dfe7bbb2672ee1  
  

# **webpack설치 및 기본 파일 생성**

```sql
npm init -y
npm i -D webpack webpack-cli
```

npm scripts에 빌드 추가 ( build : webpack) -> npm run build 시 웹팩 스크립트가 실행된다.

웹팩은 최소한의 설정이 필요하다. webpack.config.js 설정파일을 만들자.

```sql
const path = require('path')
module.exports = { 
 mode: 'development',
 entry: {
			main: './src/app.js' 
	},
	output: {
			path: path.resolve('./dist'),
			filename: '[name].js' 
	}
}
```

- mode - 개발용이냐 프로덕션 용이냐. 우리는 development로 하자.
- entry -여러개의 모듈로 연결되어있는 시작점. 웹팩에서 알려주면 연결되어있는걸 실행시킨다. 엔트리에 시작점을 만들어주면 된다.
- output은 여러개의 모듈을 하나로 만들어서 저장시킬 위치. 두개로 이루어져있는데 폴더 이름과 파일명을 입력하면 된다.
- filename : [name].js => 이 변수는 엔트리에 설정한 키값이 들어올 것이므로 결과적으로 main.js가 만들어질것

src폴더에 app.js 만들어보자. 돔이 만들어지고 실행되는 코드.

```jsx
import { sum } from './math.js';
import './app.css'

window.addEventListener('DOMContentLoaded', () => {
    sum(1, 2);
    const el = document.querySelector('#app')
    el.innerHTML = `<h1>1+2 = ${sum(1,2)}</h1>`
})
```

```jsx
export function sum(a, b) { 
	return a + b;
}
```

math.js에 sum이라는 함수를 만든 뒤, app.js에서 math.js 코드를 가져와보자.

이후 npm run build 명령어를 실행시키면

dist파일에 만들어 지는 것을 확인할 수 있다.

index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="app"></div>
    <script src="../dist/main.js"></script>
</body>

</html>
```

.open src/index.html을 하면 정상적으로 실행되는 것을 확인할 수 있다.

![https://blogfiles.pstatic.net/MjAyMDA5MjJfMjIg/MDAxNjAwNzQ3MzQ3NDUy.L7dyOUM4g6IeEMsl7sfq02ZDbtZDv6_cDD1L1rt-4j8g.g7kv_GIV09fMMboutApnB_SYZojpZtf4hlo9iLwr2B4g.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.02.17.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfMjIg/MDAxNjAwNzQ3MzQ3NDUy.L7dyOUM4g6IeEMsl7sfq02ZDbtZDv6_cDD1L1rt-4j8g.g7kv_GIV09fMMboutApnB_SYZojpZtf4hlo9iLwr2B4g.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.02.17.png?type=w1)

---

# **CSS 다루기 (css-loader, style-loader)**

![https://blogfiles.pstatic.net/MjAyMDA5MjJfMjY4/MDAxNjAwNzQ3MDU1NDE2.Kmcphorx_A7-GVHk5qAtpTQcu9p5B9kWADuPNlwnaY8g.DueCPeANPcNKkaQ5MDTRJKBZuvX448jCk1dU7NFcGoUg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_12.56.38.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfMjY4/MDAxNjAwNzQ3MDU1NDE2.Kmcphorx_A7-GVHk5qAtpTQcu9p5B9kWADuPNlwnaY8g.DueCPeANPcNKkaQ5MDTRJKBZuvX448jCk1dU7NFcGoUg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_12.56.38.png?type=w1)

웹팩은 CSS파일도 모듈로 인식한다. CSS파일도 하나의 파일로 머지가 될 것.

**그러려면 css관련된 웹팩 설정을 해야하는데 이때 사용하는 것이 css-loader.**

npm i -D css-loader 으로 설치해주자.

그다음에 웹팩 설정에 모듈에 설정을 해줘야한다.

```jsx
const path = require('path')

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: ['css-loader']
        }]
    }
}
```

app.css를 만들어 body color 적용 뒤,

app.js에서 가져와보자 import './app.css'

```jsx
body { 
	background-color: pink;
}
```

그뒤에 빌드를 해보자.

하지만 브라우저에서 확인하면 여전히 색상 적용이 안됐다. 브라우저가 아직 인식을 못했기 때문에!

**이를 위해 style-loader를 설치해줘야한다. (**npm i -D style-loader)

그 뒤에 웹팩 config module에 추가해주자.

```jsx
module: { rules: [{ test: /\.css$/, use: ['style-loader', 'css-loader'] }] }
```

다시 npm run build를 하면 색상이 적용된걸 확인할 수 있다.

![https://blogfiles.pstatic.net/MjAyMDA5MjJfMzcg/MDAxNjAwNzQ5MjgzMTMy.H7FgA_rFSLNs3p8MtnWikG0bEgfW6E4qIW-9Luu1aOMg.Kp9c-EKn5_pgegYwvzyucpxnK1BNF9KfW4e2wJpAo0gg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.34.32.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfMzcg/MDAxNjAwNzQ5MjgzMTMy.H7FgA_rFSLNs3p8MtnWikG0bEgfW6E4qIW-9Luu1aOMg.Kp9c-EKn5_pgegYwvzyucpxnK1BNF9KfW4e2wJpAo0gg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.34.32.png?type=w1)

---

# **이미지 파일 다루기 file-loader**

이미지를 src로 가져온 뒤, app.js에서 import해주자.

그 뒤에 해당 이미지를 노출시키기 위해 innerHTML에 다음과 같이 작성한다.

```jsx
import { sum } from './math.js';
import './app.css'
import webpackImage from './webpack.png'

window.addEventListener('DOMContentLoaded', () => {
    const el = document.querySelector('#app')
    el.innerHTML = `
        <h1>1+2 = ${sum(1,2)}</h1>
        <img src="${webpackImage}" alt="webpack"/>`
})
```

이미지 파일을 다룰수 있도록 웹팩 설정을 해주자 (file-loader)

```jsx
npm i -D file-loader
```

로 파일로더를 설치해주자.

그 뒤에 웹팩설정에서 png확장자에 대해 모듈 설정을 다음과 같이 추가해주자.

```jsx
const path = require('path')
module.exports = { mode: 'development', entry: { main: './src/app.js' }, output: { path: path.resolve('./dist'), filename: '[name].js' }, module: { rules: [{ test: /\.css$/, use: ['style-loader', 'css-loader'] }, { test: /\.png$/, use: [{ loader: 'file-loader', options: { publicPath: '../dist' } }] } ] }
}
```

options :

publicPath : '../dist’하면

이미지로 호출할떄 경로를 앞에 추가해준다.

npm run build를 하면 정상적으로 이미지가 로드된 것을 확인할 수 있다.

<img width="400" src="https://blogfiles.pstatic.net/MjAyMDA5MjJfMTEz/MDAxNjAwNzUwMzk3NDUx.RTFbLsv_dqjUyFf5SIPR-e8J3uerqF8PXfs-Wtw4nyQg.10J624m128DIF0lQTVCamV2LnV4T0B9wwGdVtsGBNkQg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.53.00.png?type=w1" />

---

# **HTML 다루기 (html-webpack-plugin)**

![https://blogfiles.pstatic.net/MjAyMDA5MjJfMjY1/MDAxNjAwNzUwNDM3NDg0.5aPFt3Z-Kqsi0b9CwwEYpta38811E9FFnJE4FYVYE20g.rFB2EbQBPZLD-bGef3vPf4-OBMYisCc8vNdvyN2ONcwg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.53.43.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfMjY1/MDAxNjAwNzUwNDM3NDg0.5aPFt3Z-Kqsi0b9CwwEYpta38811E9FFnJE4FYVYE20g.rFB2EbQBPZLD-bGef3vPf4-OBMYisCc8vNdvyN2ONcwg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_1.53.43.png?type=w1)

이 때는 로더가 아니라 플러그인을 사용한다.

```jsx
npm i html-webpack-plugin -D
```

플러그인 함수라서 생성자 함수가 필요.

webpack.config.js에 생성자 추가한 뒤에 그리고 모듈 아래에 plugins라는 배열을 추가.

```jsx
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
    module: {
        rules: [{
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.png$/,
                use: [{
                    loader: 'file-loader',
                    options: {
                        name: '[name].[ext]?[hash]'
                    }
                }]
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new CleanWebpackPlugin(),
    ]
}
```

인자로 템플릿파일의 위치를 지정한다.

index.html을 보면 로딩했던 아웃풋의 경로를 넣어줬었는데

이제는 플러그인이 자동으로 넣어줄 것이기 때문에 제거해도 괜찮음.

npm run build해주면 dist 폴더에 index.html이 만들어지는 것을 볼 수 있다.

그리고 이미지 호출할때 path로 dist를 별도로 지정했었는데

이제는 index.html도 웹팩이 만들기 때문에 주소를 따로 만들지 않아도 된다.

so options : publicPath를 제거 해주자. 그러면 이미지도 dist에 있는걸 자동으로 가져오니까 문제없이 작동한다.

+ options: 결과물의 이름을 해시값보다는 실제 파일값으로 하는게 좋을 것 같다.

```jsx
								test: /\.png$/,
                use: [{
                    loader: 'file-loader',
                    options: {
                        name: '[name].[ext]?[hash]'
                    }
                }]
```

이렇게 하면 원본파일의 이름과 확장자를 그대로 가져오고 파일을 호출할때 최신 파일을 호출.

그러면 dist 안에 있는 파일들의 이름이 원본 파일로 바뀌는 것을 확인가능.

## **+ clean-webpack-plugin**

![https://blogfiles.pstatic.net/MjAyMDA5MjJfMTkz/MDAxNjAwNzUyMjY2NTg5.Aoh5IcCpDuGTEChlWKe-gU76qeuMorzucVpQ7Wf1diwg.5n-kGw70DttL_c4rZEIlIP1jGtVRZx6jjlM_h7w6ZGAg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_2.04.03.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfMTkz/MDAxNjAwNzUyMjY2NTg5.Aoh5IcCpDuGTEChlWKe-gU76qeuMorzucVpQ7Wf1diwg.5n-kGw70DttL_c4rZEIlIP1jGtVRZx6jjlM_h7w6ZGAg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_2.04.03.png?type=w1)

웹팩으로 이미 만들어진 해시 파일명의 파일들을 삭제가능하다.

```jsx
npm i -D clean-webpack-plugin
```

생성후({}감싸야함), plugins인 아래에 new CleanWebpackPlugin()

하면 자동으로 아웃풋으로 설정된 dist폴더를 삭제하고 웹팩을 돌릴 것.

![https://blogfiles.pstatic.net/MjAyMDA5MjJfOTIg/MDAxNjAwNzUyMzcyNTMz.sX8blboXLX8-AamceDSM77KXFsY542tqRm3xqn5NAcAg.VmlzUFG1QzQw-48C7vspR3iQCkuJVRs2D_4JUxQUhQsg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_2.26.06.png?type=w1](https://blogfiles.pstatic.net/MjAyMDA5MjJfOTIg/MDAxNjAwNzUyMzcyNTMz.sX8blboXLX8-AamceDSM77KXFsY542tqRm3xqn5NAcAg.VmlzUFG1QzQw-48C7vspR3iQCkuJVRs2D_4JUxQUhQsg.PNG.sknglee22/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2020-09-22_%EC%98%A4%ED%9B%84_2.26.06.png?type=w1)

 해시가 파일명인 파일이 사라지고 깔끔하게 파일 생성됨!
