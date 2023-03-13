## 1. 개요

<br>

조회수 중복 방지는 웹사이트나 애플리케이션에서 매우 중요한 기능 중 하나입니다. 중복된 조회수를 제대로 처리하지 않으면, 신뢰성 있는 데이터를 제공할 수 없고, 광고 수익을 추적하는 등의 목적에도 문제가 생길 수 있습니다. 조회수 중복 방지를 위해서는 다음과 같은 방법을 사용할 수 있습니다.

<br>

## 2. 방법

<br>
 
* **쿠키를 사용한 중복 방지**

쿠키를 사용하여 중복된 조회수를 방지할 수 있습니다. 이 방법은 간단하게 사용자의 브라우저에 쿠키를 생성하여, 하루 동안의 중복 조회를 막을 수 있습니다. 이 방법의 단점은 사용자가 쿠키를 삭제하거나 다른 브라우저를 사용할 경우 중복 조회가 가능하다는 점입니다.

<br>

- **IP 주소를 사용한 중복 방지**

사용자의 IP 주소를 기록하여 중복 조회를 방지할 수 있습니다. 이 방법은 쿠키와 달리, 사용자가 브라우저를 변경하거나 쿠키를 삭제해도 중복 조회를 방지할 수 있습니다. 하지만, 여러 사용자가 같은 IP 주소를 사용하는 경우나, IP 주소가 공유될 경우 정확한 조회수를 계산하기 어렵다는 단점이 있습니다.

<br>

- **로그인을 사용한 중복 방지**

로그인을 사용하여 중복 조회를 방지할 수 있습니다. 이 방법은 사용자가 로그인한 경우에만 조회수를 누적하도록 구현할 수 있습니다. 로그인한 사용자만 조회수를 누적하므로, 더 정확한 조회수를 계산할 수 있습니다. 하지만, 로그인을 하지 않은 사용자의 조회수를 제대로 측정할 수 없다는 단점이 있습니다.

<br>

- **세션을 사용한 중복 방지**

세션을 사용하여 중복 조회를 방지할 수 있습니다. 이 방법은 사용자의 브라우저가 서버에 접속한 시간을 기록하여, 일정 시간 동안 중복 조회를 막는 방법입니다. 이 방법은 사용자가 로그인하지 않은 경우에도 사용할 수 있으며, 로그인한 사용자의 경우에는 세션을 더욱 활용할 수 있습니다.

조회수 중복 방지를 위해서는 위의 방법을 사용하거나, 이를 조합하여 사용할 수 있습니다. 하지만, 이러한 방법을 사용하여도 완전한 중복 방지를 보장할 수는 없으므로, 다른 방법들과 함께 사용하여 최대한의 보안성을 확보하는 것이 좋습니다.

<br>
 
## 여기서 저는 쿠키를 사용하는 방식으로 해보았습니다! 


- 설치법

```javascript
import VueCookies from "vue-cookies";
```

<br>

- 등록

```javascript
import VueCookies from "vue-cookies";
```

 <br>

## back

 <br>
 
* postController.java

```java
/**
	 * 게시글 조회수
	 * @param postDto
	 * @param 정상 : 상태코드 OK 200 반환
	 * @return
	 */
	@PutMapping("viewsUpdate")
	public ResponseEntity<PostDto> viewsUpdate(@RequestBody PostDto postDto) {

		log.info("PostController/viewsUpdate 진입");

		PostDto result = postService.viewsUpdate(postDto);
		return ResponseEntity.ok(result);
	}
```

 <br>

- post.xml

```java
<update id="viewsUpdate" parameterType="int">
		UPDATE post SET views = views + 1 WHERE post_seq = #{postSeq}
	</update>
```

<br>

## front

 <br>

```javascript
methods: {

    // 게시글 리스트

    async getPostList() {
     생략..

        // 댓글 리스트
	axios
      생략..

        // 게시글 조회수 증가 및 중복 방지 코드

      const postId = this.$route.params.id
      const viewed = VueCookies.get('viewed')

      if (!viewed || !viewed.includes(postId)) {
        axios
          .put('/api/post/viewsUpdate', this.commentData, {
            withCredentials: true
          })
          .then((response) => {
            if (response.status === 200) {
              console.log('viewsUpdate', response.data)
              this.veiwCount = response.data
              const viewed = VueCookies.get('viewed') || ''
              VueCookies.set('viewed', `${viewed},${postId}`, 86400)
              console.log(VueCookies.get('viewed'))
            }
          })
      } else {
        console.log('중복 조회 차단')
      }
    },
    ~~~

   <br>



<br>

### 코드 설명

<br>

* VueCookies를 사용하여 쿠키를 생성하고, 조회수 중복 방지를 구현합니다. 이미 조회수를 누적한 경우에는 데이터베이스에서 게시물을 조회하고, 그렇지 않은 경우에는 데이터베이스에 조회수를 누적합니다. 이때, VueCookies를 사용하여 쿠키를 생성하고, 하루 동안 유지되도록 설정합니다.

 <br>

~~~javascript
if (!viewed || !viewed.includes(postId))
```

- !viewed.includes(postId)은 문자열 viewed가 postId를 포함하고 있는지 여부를 확인하는 코드입니다.

- **VueCookies.get('viewed')** 를 통해 브라우저에서 쿠키에 저장된 **viewed** 값을 가져옵니다. 이 값은 문자열 형태이며, 여러 개의 게시물 ID가 쉼표(,)로 구분되어 저장되어 있습니다.

- **!viewed.includes(postId)** 은 **viewed** 문자열이 **postId** 를 포함하지 않을 경우 **true** 를 반환합니다. 즉, 이 코드는 이미 해당 게시물을 조회한 경우에는 **false** 를 반환하여 조회수를 누적하지 않도록 방지합니다.

  <br>

```javascript
const viewed = VueCookies.get("viewed") || "";
VueCookies.set("viewed", `${viewed},${postId}`, 86400);
```

- 이 코드는 **Vue.js** 에서 **Vue-cookies** 라이브러리를 사용하여, 이미 조회한 게시물의 **ID** 를 쿠키에 저장하는 코드입니다.

- 먼저 **VueCookies.get('viewed')** 를 사용하여, **viewed** 라는 이름의 쿠키를 가져옵니다. 이때 만약 해당 쿠키가 존재하지 않는다면 **null** 을 반환합니다. 이를 방지하기 위해 || ''를 사용하여 쿠키가 존재하지 않을 때, 빈 문자열을 반환하도록 합니다.

- 다음으로, **VueCookies.set('viewed', ${viewed},${postId}, 1)** 을 사용하여 **viewed** 라는 이름의 쿠키를 생성하고 저장합니다. 이때, **postId** 는 새롭게 조회한 게시물의 ID입니다. 이를 기존의 **viewed** 값과 합쳐서 저장하도록 합니다. 즉, 기존에 조회한 게시물 ID들과 새로운 게시물 ID를 ,로 구분하여 하나의 문자열로 만들어 저장합니다. 또한, 86400은 쿠키의 만료 시간입니다. 이를 설정함으로써, 쿠키가 하루동안 유지되도록 합니다.
