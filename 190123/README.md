<190123>

## 2. 주소록 만들기 (1)

#### (1) 환경변수 개념 확인

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/Environment-Variable-환경변수](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/Environment-Variable-환경변수)



#### (2) 프로젝트 생성 및 mongoose 연결하기

```
> npm init
> npm install --save ejs express mongoose
```

> `package.json` 파일을 생성<br>ejs, express, mongoose 패키지 설치<br>`index.js` 파일 생성하기.



아래의 소스코드를 입력해보자. (출처: [https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/주소록-프로젝트-생성-및-mongoose로-DB-연결](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/주소록-프로젝트-생성-및-mongoose로-DB-연결))

```Javascript
/*** index.js ***/
const express = require("express");
const mongoose = require("mongoose");
const app = express();

// DB setting
mongoose.connect(process.env.MONGO_DB, { useNewUrlParser: true }); // 1
var db = mongoose.connection; // 2

// 3
db.once("open", function(){
 console.log("DB connected");
});

// 4
db.on("error", function(err){
 console.log("DB ERROR : ", err);
});

// Other settings
app.set("view engine", "ejs");
app.use(express.static(__dirname+"/public"));

// Port setting
var port = 3000;
app.listen(3000, function(){
 console.log("server on! http://localhost:"+port);
});
```

> 각 코드에 대한 설명은 코드의 출처인 곳에 가서 확인해보기.<br>또한, 정상적으로 서버가 실행이되고, mongoDB 에 접속이 되면 `DB connected` 메세지를 확인할 수 있다.



#### (3) process.env.MONGO_DB 가 undefined 인 경우.

`dotenv` 라는 모듈의 도움을 받아보자.

`process.env` 를 통해 환경변수로 등록해둔 변수를 가져올 수 있다. 

```
> npm i dotenv
```

>  `dotenv` 모듈을 설치 후 `.env` 파일을 만들면, `.env` 파일의 내용이 환경변수에 들어가게 된다. <br>
>
> `npm install` 은 `npm i` 로 줄여쓸 수 있으며, `--save` 는 생략가능하다. 



폴더구조는 아래와 같다.

<img width="282" alt="nodejs_01" src="https://user-images.githubusercontent.com/13485924/51615285-c6929380-1f6a-11e9-8e44-12c7f3b730e5.png">

\- `.env` 파일을 생성한 상태이다.



`.env` 파일의 내용

<img width="607" alt="nodejs_dotenv" src="https://user-images.githubusercontent.com/13485924/51615450-3c96fa80-1f6b-11e9-90f5-81537b06f9d2.png">

> 위와 같이 입력하면 된다. 



위의 `.env` 에서 `username` 과 `password` 는 아래의 화면에서 입력하는 내용을 입력하면 된다. 

<img width="1221" alt="nodejs_mongodb" src="https://user-images.githubusercontent.com/13485924/51624426-26466a00-1f7e-11e9-9611-3b86a72141a2.png">

> Database username / Database password



@ 뒷부분은 아래 그림의 MongoDB URI 에서 @ 뒷부분을 가져다 쓰면 된다. 

<img width="640" alt="nodejs03" src="https://user-images.githubusercontent.com/13485924/51615571-8b449480-1f6b-11e9-9492-de98e096fb03.png">

> 위 내용은 [mlab](https://mlab.com/home) 에 접속후 `MongoDB Deployments` 에서 `Create New` 를 통해 생성할 수 있다.



그리고 index.js 파일에서 코드를 추가해야 한다. 

```Javascript
const express = require("express");
const mongoose = require("mongoose");
require('dotenv').config();

const app = express();

mongoose.connect(process.env.MONGO_DB, { useNewUrlParser: true });
const db = mongoose.connection;

db.once("open", function() {
    console.log("DB connected");
});

db.on("error", function(err) {
    console.log("DB ERROR : ", err);
});

app.set("view engine", "ejs");
app.use(express.static(__dirname+"/public"));

const port = 3000;
app.listen(port, function() {
    console.log("server on! http://localhost:"+port);
});
```

> `require('dotenv').config();` 한 줄을 추가시키면 된다. 



당연한 이야기지만, 나중에 이 프로젝트의 코드를 github 등 remote repository 에 올릴 경우 `.env` 파일만 올리지 않으면 된다. 



#### (4) Module 을 이용한 처리.

Module 에 대한 내용은 아래 링크에서 확인하기.

[https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/Node-js-Module](https://www.a-mean-blog.com/ko/blog/Node-JS-첫걸음/주소록-만들기/Node-js-Module)



`utils` 폴더를 생성 후 그 안에 `mongodb.util.js` 파일을 만들자.

```Javascript
/*** utils/mongodb.util.js ***/
const dbData = {
    dbName: "username",
    dbPassword: "password",
    mlabURI: "ds211275.mlab.com:11275/contact-book"
};

module.exports = dbData;
```

> 이 파일을 remote repository 에 올리지 않으면 된다. 



```Javascript
/*** index.js ***/
const express = require("express");
const mongoose = require("mongoose");
const dbData = require('./utils/mongodb.utils');

const app = express();

const dbName = dbData.dbName;
const dbPasswd = dbData.dbPassword;
const mlabURI = dbData.mlabURI;
const dbURI = `mongodb://${dbName}:${dbPasswd}@${mlabURI}`;
mongoose.connect(dbURI, { useNewUrlParser: true });
//mongoose.connect(process.env.MONGO_DB, { useNewUrlParser: true });
const db = mongoose.connection;
```

> 위와 같이 사용자 Module 을 만들어서 불러올 수 있다.



#### (5) 템플릿 문자열 (ES6+ 문법)

```javascript
const a = 'hello';
const b = true;
const c = 3;
const d = `${a} ${b} ${c}`;
console.log(d);				// hello true 3
```

> d 에는 'hello true 3' 이 저장된다.



변수를 쓰는 문자열은 템플릿 문자열을 선호한다.<br>또한, 백틱을 사용하면 작은 따옴표와 큰 따옴표의 구분을 할 필요가 없다.



#### (6) mongoDB 와 mongoose 와의 관게

`Mongoose`는 MongoDB 기반 ODM(Object Data Mapping) Node.JS 전용 라이브러리입니다. ODM은 데이터베이스와 객체지향 프로그래밍 언어 사이 호환되지 않는 데이터를 변환하는 프로그래밍 기법입니다. 즉 MongoDB 에 있는 데이터를 여러분의 Application에서 JavaScript 객체로 사용 할 수 있도록 해줍니다.<br>(출처: [https://velopert.com/594](https://velopert.com/594))