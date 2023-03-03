# Vue.js에서 두 개의 key-value 배열을 합치는 방법

</br>

**JavaScript에서 제공하는 Object.assign() 메소드를 사용하면 된다.
Object.assign() 메소드는 인자로 전달된 객체를 병합하여 새로운 객체를 반환합니다. 이를 이용하여 두 개의 배열을 병합할 수 있습니다.**

</br></br>

> **예시 코드입니다.**

```javascript
let obj1 = { a: 1, b: 2 };
let obj2 = { c: 3, d: 4 };

let merged = Object.assign({}, obj1, obj2);

console.log(merged); // { a: 1, b: 2, c: 3, d: 4 }
```

</br>

- 위 예시에서 Object.assign() 메소드의 첫 번째 인자는 새로운 객체를 생성하기 위한 빈 객체 {}이며, 두 번째와 세 번째 인자는 병합할 객체 obj1과 obj2입니다. 반환된 merged 객체에는 obj1과 obj2의 key-value 쌍이 모두 포함됩니다.

</br>

- 따라서, 두 개의 key-value 배열을 합치기 위해서는 해당 배열을 객체로 변환하고, Object.assign() 메소드를 이용하여 병합한 후, 다시 배열로 변환하는 과정이 필요합니다.
