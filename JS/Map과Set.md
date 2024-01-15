- JS -> 객체, 배열 : 많고, 다양하고, 복잡한 프로그램을 만들어 왔지만 현실세계를 반영하기는 좀 많이 어려웠음
- Map, Set 추가적인 자료구조가 등장
- 기존의 객체나 배열보다 데이터의 구성, 검색, 사용을 효율적으로 처리

## 1. Map

- Key / Value
- Key에 어떤 데이터 타입도 다 들어올 수 있다. (객체와의 차이점)
- Map은 키가 정렬된 순서로 저장되기 때문이다.
- 기능 : 검색, 삭제, 제거, 여부 확인

<aside>
💡 맵의 주요 메서드와 프로퍼티

- `new Map()` – 맵을 만든다.
- `map.set(key, value)` – `key`를 이용해 `value`를 저장한다.
- `map.get(key)` – `key`에 해당하는 값을 반환한다. `key`가 존재하지 않으면 `undefined`를 반환한다.
- `map.has(key)` – `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.
- `map.delete(key)` – `key`에 해당하는 값을 삭제한다.
- `map.clear()` – 맵 안의 모든 요소를 제거한다.
- `map.size` – 요소의 개수를 반환한다.
</aside>

```
const myMap = new Map();
myMap.set('key', 'value');

// ...
// ...

myMap.get('key')
// 반복 ... !! -> method: keys, values, entries
```

### <**Map의 반복>**

**Map**에서는 **keys()**, **values()**, **entries()** 메소드를 사용하여 키, 값 및 키-값 쌍을 반복할 수 있다.

<aside>
💡for …of 반복문

`for of` 반복문은 ES6에 추가된 새로운 컬렉션 전용 반복 구문입니다. `for of` 구문을 사용하기 위해선 컬렉션 객체가 `[Symbol.iterator]` 속성을 가지고 있어야만 합니다(직접 명시 가능).

```jsx
var iterable = [10, 20, 30];

for (var valueof iterable) {
  console.log(value);// 10, 20, 30
}
```

</aside>

```jsx
const myMap = new Map();
myMap.set('one', 1);
myMap.set('two', 2);
myMap.set('three', 3);

console.log(myMap.keys()); //**[Map Iterator]** { 'one', 'two', 'three' }

for (const key of myMap.keys()) {
    console.log(key); 
} 
// one 
// two  
// three

for (const value of myMap.values()) {
    console.log(value);
}
//1
//2
//3

console.log(myMap.entries()); //[Map Entries] { [ 'one', 1 ], [ 'two', 2 ], [ 'three', 3 ] }
for (const entry of myMap.entries()) {
    console.log(entry);
}
//[ 'one', 1 ]
//[ 'two', 2 ]
//[ 'three', 3 ]
```

**<Map의 크기 및 존재 여부 확인>**

**Map**의 크기를 확인하려면 **size** 속성을 사용한다.

```jsx
console.log(myMap.size); // 3 출력
```

특정 키가 **Map**에 존재하는지 여부를 확인하려면 **has()** 메소드를 사용한다.

```jsx
console.log(myMap.has('two')); // true 출력
```

## 2. Set

- 고유한 값을 저장하는 자료구조
- 값만 저장
- 키는 저장하지 않음
- 값이 중복되지 않는 유일한 요소로만 구성된다(집합)
- 값 추가, 검색, 삭제, 모든 값 제거, 존재 여부 확인

### <**`Set` 생성 및 사용>**

1. Set() 생성자
    
    ```jsx
    const mySet = new Set();
    ```
    
2. add()
    
    ```jsx
    mySet.add('value1');
    mySet.add('value2');
    ```
    
3. size(), has()
    
    ```jsx
    console.log(mySet.size()); //2
    console.log(mySet.has('value1')); // true 출력
    ```
    

### <**`Set`의 반복>**

```jsx
const mySet = new Set();
mySet.add('value1');
mySet.add('value2');

for (const value of mySet.values()) {
  console.log(value);
}
//value1
//value2

```