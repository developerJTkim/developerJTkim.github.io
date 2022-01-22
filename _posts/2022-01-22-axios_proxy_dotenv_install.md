---
title: "[Nuxt.js] axios,proxy,dotenv 설치"
---

Axios는 http통신을 위한 오픈소스 라이브러리이다. 기본 Axios를 사용해도 무방 하지만,  
[Nuxt.js axios](http://https://axios.nuxtjs.org/setup/)용 모듈화로 만들어진것이 있기 때문에 그걸로 사용하며, 
[Proxy모듈](https://github.com/nuxt-community/proxy-module)과 통합이 가능하기 때문에 같이 설치한다.

> 자바스크립트 보안 정책 중 Same-Origin Policy로, 스크립트가 실행되는 페이지와 비동기 호출 시 주소의 프로토콜, 호스트, 포트가 같아야 함.
> 
> 크로스 도메인 이슈해결을 위해 Nuxt.js의 @nuxtjs/proxy모듈을 설치했다.
> 
> 기타 프록시 설정 필요시 다음과 같이 사용 가능하다.

.env파일이나 dotenv파일은 응용 프로그램 환경 변수를 제어하는 간단한 텍스트 구성 파일
애플리케이션에는 각 환경 사이에 일부 변수를 변경해야 하는 경우가 있기때문에
.env파일은 변수명=값의 형태로 되어 있습니다.
변수명은 대문자로 사용하며 각 문자 사이에는 underscore( _ )를 통해 구분하게 됩니다


> .env의 경우 git 노출시 보안상 문제가 발생할수 있기때문에, git ignore등록되어 push가 안되게끔 설정이 필요함.
> 
> 

.env
```json
NUXT_APP_TYPE="local" # local : 로컬환경 ,  product : 실 서버 환경
NUXT_LOCAL_APP_BASE_URL="http://192.168.0.185:3000"
NUXT_LOCAL_APP_API_URL="http://192.168.0.185:9000"

NUXT_PRODUCT_APP_BASE_URL="http://nuxt.com:3000"
NUXT_PRODUCT_APP_API_URL="http://api.nuxt.com:3000"
```

nuxt.config.js
```javascript
// Axios module configuration: https://go.nuxtjs.dev/config-axios
  axios: {
    // Workaround to avoid enforcing hard-coded localhost:3000: https://github.com/nuxt-community/axios-module/issues/308
    proxy: true,   // default: false, boolean or object 설정 가능
    // baseUrl: 'http://localhost', // proxy 설정시 baseUrl 사용 안함
    credential: false,
    debug: true,
  },
  proxy: {
    '/api/': {
      target: process.env.NUXT_APP_TYPE == "local" ?
        process.env.NUXT_LOCAL_APP_API_URL :
        process.env.NUXT_PRODUCT_APP_API_URL
      ,
      pathRewrite: { '^/api:': ''},
      changeOrigin: false,
      prependPath: false
    },
  },
```
