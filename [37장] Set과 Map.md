# 37. Set과 Map

## 1. Set

- `Set` 객체는 **중복되지 않은 유일한 값들의 집합**이다.

### 1.1 Set 객체의 생성

- `Set` 객체는 `Set` 생성자 함수로 생성한다.
- `Set` 생성자 함수에 인수를 전달하지 않으면 빈 `Set` 객체가 생성된다.

```js
const set = new Set();
console.log(set); // Set(0) {}
```

- **`Set` 생성자 함수는 이터러블을 인수로 전달받아 `Set` 객체를 생성한다.**
- **이때 이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않는다.**

```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set("Hello");
console.log(set2); // Set(4) {"H", "e", "l", "o"}
```

🔥 `Set` 활용

- 중복을 허용하지 않는 `Set` 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.

```js
// 배열의 중복 요소 제거
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = (array) => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 1.2 요소 개수 확인

- `Set` 객체의 요소 개수를 확인할 때는 `Set.prototype.size` 프로퍼티를 사용한다.

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 1.3 요소 추가

- `Set` 객체의 요소를 추가 때는 `Set.prototype.add` 메서드를 사용한다.

```js
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

- `add` 메서드는 새로운 요소가 추가된 `Set` 객체를 반환한다.
- 따라서 `add` 메서드를 호출한 후에 `add` 메서드를 **연속적으로** 호출할 수 있다.
- `Set` 객체에 중복된 요소의 추가는 허용되지 않는다.

```js
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}

set.add(2);
console.log(set); // Set(2) {1, 2}
```

🔥 주의

- `Set` 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.
- ‼️ 생성자 함수로 `Set` 객체를 생성할 때 인수로 이터러블을 넣어줘야만 한다. 그렇지 않으면 에러가 난다.
- ‼️ 그런데 `Set` 객체에 요소를 추가할 때는 인수가 이터러블인지 상관 없이 모든 값을 저장할 수 있다.

```js
const set = new Set();

set
  .add(1)
  .add("a")
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([])
  .add(() => {});

console.log(set); // Set(8) {1, "a", true, undefined, null, {}, [], () => {}}
```

### 1.4 요소 존재 여부 확인

- `Set` 객체에 특정 요소가 존재하는지 확인하려면 `Set.prototype.has` 메서드를 사용한다.
- `has` 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 1.5 요소 삭제

- `Set` 객체의 특정 요소를 삭제하려면 `Set.prototype.delete` 메서드를 사용한다.
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.
- `delete` 메서드에는 **인덱스가 아니라 삭제하려는 요소값을 인수로 전달**해야 한다.

```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}
```

- 존재하지 않는 `Set` 객체를 삭제하면 에러 없이 무시된다.
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다. 따라서 `Set.prototype.add` 메서드와 달리 연속적으로 호출할 수 없다.

```js
const set = new Set([1, 2, 3]);

set.delete(4);
console.log(set); // Set(3) {1, 2, 3}

set.delete(1).delete(2); // TypeError
```

### 1.6 요소 일괄 삭제

- `Set` 객체의 모든 요소를 일괄 삭제하려면 `Set.prototype.clear` 메서드를 사용한다.
- `clear` 메서드는 언제나 `undefined`를 반환한다.

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 1.7 요소 순회

- `Set` 객체의 요소를 순회하려면 `Set.prototype.forEach` 메서드를 사용한다.
- `Set.prototype.forEach` 메서드는
  - `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와
  - 콜백함수 내부에서 `this` 로 사용될 객체(옵션)를 인수로 전달한다.

```js
const set = new Set([1, 2, 3]);

set.forEach((value, value2, set) => console.log(value, value2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- `Set` 객체는 이터러블이다. 따라서 `for...of` 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

```js
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

### 1.8 집합 연산

- `Set` 객체는 수학적 집합을 구현하기 위한 자료구조다.

#### 1.8.1 교집합

- 교집합 A∩B 는 집합 A와 집합 B의 공통 요소로 구성된다.

```js
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}
```

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};
```

#### 1.8.2 합집합

- 합집합 A∪B 는 집합 A와 집합 B의 중복 없는 모든 요소로 구성된다.

```js
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합니다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};
```

#### 1.8.3 치집합

- 차집합 A-B는 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.

```js
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한 쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.union(setA)); // Set(0) {}
```

```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};
```

#### 1.8.4 부분 집합과 상위 집합

- 집합 A가 집합 B에 포함되는 경우 (A⊆B) 집합 A는 집합 B의 부분 집합이며, 집합 B는 집합 A의 상위 집합이다.

```js
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
console.log(setB.isSuperset(setA)); // false
```

```js
Set.prototype.isSuperset = function (subset) {
  const superSetArr = [...this];
  return [...subset].every((v) => superSetArr.includes(v));
};
```

## 2. Map

- `Map` 객체는 **키와 값의 쌍으로 이루어진 컬렉션**이다.

### 2.1 Map 객체의 생성

- `Map` 객체는 `Map` 생성자 함수로 생성한다.
- `Map` 생성자 함수에 인수를 전달하지 않으면 빈 `Map` 객체가 생성된다.

```js
const map = new Map();
console.log(map); // Map(0) {}
```

- `Map` 생성자 함수는 **이터러블**을 인수로 전달받아 Map 객체를 생성한다.
- 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```js
const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]);
console.log(map2); // TypeError
```

🔥 주의 사항!

- `Map` 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.
- 따라서 `Map` 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

### 2.2 요소 개수 확인

- `Map` 객체의 요소 개수를 확인할 때는 `Map.prototype.size` 프로퍼티를 사용한다.

```js
const { size } = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(size); // 2
```

### 2.3 요소 추가

- `Map` 객체의 요소를 추가 때는 `Map.prototype.set` 메서드를 사용한다.

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1");
console.log(map); // Map(1) {"key1" => "value1"}
```

- `set` 메서드는 새로운 요소가 추가된 `Map` 객체를 반환한다.
- 따라서 `set` 메서드를 호출한 후에 `set` 메서드를 연속적으로 호출할 수 있다.

```js
const map = new Map();

map.set("key1", "value1").set("key2", "value2");
console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

### 2.4 요소 취득

- `Map` 객체에서 특정 요소를 취득하려면 `Map.prototype.get` 메서드를 사용한다.
- `get` 메서드의 인수로 키를 전달하면 `Map` 객체에서 인수로 전달한 키를 갖는 값을 전달한다.
- `Map` 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 `undefined`를 반환한다.

```js
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // "developer"
console.log(map.get("key")); // undefined
```

### 2.5 요소 존재 여부 확인

- `Map` 객체에 특정 요소가 존재하는지 확인하려면 `Map.prototype.has` 메서드를 사용한다.
- `has` 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

### 2.6 요소 삭제

- `Map` 객체의 요소를 삭제하려면 `Map.prototype.delete` 메서드를 사용한다.
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.delete(kim); // true
console.log(map); // Map(1) {{ name: "Lee" } => "developer"}
```

- 존재하지 않는 `Map` 객체를 삭제하면 에러 없이 무시된다.
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다. 따라서 `Map.prototype.set` 메서드와 달리 연속적으로 호출할 수 없다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.delete(kim).delete(lee); // TypeError
```

### 2.7 요소 일괄 삭제

- `Map` 객체의 요소를 일괄 삭제하려면 `Map.prototype.clear` 메서드를 사용한다.
- `clear` 메서드는 언제나 `undefined`를 반환한다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.clear(); // undefined
console.log(map); // Map(0) {}
```

### 2.8 요소 순회

- `Map` 객체의 요소를 순회하려면 `MAp.prototype.forEach` 메서드를 사용한다.
- `Map.prototype.forEach` 메서드는
  - `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와
  - 콜백함수 내부에서 `this` 로 사용될 객체(옵션)를 인수로 전달한다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((value, key, map) => console.log(value, key, map));
/*
developer { name: "Lee" } Map(2) {{ name: "Lee" } => "developer", { name: "Kim" } => "designer"}
designer { name: "Kim" } Map(2) {{ name: "Lee" } => "developer", { name: "Kim" } => "designer"}

*/
```

- `Map` 객체는 이터러블이다. 따라서 `for...of` 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]); // [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링의 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
```

🔥 `Map` 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

- `Map.prototype.keys` : `Map` 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.
- `Map.prototype.values` : `Map` 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.
- `Map.prototype.entries` : `Map` 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map) {
  console.log(key); // { name: "Lee" } { name: "Kim" }
}

// Map.prototype.values Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map) {
  console.log(value); // developer designer
}

// Map.prototype.entries Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}
```
