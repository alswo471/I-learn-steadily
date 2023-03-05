## Springboot - 생성자 주입 , 필드 주입 방식, 수정자 주입 방식의 차이점 & 생성자 주입 방식을 권하는 이유?

</br>

```java
@Autowired
InquiryService inquirService;
```

- 위에 코드를 보면 나는 컨트롤러를 구성할 때 필드 주입 방식을 사용하였다. 이렇게 컨트롤러를 구성했을 때 선배들한테 피드백을 받았다. 필드 주입 방식이 아닌 생성자 주입 방식으로 처리를 하라는 피드백 이었다. 나는 궁금해서 생성자 주입 방식 필드 주입 방식 수정자 주입 방식의 차이점을 공부를 해보았다.

</br>
 
### 우선 위에 설명 했듯이 스프링 프레임워크에 의존성을 주입하는 방식은 3가지가 있다.

</br>

- 1. 생성자 주입

```java
public class AdminController {

private final MemberService memberservice;

	private final AdminService adminService;

	@Autowired // (단일 생성자일 경우는 스프링 4.3 부터 @Autowired 생략 가능)
	public AdminController(MemberService memberservice, AdminService adminService) {
		this.memberservice = memberservice;
		this.adminService = adminService;
	}
    }
```

</br>

- 2. 필드 주입

```java
@Autowired
Memberservice memberservice;
```

</br>

- 3. 수정자 주입

```java
public class AdminController{

private Memberservice memberservice

@Autowired
public void setMemberservice(Memberservice memberservice) {
	this.memberservice = memberservice;
}

}
```

</br>

### 스프링 팀에서는 생성자 주입 방식을 추천하고 있다고 한다.

 </br>

**_왜 일까?_**

- 첫번째로 순환 참조 방지이다.
- 순환 참조는 A -> B 를 참조하면서 B -> A 를 참조할 때 문제가 발생한다.
- 서버를 구동 시키면 운영중에 메서드가 실행되면서 순환참조로 인해 서버가 죽는다.
- 이것을 방지하기 위해서 생성자 주입 방식으로 변경하게 되면 바로 에러가 발생 되면서 순환 참조를 알 수 있고 방지할 수 있다. 즉 서버가 구동되지 않는다.

 </br>

**_이러한 차이의 이유는??_**

</br>

- 3가지의 주입 방식 중에서 생성자 주입 방식은 빈을 주입하는 순서가 다르다 .
- 필드 주입 과 수정자 주입은 빈을 먼저 생성 후 주입 하는 방식이고 ,
- 생성자 주입은 먼저 빈을 생성하지 않고 주입하려는 빈을 먼저 찾는다.

### 두번째는 final 선언 가능이다.

</br>

- 필드주입과 수정자 주입은 final 을 선언할 수 없지만,

- 생성자 주입 방식은 가능하다. (런타임 객체 불변성을 보장한다.)
