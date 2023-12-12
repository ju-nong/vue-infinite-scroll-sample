# Vue에서 무한 스크롤 구현해보기

기존에 사용하던 vue-infinite-loading 라이브러리가 맘에 안 들어서, 직접 `IntersectionObserver`로 구현

<br/>

1. VueUse에 useElementVisibility

-   target으로 지정한 엘리먼트의 가시성을 반환 해주는 함수
-   이걸로 무한 스크롤 구현하다가, 게시글 업데이트 됐는데 아직도 보여지고 있으면 다시 게시글 업데이트를 치려고 했다
-   근데 해당 함수에서 반환해주는 ref 변수가 게시글 업데이트하는 함수에 nextTick을 걸고 조회해도 한발짝 늦게 값이 변경되는 이슈가 있다.
-   라이브러리 코드를 까보니 IntersectionObserver 기반임

    <br/><br/>

2. VueUse에 useInfiniteScroll

-   사실 사용해보진 않았지만 코드 까보니 위에 useElementVisibility 기반
-   바로 패스

<br/><br/>

3. IntersectionObserver 사용

-   대부분 무한 스크롤 라이브러리에서 쓰이는 방식이 이거임
-   매우 잘 됨, 커스텀 자유분방
-   위에 함수들은 `unobserver()` 같은 기능을 제공 안해서 패스 됐음
-   해당 엘리먼트가 보였을 때 바로 `unobserver()` 메소드로 가시성 변화 주시를 중단 시키고, API가 끝나고 다시 `observer()` 메소드를 실행하면 해결
