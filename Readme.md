0. Reference
    - https://poiemaweb.com/es6-babel-webpack-1

1. Babel이란
    - 최신 자바스크립트 코드를 IE나 구형브라우저에서도 동작하는 ES5이하의 코드로 트랜스파일링해줌.

2. Babel CLI 설치
    - npm i -D @babel/core @babel/cli

3. .babelrc 파일 생성
    - Babel을 사용하려면 @babel/preset-env를 설치해야 함
        - 함께 사용되어야하는 Babel 플러그인을 모아둔 것으로 Babel 프리셋이라고 부름
            - @babel/preset-env
            - @babel/preset-flow
            - @babel/preset-react
            - @babel/preset-typescript
    - @babel/preset-env도 공식 프리셋의 하나이며 필요한 플러그인들을 프로젝트 지원환경에 맞춰 동적으로 결정해줌. 
        - 프로젝트 지원환경은 .browserslistrc 파일에 상세히 설정할 수 있음. 생략하면 기본값으로 설정됨.
    - npm i -D @babel/preset-env
    - 설치 후 .babelrc 파일에 코드 작성
        - presets의 값으로 @babel/preset-env

3. 트랜스파일링
    - Babel CLI 명령어를 사용하는 방법과 npm script를 사용하는 방법이 있음
    - npm script 사용시
        - package.json 파일에 script를 추가
            - babel src/js -w -d dist/js
    - @babel/preset-env는 현재 제안 단계에 있는 사양에 대한 플러그인을 지원하지 않기 때문에 이를 지원하려면 별도의 플러그인을 설치해야 함.

4. Babel 플러그인
    - 설치가 필요한 플러그인은 Babel홈페이지에서 검색
    - 설치한 플러그인은 .babelrc의 plugins에 추가해야 함

5. 브라우저에서 모듈 로딩 테스트
    - nodejs 환경에서 Babel이 모듈을 트랜스파일링 한 것은 nodejs가 기본 지원하는 CommonJS방식의 module loading system에 따른것임.
    - 브라우저는 CommonJS 방식의 module loading system(require함수)를 지원하지 않으므로 트랜스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생햐게 됨.
    - 브라우저의 ES6 모듈 기능을 사용하도록 Babel을 설정할 수 있으나 이는 문제가 있음. Webpack을 사용해야 함.

6. Webpack 이란
    - Webpack은 의존 관계에 있는 모듈들을 하나의 자바스크립트 파일로 번들링하는 모듈 번들러임.
    - 이를 사용하면 의존모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없음.
    - Webpack이 자바스크립트 파일을 번들링하기 전에 Babel을 로드해 ES6+코드를 ES5코드로 트랜스파일링하는 작업을 실행하도록 설정해야 함.

7. Webpack 설치
    - npm i -D webpack webpack-cli

8. babel-loader
    - Webpack이 모듈을 번들링할때 Babel을 사용해 ES6+코드를 ES5코드로 트랜스파일링하도록 babel-loader 설치
    - npm i -D babel-loader
    - npm script를 변경해 Babel 대신 Webpack을 실행하도록 수정
        - webpack -w

9. webpack.config.js
    - Webpack이 실행될때 참조하는 설정 파일
    - Webpack을 실행하게 되면 트랜스파일링은 Babel이 실행하고 번들링은 Webpack이 실행함. ``

10. babel-polyfill
    - Babel을 사용해 ES5이하로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아있을 수 있음. 
    - ES6의 Promise, Object.assign, Array.from 은 ES5이하로 트랜스파일링해도 대체할 ES5기능이 없어서 그대로 남아있게 됨.
    - 따라서 오래된 에서도 ES6+에서 새롭게 추가된 객체나 메소드를 사용하기 위해서는 @babel/polyfiil을 설치해야 함
    - npm i @babel/polyfill
    - babel-polyfill은 개발환경에서만 사용하는 것이 아니라 실제환경에서도 사용해야 하므로 --save-dev를 사용해 개발설치를 하지 않도록 함
    - ES6의 import를 사용하는 경우 파일의 진입점의 선두에 먼저 폴리필을 로드해야함
        - import "@babel/polyfill"
    - webpack을 사용하는 경우 위 방법 대신 webpack.config.js의 entry 배열에 추가
    - bundle.js를 확인하면 polyfill이 추가된것을 확인할 수 있음.

11. Sass 컴파일