</br>

# Vue.js에서 배열을 문자열로 바꾸는 방법

</br>

**Vue.js에서 배열을 문자열로 바꾸는 방법은 여러 가지가 있습니다. 이 중에서 가장 간단한 방법은 JavaScript의 join() 메소드를 사용하는 것입니다. join() 메소드는 배열의 각 요소를 문자열로 결합하여 하나의 문자열로 만들어 줍니다.**

</br></br>

> 다음은 Vue.js에서 join() 메소드를 사용하여 배열을 문자열로 바꾸는 예시입니다.

</br>

```javascript
<template>
  <div>
    <p>{{ items.join(", ") }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: ["apple", "banana", "orange"],
    };
  },
};
</script>
```

</br>

- 위 코드에서 items 배열의 각 요소를 쉼표와 공백으로 구분하여 하나의 문자열로 결합하고 있습니다. join() 메소드를 사용하여 문자열을 만들고, 이를 Vue.js 템플릿에서 출력하고 있습니다.

- 만약 다른 구분자를 사용하고 싶다면, join() 메소드의 매개변수로 구분자를 전달하면 됩니다.

- 위 코드에서는 각 요소를 하이픈으로 구분하여 문자열로 결합하고 있습니다. join() 메소드를 이용하면 간단하게 배열을 문자열로 바꿀 수 있습니다.
