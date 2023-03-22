# Git Repository 구성

<br>

- Upstream Remote Repository(이하 Upstream Repository) : 개발자들이 공유하는 저장소로 최신 소스코드가 저장되어 있는 원격 저장소

- Origin Remote Repository(이하 Origin Repository) : Upstream Repository를 Fork한 원격 개인 저장소

- Local Repository : 내 컴퓨터에 저장되어 있는 개인 저장소

 <br>

![image](https://user-images.githubusercontent.com/92145785/226789841-09f0d825-5c73-423f-8a5e-77204df432bd.png)

1. Local Repository에서 작업을 완료한 후 작업 브랜치을 Origin Repository에 push

2. Github에서 Origin Repository에 push한 브랜치를 Upstream Repository로 merge하는 Pull Request를 생성하고 코드리뷰를 거친 후 merge

3. 다시 새로운 작업을 할 때 Local Repository에서 Upstream Repository를 pull

<br>

# Git-flow 전략

<br>

#### Git-flow에는 5가지 종류의 브랜치가 존재하는데 항상 유지되는 메인 브랜치 master, devlop 와 일정 기간 동안만 유지되는 보조 브랜치 feature, release, hotfix 가 있다.

- master : 제품으로 출시될 수 있는 브랜치
- develop : 다음 출시 버전을 개발하는 브랜치
- feature : 기능을 개발하는 브랜치
- release : 이번 출시 버전을 준비하는 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정 하는 브랜치

<br>

![image](https://user-images.githubusercontent.com/92145785/226789996-69f91abd-3fef-41aa-8e4f-4c6deb302339.png)

1. master와 develop 브랜치 존재

2. develop 브랜치에서는 상시 버그를 수정한 커밋들이 추가

3. 새로운 기능 추가 작업이 있는 경우 develop 브랜치에서 feature 브랜치를 생성 (feature 브랜치는 언제나 develop 브랜치에서부터 시작)

4. 기능 추가 작업이 완료 시 feature 브랜치는 develop 브랜치로 merge , develop에 이번 버전에 포함되는 모든 기능이 merge 되면 QA를 하기 위해 develop 브랜치에서부터 release 브랜치를 생성

5. QA를 진행하면서 발생한 버그들은 release 브랜치에 수정. QA를 무사히 통과했다면 release 브랜치를 master와 develop 브랜치로 merge 합니다. 마지막으로 출시된 master 브랜치에서 버전 태그를 추가
