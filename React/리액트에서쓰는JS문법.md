## 상수와 변수

이제는 const(상수)와 let(변수)를 이용

<aside>
❗ const : 재할당 안됨. 내부 속성값은 수정 가능. 변함없는 수
let : 재할당 가능

* 둘 다 block level scope를 가짐 → {} 안에서만 선언한 변수 유효해야 함

</aside>

```jsx
if (true) {
  var a = 1;
  // const a = 1;
}

console.log(a);
```

## Object 선언, 단축 속성, 객체 복사

<aside>
❗ 가장 중요한 것!
object는 key-value pair!

</aside>

### 객체를 선언하는 방법

```jsx
// 선언방법
const person = {
	name: '르탄',
	age: 21,
	company: 'sparta coding club',
	doSomething: () => {},
}
```

### 생략해서 표현하는 방법

```jsx
const name = '르탄';
const age = 21;

/** 단축속성 : key - value가 일치하면 생략 */

// 변경 전
const person = {
	name: name,
	age: age,
	company: 'sparta coding club',
	doSomething: function(){},
}

// 변경 후
const person = {
	name,
	age,
	company: 'sparta coding club',
	doSomething: function(){},
}
```

### 복사 주의! (얕은 복사)

```jsx
const obj1 = { value1: 10 };
const obj2 = obj1; // 얕은 복사
const obj3 = JSON.parse(JSON.stringify(obj1)) //새로운 주소값 생성

obj1.value1 += 1;

console.log(`obj1:`, obj1); //11
console.log(`obj2:`, obj2); //11
console.log(`obj3:`, obj3); //10
```

- 결론적으로는, `const obj2 = obj1;` 이런 식의 복사 방법(얕은 복사)은 주의하기!

## Template Literals

```jsx
// 일반 텍스트
`string text`

// 멀티라인
`string text line 1
 string text line 2`

// 플레이스 홀더를 이용한 표현식
`string text ${expression} string text`
```

## 배열/객체 비구조화 (Array/Object Destructuring)

**구조분해 할당**

### 객체의 비구조화(구조분해 할당)

```jsx
// person 객체 안에 있는 값들이 구조가 해제되어 각각 변수에 할당

const person = {
	name: '르탄',
	age: '21'
}

const { name, age } = person;
console.log(`${name}님, ${age}살이시네요!`);
```

```jsx
// 함수의 입력값 또한 각각 구조가 해제되어 각각 변수에 할당
**// 특히 이 패턴을 많이 쓰니까 기억하기!1**

const person = {
	name: '르탄',
	age: '21'
}

function hello({name, age}) {
	console.log(`${name}님, ${age}살이시네요!`);
}

hello(person);
```

### 배열의 비구조화(구조분해 할당)

```jsx
// 객체에서의 케이스와 마찬가지로 배열도 구조분해 할당이 이루어짐

const testArr = [1, 2, 3, 4, 5];
const [val1, val2, val3, val4, val5] = testArr;

console.log(val1);
```

```jsx
// 추가 예제

let [name] = ["Tom", 10, "Seoul"];
// let [, age] = ["Tom", 10, "Seoul"];
// let [name, age, region] = ["Tom", 10, "Seoul"];
// let [name, age, region, height] = ["Tom", 10, "Seoul"];
// let [name, age, region, height = 150] = ["Tom", 10, "Seoul"];
```

## 전개 연산자 (Spread Operator)

전개연산자(…)

CASE 1

```jsx
let [name, ...rest] = ["Tom", 10, "Seoul"];
console.lod(rest) = [10, "Seoul"] //배열로 
```

CASE 2

```jsx
let names = ["Steve", "John"];
let students = ["Tom", ...names, ...names];
```

CASE 3

```jsx
let tom = {
  name: "Tom",
  age: 10,
  region: "Seoul",
};

let steve = {
  ...tom,
  name: "Steve",
};
```

## Arrow Functions

```jsx
const mysum1 = (x, y) => x + y;
const mysum2 = (x, y) => {x, y};
const mysum3 = (x, y) => ({x: x, y: y}); const mysum4 = (x, y) => {
  return {x: x, y: y};
}
const mysum5 = function(x, y) {
  return {x: x, y: y};
};
function mysum6(x, y) {
  return {x: x, y: y};
}
```