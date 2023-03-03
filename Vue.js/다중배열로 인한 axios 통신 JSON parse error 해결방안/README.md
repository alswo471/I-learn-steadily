# 다중 배열로 인한 axios 통신 JSON parse error 해결방안

</br>

### 백엔드로 넘겨주고 싶은 데이터

</br>

```javascript
<template>
  <div>
    <!-- 템플릿 코드 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      sendDate: {
        memId: localStorage.getItem('memId'),
        callNum: '01040047591',
        recName: '',
        recNum: '',
        smsTitle: '',
        smsContent: ''
      },
      items: [
        {
          name: '홍길동',
          phone: '010-1234-1234'
        }
      ],
       mergedList: {}, // mergedList를 할당할 데이터 속성
    };
  },
  methods: {

   registSend() {
        this.$store.dispatch('Send/SEND_SAVE', this.mergedList)
    },
    addRow() {
      const newRow = {
        name: this.items.name,
        phone: this.items.phone
      };

      if (this.sendDate.smsTitle || this.sendDate.smsContent !== '') {
        this.items.push(newRow);
        this.mergedList = this.items.map((item) => ({
          ...this.sendDate,
          recName: item.name,
          recNum: item.phone
        }));
        console.log(this.mergedListData);
      } else {
        alert('문자 내용을 먼저 입력해주세요');
      }
    },
    // 다른 메소드에서 mergedList 사용
    exampleMethod() {
      console.log(this.mergedList);
    }
  }
};
</script>
```

</br>

### store - Send.js

</br>

```javascript
import axios from "axios";

const Send = {
  namespaced: true,
  state: {},
  getters: {},
  mutations: {},
  actions: {
    SEND_SAVE(context, sendDate) {
      axios
        .post("/api/send/sendSave", sendDate)

        .then((response) => {
          if (response.status === 200) {
            console.log(response);
          }
        })
        .catch((error) => {
          alert(error.response.data.message);
          console.log(error);
        });
    },
  },
};

export default Send;
```

- 위 코드에서 백엔드와 통신해 데이터를 넘겨주어 DB 에 담고 싶은데 콘솔에 출력해보니 데이터가 [Proxy, Proxy]와 같은 형태로 출력되었는데 알아본 결과 [Proxy, Proxy] 로 출력되는 이유는 Vue에서 데이터를 관찰하는 방식 때문이라고합니다.

- Vue는 데이터 변화를 감지하기 위해 객체에 Proxy를 적용하며, 이 Proxy 객체가 관찰 중인 데이터의 변화를 감지합니다. 따라서 Vue에서 데이터를 관리하는 객체를 콘솔에 출력하면 Proxy 객체가 출력되는 것입니다.

- 백엔드에서 JSON 에러가 발생하는 이유는 데이터가 올바른 JSON 형식이 아니기 때문입니다. Vue에서 [Proxy, Proxy]와 같은 형태로 출력되는 데이터는 JSON.stringify() 메서드를 이용해 JSON 문자열로 변환할 수 없습니다. 그러므로 백엔드에서 JSON 형식으로 변환하려면 Vue의 데이터를 직접 사용하는 것이 아니라 데이터를 복제한 후 사용해야 합니다.

- Vue에서 데이터를 복제하려면 JSON.parse(JSON.stringify(data))와 같이 사용하면 됩니다. 이렇게 하면 Vue 데이터의 복제본을 만들어 JSON 형식으로 변환할 수 있습니다. 예를 들어 다음과 같이 데이터를 복제하고 JSON 문자열로 변환하여 백엔드에 전달할 수 있습니다.

- 제가 진행하는 프로젝트는 주소록에 [이름], [전화번호] 를 여러명을 추가하여 다중 발송 기능을 구현하고 있었습니다. 이 프로젝트에 위에 설명드린 방법을 시도해보았습니다.

</br>

### 수정 코드

</br>

```javascript
registSend() {
      const clonedData = JSON.parse(JSON.stringify(this.mergedList))
      console.log('clonedData', clonedData.length)
      for (let i = 0; i < clonedData.length; i++) {
        this.$store.dispatch('Send/SEND_SAVE', clonedData[i])
      }
      // const jsonData = JSON.stringify(clonedData)
    },
```

> const clonedData 객체를 선언하고 JSON.parse(JSON.stringify(this.mergedList)) 을 clonedData 객체에 담아주었고 담아준 객체를 for 을 이용해서 clonedData[i] 배열에 담긴 인덱스를 하나씩 $store.dispatch 하여 actions을 실행시켜 통신하였습니다.
