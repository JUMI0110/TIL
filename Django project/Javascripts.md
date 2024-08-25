# 자바스크립트?
- 객체 기반의 스크립트 언어
-  HTML로는 웹의 내용 작성하고, CSS로는 웹의 디자인, 자바스크립트로는 웹의 동작을 구현 
- 주로 웹 브라우저에서 사용되나 Node.js와 같은 프레임워크를 사용하면 서버 측 프로그래밍에서도 사용 가능
- 대부분의 웹 브라우저에는 자바스크립트 인터프리터가 내장되어 있음

## 특징
1. 자바스크립트는 객체기반의 스크립트 언어
2. 동적이며, 타입을 명시 할 필요가 없는 인터프리터 언어
3. 객체지향형 프로그래밍과 함수형 프로그래밍을 모두 표현할 수 있음   
→ 인터프리터 언어 : 컴파일 작업을 거치지 않고, 소스 코드를 바로 실행 할 수 있는 언어

## 기본 문법
### querySelector()
- document 내의 요소를 검색하고 여러 결과를 찾았다면 첫번째 요소만 리턴해주는 메소드
- CSS 선택자 또는 선택자 그룹과 일치하는 요소를 찾고 찾지 못한 경우 null을 리턴
- 매개 변수로 유효한 CSS선택자 지정하지 않으면 SyntaxError 발생
- querySelectorAll() : CSS 선택자 또는 선택자 그룹과 매치되는 모든 요소의 목록을 리턴
```js
document.querySelector('h1')
```

### console.log
- 가장 기본적인 디버깅(출력)방법
```js
console.log('hello')
```
- 콘솔에 로그를 남긴다 → 콘솔(브라우저 안에 있는 콘솔 탭)에 특정 값을 보여주는 것 

### Variable(변수)   
- 변수 : 자료를 임시로 저장하는 공간 선언 후 데이터 할당 
- var : 재선언, 재할당 가능
```js
var name = 'kim'
var name = 'park'
```
- let : 재할당 가능, 재선언 불가능
```js
let name = 'kim'
name = 'park'
```
- const : 재선언, 재할당 불가능 (오브젝트 안의 데이터는 변경 가능)
```js
const name = 'kim'
```
### 데이터 타입
1. number : 숫자타입
2. string : 문자열
3. boolean :  true, false 참과 거짓 확인
4. array : []배열 (숫자, 텍스트, 불리언)
5. object : 여러가지 데이터가 모여있는 묶음 {key: value}
6. function : 함수형 

### 조건문
- 비교 연산자 
> `==` equal(값)   
`!=` not equal   
`===` equal(값, 타입)   
`!==` not equal
-  조건이 참인지 거짓인지 확인
```js
if(조건){
    조건에 맞으면 나타낼 식
}else if(조건){
    조건에 맞으면 나타낼 식
}else{
    조건에 맞지 않으면 나타낼 식
}
```

### 반복문
```js
for(반복조건){}
while(반복조건){}
```


### 함수
- function  함수는 변수와 마찬가지로 선언을 하고 시작
- 선언을 하면 실행도 해줘야 함
- 함수를 정의 = 함수를 호출
- 기본형
```js
function multiply(num1, num2){
    let result = num1 * num2
    return result
}

console.log(multiply(3, 5))
```
✔️`return`은 결과물을 반환하는 것 눈에 바로 보이지 않음

- 화살표 함수
```js
let multiply2 = (num1, num2) =>{
    let result = num1 * num2
    return result
}
console.log(multiply2(3, 4))
```
- 화살표 함수 생략 1 : {}안에 코드가 return하는 문장하나만 있는 경우   {}, return 생략가능
```js
let multiply3 = (num1, num2) => num1 * num2
console.log(multiply3(3, 4))
```
- 화살표 함수 생략 2 : ()안에 매개변수가 하나만 있으면 ()생략가능
```js
let multiply4 = num => num * 2
console.log(multiply4(4))
```
### 이벤트
- 웹 페이지에서 마우스를 클릭, 키를 입려, 특정요소에 포커스가 이동되었을 때 어떤 사건을 발생시키는 것
- 마우스, 키, 폼, 로드 등 여러 이벤트 있음 (필요할 때 찾아서 사용)
```js
// 클릭 했을 때
document.querySelector('html').onclick = function(){
    alert('ouch!!!')
     console.log('hello')
}
```
- addEventListener(무슨일이 일어났을 때, 무슨행동을 할 지 ){} : 이벤트를 등록
```js
let myH1 = document.querySelector('h1')
// addEventListener(무슨일이 일어 났을때, 무슨행동을 할지)
myH1.addEventListener('click', function(e){
    console.log('hello')
    console.log(e)
    console.log(e.target)
}) 
```
- 비동기 처리 (요청과 결과가 동시에 일어나지 않을 거라는 약속)
> __promise__   
```js
const URL = 'https://jsonplaceholder.typicode.com/todos/1'
fetch(URL)
    .then(response => response.json())
    .then(json => console.log(json))

console.log('after fetch')
```
> __async await__   
```js
async function fetchAndPrint(){
    let res = await fetch(URL)
    let result = await res.json()

    // console.log(result)
}
fetchAndPrint()
```
## 참고
[basic]https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics

[practice]http://mdn.github.io/beginner-html-site-styled/





