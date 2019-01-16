<190116>



## 1. Hello Node.js

#### HTTP Methods 개념 확인하기(GET, POST, PUT, DELETE)

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/HTTP-Methods-HTTP-Verbs-GET-POST-PUT-PATCH-DELETE](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/HTTP-Methods-HTTP-Verbs-GET-POST-PUT-PATCH-DELETE)



#### Express 로 서버 실행하기

1.<br>작업 디렉토리(workspace) 생성하기<br>웹스톰에서 해당 디렉토리 오픈 후 View - Tool Windows - Terminal 실행 // 하단에 터미널 창이 생성된다. 



2.<br>터미널 창에서 아래의 명령어를 입력하여 helloworld 디렉토리 생성 후 해당폴더로 이동.

```javascript
> mkdir helloworld
> cd helloworld
```



3.<br>`npm init` 을 통행 `package.json` 생성.

```
> npm  init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (npmtest) npmTest-888
version: (1.0.0) 
description: npm test code
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: YongHoonJJo
license: (ISC) MIT
About to write to /Users/ychooni/dev/zerocho/lecture/ch05/npmTest/package.json:

{
  "name": "npmtest",
  "version": "1.0.0",
  "description": "npm test code",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "YongHoonJJo",
  "license": "MIT"
}

Is this OK? (yes) 
```

> `npm init` 을 커맨드 라인에서 입력을 하면, 내가 만들 패키지에 대한 이름, 설명, 버전 등을 지정할 수 있게 된다.<br>그리고 결과적으로 `package.json` 파일이 생성된다. 새로운 프로젝트나 패키지를 만들 때 사용합니다.



```json
{
  "name": "helloworld",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "YongHoonJJo",
  "license": "MIT"
}
```

> `npm init` 후 생성된 `package.json` 파일의 내용이며, 이 패키지의 설명서 역할을 한다.<br>그리고 entry point 가 main 이 되었다. `package.json` 에서 "main" 에 해당하는 파일이름이 이 패키지에서 처음 실행되는 파일이 된다. 또한, 이곳을 통해 이 패키지에서 가장 중요한 파일을 확인할 수 있다
>
> 다른 사람의 패키지를 사용할 때는 먼저 그 패키지의 `package.json` 파일을 열어보자. 그렇게 하면, 거기서 얻을 수 있는 정보가 많다. 소스코드 분석 보다는 `package.json` 파일을 먼저 열어보고, 이런 것을 사용하고 있구나 라는 것을 파악하는 것이 좋다. (by zerocho)
>
> [MIT 라이센스](https://ko.wikipedia.org/wiki/MIT_허가서) 관련 위키 개요 읽어보기.



4.<br>entry point 로 지정된 파일 생성하기. <br>여기서는 `/helloworld` 디렉토리에 `index.js` 파일을 생성하면 된다. 



5.<br>Express 설치하기

```
> npm install --save express
```

> express 패키지를 설치하는 명령어로, 패키지를 설치할 때는 `package.json` 파일이 있는 곳에서 진행을 해야한다.
>
>  `package.json` 에 변화가 생기며, `package-lock.json` 파일이 생성된다. 또한 `node_modules` 라는 폴더가 생성된다. 그리고 이 폴더 안에는 `express` 뿐만 아리다 다른 여러 패키지들까지 포함되어 있다. 이는 `express` 가 사용하고 있는 package 이기도 하며, `express` 가 사용하고 있는 package 가 사용하고 있는 package 이기도 하다.  



```JSON
{
  "name": "helloworld",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "YongHoonJJo",
  "license": "MIT",
  "dependencies": {
    "express": "^4.16.4"
  }
}
```

> dependencies 에 express 가 추가되어 있다. 뒤의 숫자는 현재 해당 패키지의 설치된 버전.
>
> 현 시점에서 `package-lock.json` 에는  `express` 가 사용하고 있는 패키지들에 대한 정보가 담긴다. 



6.<br>index.js 소스코드 작성 <br>(소스코드 출처: [https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Express로-서버-실행하기](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Express로-서버-실행하기))

```javascript
//index.js

const express = require('express'); // 설치한 express module을 불러와서 변수(express)에 담습니다.
const app = express(); //express를 실행하여 app object를 초기화 합니다.

app.get('/',function (req,res) { // '/' 위치에 'get'요청을 받는 경우,
 res.send('Hello World!'); // "Hello World!"를 보냅니다.
});

app.listen(3000, function(){ //3000번 포트를 사용합니다.
 console.log('Server On!'); //서버가 실행되면 콘솔창에 표시될 메세지입니다.
});
```

> `require('model_name')` 의 결과는 함수이고, 그 함수를 실행시킨 결과를 `app` 변수에 담는다 . 그리고 `app.get()` 등의 사용방식을 보아 이 `app` 은 객체라고 생각할 수 있다. 
>
> `app.get()` 에서 get 은 HTTP Method 이며, 첫번째 인자로 URL 을 문자열 형태로 저장한다. 두 번째 인자로는 함수를 넣고, GET 방식으로 첫번째 인자로 등록된 URL 로 어떤 요청을 받았을 때, 두번째 인자로 등록된 함수가 실행이 된다. 
>
> `req` 는 리퀘스트 관련 내용이 담긴다. <br>`res` 에는 응답으로 전달될 내용을 담을 수 있다.
>
> 자세한 내용은 주석 및  [소스코드 출처의 블로그](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Express로-서버-실행하기) 내용을 읽어보자. 



7.<br>서버 실행하기

```
> node index.js
```

>  정상적으로 실행이 되면, 터미널에서 Server On! 이란 메세지를 확인할 수 있다.
>
> 그리고 브라우저에서 `localhost:3000/` 을 입력하면 Hello World! 라는 내용을 확인할 수 있다.

<img alt="2019-01-16 10 00 13" src="https://user-images.githubusercontent.com/13485924/51250599-35577600-19da-11e9-9ebd-84e5ee2bea5d.png" width="70%" height="70%">

8.<br>응용하기

```javascript
const express = require('express'); // 설치한 express module을 불러와서 변수(express)에 담습니다.
const app = express(); //express를 실행하여 app object를 초기화 합니다.

app.get('/a',function (req,res) { // '/' 위치에 'get'요청을 받는 경우,
    res.send('Hello World!'); // "Hello World!"를 보냅니다.
});

app.listen(3000, function(){ //3000번 포트를 사용합니다.
    console.log('Server On!'); //서버가 실행되면 콘솔창에 표시될 메세지입니다.
});
```

> `index.js` 파일에서 `app.get()` 에서 URL 의 내용을 위와 같이 변경해보자.
>
> 그리고 서버 재시작(종료 후 다시 시작) 후 브라우저에서 `localhost:3000/` 와 `localhost:/3000/a` 를 각각 입력해보자. 전자의 경우 `Cannot GET /`  이란 내용의 화면이 보이게 되며, 후자의 경우 이전에 확인했던 결과를 확인할 수 있게 된다. 등록되지 않은 곳으로 접근을 시도했기 때문에 발생한 문제라고 생각하면 된다.



9.<br>Github 에 소스코드 업로드 관련.

공개되어서는 안되는 정보를 담고있는 파일들과 굳이 올릴 필요가 없는 파일들이 있을 것이다. `.gitignore` 파일을 통해 설정가능하며,  `node_modules` 라는 폴더가 굳이 올릴필요가 없는 것중에 하나이다. 이는 여기에 들어있는 모듈(패키지)들은 `pacakage.json` 에 모두 기록이 되어 있기 때문에, 추후 `npm install` 명령어만을 통해서 이 패키지가 의존하고 있는 모듈들을 손쉽게 설치할 수 있다.



#### Static(정적) Site, Dynamic(동적) Site 개념 확인하기

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Static-정적-Site-Dynamic-동적-Site](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Static-정적-Site-Dynamic-동적-Site)



#### Static 폴더 추가하기

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Static-폴더-추가하기](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/Static-폴더-추가하기)

위 URL 에서 폴더구조 확인 후 실습 진행. (가볍게 확인해보고 넘어가도 된다.)



#### ejs로 Dynamic Website 만들기

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/EJS로-Dynamic-Website-만들기](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World/EJS로-Dynamic-Website-만들기)

마찬가지로 가볍게 실습 진행 후 개념정도만 간단하게 확인 후 넘어가자.

ejs 는  Express 의 View 엔진중 하나이며, 다른 View 엔진들로는 pug, nunjucks 가 있다.





참고 : [https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/Hello-World)

참고 : [https://www.zerocho.com/category/NodeJS/post/58285e4840a6d700184ebd87](https://www.zerocho.com/category/NodeJS/post/58285e4840a6d700184ebd87)

