# Vue CLI 프로젝트 기반 DevOps 개발환경 실습

## 사전 환경구성
https://classic.yarnpkg.com/en/docs/install <br>
yarn 설치 후, Vue CLI를 설치해서 버전 확인 <br>
```
yarn global add @vue/cli 
vue -V
```

## Vue 프로젝트 생성
```
vue create vue-devops
```
정보 설정이 필요한 경우<br>
```
git config --global user.name "이지훈"
git config --global user.email dlwlgns102@naver.com
```
프로젝트 생성 과정에서 아래와 같은 순서로 선택<br>
Manual select features<br>
Unit Testing 추가 선택<br>
3.x<br>
ESLint + Prettier<br>
Lint on save<br>
Jest<br>
In dedicated config files<br>
N<br><br>
웹브라우저에서 확인<br>
```
cd vue-devops
yarn serve
```


## 깃허브에 코드 push 및 pages 수동 배포
깃허브에 vue-devops 프로젝트 생성하기<br>
```
git remote add origin [주소]
git push -u origin master
```
깃허브 페이지로 배포하기 위한 라이브러리 추가 및 package.json에서 배포에 필요한 정보 추가<br>
```
yarn add gh-pages -D
```
![image](https://user-images.githubusercontent.com/50227342/123760097-7e29ed80-d8fb-11eb-827f-71bd3fe5cd4b.png) <br>
배포용 publicPath 설정 - 프로젝트 최상단에 vue.config.js 파일 생성<br>
![image](https://user-images.githubusercontent.com/50227342/123760135-85e99200-d8fb-11eb-8675-40b9f86f4d55.png) <br>
yarn deploy 명령을 실행하여 빌드된 정적 파일을 푸시<br>
```
yarn deploy
```
로그인 실패후 재시도 경우, yarn clean후 재시도
```
yarn clean
```
깃허브 설정에서 배포 주소 확인 가능<br>
![image](https://user-images.githubusercontent.com/50227342/123760166-8da93680-d8fb-11eb-9ba9-77245bed88b6.png)<br>

이렇게 yarn deploy 명령어로 수동으로 빌드된 정적 파일을 배포할 수 있음<br>

## 깃허브 Actions workflow로 배포 자동화
Action 탭에서 Set up this workflow 버튼을 클릭<br>
![image](https://user-images.githubusercontent.com/50227342/123760184-93068100-d8fb-11eb-94d2-73714560b061.png)<br>
![image](https://user-images.githubusercontent.com/50227342/123760210-98fc6200-d8fb-11eb-8604-3d32e75c3490.png)<br>
커밋 후 workflow가 동작되는 것을 확인<br>
![image](https://user-images.githubusercontent.com/50227342/123760230-9dc11600-d8fb-11eb-9ae1-ac69fbdffa6e.png)<br>
git pull로 코드를 내려 받아서 로컬 PC에 적용<br>
deploy.yml파일을 다음과 같이 수정<br>
![image](https://user-images.githubusercontent.com/50227342/123760263-a3b6f700-d8fb-11eb-9410-846689e333c1.png)<br>
커밋 & 푸시<br>
변경 사항을 workflow와 배포된 사이트에서 확인 가능<br>
테스트 코드 추가 및 테스트 실패로 인한 자동 배포 실패 확인<br>
deploy.yml에 단위 테스트 추가<br>
![image](https://user-images.githubusercontent.com/50227342/123760295-aade0500-d8fb-11eb-8762-6a3a32fc3dc7.png)<br>
코드를 일부로 틀리게 작성하고 푸시하면 다음과 같이 화면이 뜬다<br>
![image](https://user-images.githubusercontent.com/50227342/123760320-b03b4f80-d8fb-11eb-8afe-d46aacc4c9d1.png)<br>
또한 배포 사이트를 가보면, 틀린 내용이 들어간 푸시가 아예 적용되지 않았음을 확인할 수 있다<br>
