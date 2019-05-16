#### 19.05.17

## map

map 은 배열의 각 원소들에 전달받은 함수를 적용하고 그 결과를 모아 새로운 배열을 만든다. 이 때 return이 없는 함수인 경우 undefined로 배열이 채워진다

예를들어 과일이름 배열에서 각 원소에 juice 라는 문자열을 덧붙여 새로운 배열을 만든다면

```js
cosnt juice = fruits.map(fruit => `${fruit} juice`);
```

## filter
객체 배열에서 어떤 특정 조건에 맞는 원소들로만 배열을 만들기

```js
// type이 post인 조건
const filteredData = datas.filter( data => {
  return data.type === 'post';
});
```

```js
// like가 5개 이상인 comment 찾는 복수 조건
const filteredData = datas.filter( data => {
  return data.type === 'comment'&& data.like > 5;
});
});
```

## find
filter 와 비슷해보이지만 find는 배열 원소에 대해서 주어진 함수연산을 하다가 함수가 true를 반환하면 find 역시 종료되는 차이가 있다

find함수로 조건에 만족하는 원소를 반환하지 못하는 경우 undefined 를 반환

```js
const ret = datas.find( data => {
	return data.id === 10;
});
```

## findIndex
find와 유사하고 해당 element 대신 element의 index를 리턴한다

## splice
splice() 메서드는 배열의 기존 요소를 삭제 또는 교체 하거나 새 요소를 추가여 배열의 내용을 변경한다

###
- [ES6 array helper method들을 알아보자](https://gnujoow.github.io/dev/2016/10/14/Dev6-es6-array-helper/)
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/