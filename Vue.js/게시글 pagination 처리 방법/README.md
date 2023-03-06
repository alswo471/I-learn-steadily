# Vue.js를 사용하여 게시글 페이지네이션을 처리하는 방법

</br>

### 1. 데이터 가져오기

- 서버에서 게시글 데이터를 가져옵니다. 이때, 게시글의 총 개수와 한 페이지에 보여줄 게시글의 개수도 함께 가져옵니다.

 </br>

### 2. 데이터 처리하기

- 가져온 게시글 데이터를 Vue 인스턴스의 data 객체에 저장합니다. 이때, 현재 페이지 번호와 총 페이지 수도 계산하여 data 객체에 저장합니다

 </br>

### 3. 템플릿 작성하기

- 게시글 목록을 보여주는 템플릿을 작성합니다. v-for 디렉티브를 사용하여 게시글 목록을 출력합니다. 또한, v-if 디렉티브를 사용하여 현재 페이지에 해당하는 게시글만 출력하도록 처리합니다.

 </br>

### 4. 페이지네이션 구현하기

- v-for 디렉티브를 사용하여 페이지네이션을 구현합니다. 현재 페이지를 기준으로 이전 페이지와 다음 페이지를 이동할 수 있는 버튼을 만듭니다. 또한, 페이지 번호를 클릭하여 해당 페이지로 이동할 수 있는 링크를 만듭니다.

 </br>

다음은 위 방법을 코드로 구현한 예시입니다.

```html
<template>
  <div>
    <div v-for="post in displayedPosts" :key="post.id">
      <h2>{{ post.title }}</h2>
      <p>{{ post.content }}</p>
    </div>
    <div>
      <button v-if="currentPage > 1" @click="currentPage--">이전 페이지</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button v-if="currentPage < totalPages" @click="currentPage++">
        다음 페이지
      </button>
    </div>
  </div>
</template>
```

```javascript
<script>
export default {
  data() {
    return {
      posts: [], // 게시글 데이터
      currentPage: 1, // 현재 페이지 번호
      postsPerPage: 10, // 한 페이지에 보여줄 게시글의 개수
    };
  },
  computed: {
    displayedPosts() {
      // 현재 페이지에 해당하는 게시글 목록을 반환하는 computed 속성
      const startIndex = (this.currentPage - 1) * this.postsPerPage;
      const endIndex = startIndex + this.postsPerPage;
      return this.posts.slice(startIndex, endIndex);
    },
    totalPages() {
      // 총 페이지 수를 계산하는 computed 속성
      return Math.ceil(this.posts.length / this.postsPerPage);
    },
  },
  mounted() {
    // 서버에서 게시글 데이터를 가져오는 API 호출
    fetch('/api/posts')
      .then((response) => response.json())
      .then((data) => {
        this.posts = data.posts;
      });
  },
};
</script>
```
