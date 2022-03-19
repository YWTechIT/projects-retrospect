## ✏️ preview

deploy: https://elice-kdt-sw-1st-team1.elicecoding.com

README: https://github.com/YWTechIT/GitFarm

---
### 글을 시작하며..
누군가가 나에게 지금까지 했던 프로젝트 중에서 시간과 노력을 가장 많이 쏟은 프로젝트가 뭐냐라고 물어본다면 `GitFarm` 프로젝트를 꼽을것이다. 팀 프로젝트에 첫 팀장으로서 진행했던 프로젝트이기에 더욱 애착이 갔던 것 같다. 그리고 남에게 잘 보이기 위해 프로젝트를 진행한게 아니라 기능 하나를 추가하면 어떻게 더욱 효율적인 방식으로 개발 할 수 있을까?를 고민하게 해줬던 프로젝트다. 그래서 틈만 나면 유지보수를 시작했다. 글 뒤에 프로젝트가 끝나고 어떤 유지보수를 했는지 설명하겠다.

---
### 어떤 프로젝트인가요?
버전관리를 뜻하는 `Git`과 농장인 `Farm`의 합성어로, 농장을 컨셉으로하여 `user`들에게 `Git` 잔디 관리를 조금 더 즐겁고 꾸준하게 할 수 있도록 장려하고 도와주는 서비스이다.

---
### 왜 만들었나요?
프로젝트 기획 당시 공통적인 팀원들의 의견은 일반적으로 서비스를 소비하는 소비자들에게 초점을 맞추는것보다 그러한 서비스를 만드는 개발자에게 도움을 주는 서비스를 만들자는 의견이 나왔다. 본업을 개발자로 삼으려면 트렌드에 뒤쳐지지 않게 꾸준하게 공부하는 습관이 중요한데 기록으로 남기기위해 1일 1커밋을 주로 이용한다. 그런데 자신만의 의지로 1일 1커밋을 하는것보다 꾸준하게 커밋 할 수 있도록 옆에서 도와줄 수 있도록 인프라를 만들면 개발자가 즐겁게 공부하며 커밋 습관을 유지하고 도와주어 결론적으로 장기간 목표를 달성할 수 있지 않을까라는 생각에 이 프로젝트를 기획하게 되었다.

---
### 내가 담당한 역할과 기능은..?
처음 배정받은 역할은 프론트인데 결론적으로 팀장 + 백엔드를 주로 담당하고 프론트엔드도 같이 담당했다.(왜 역할이 바뀌었는지 궁금하다면 <a href='https://ywtechit.tistory.com/427'>다음 글</a>을 참고해주세요.)  

2차 팀 프로젝트에서 구현하고 싶었던 기능을 팀원의 동의하에 구현하게 되었고, 내가 구현한 기능은 다음과 같다.

1. webpack, babel, Eslint, Prettier, jest boilerplate 작성
2. ContextAPI로 Login 전역 상태 관리 구현
3. OAuth 및 JWT 인증 구현
4. GitHub API 가공
5. mongoDB 연동
6. Unit, API 테스트 코드 작성
7. Nginx 연동(SSL) 및 Azure + PM2 배포
8. Docker, Docker-compose 사용
9. PWA 적용(manifest, service-worker)

`React`로 구현 할 때 왜 `CRA` 대신 `webpack`을 사용했냐면 공부목적에서 `webpack`을 사용해보고 싶었다. `CRA`가 물론 번들링까지 해줘서 편하지만 어떤 과정을 통해 번들링을 해주는지 중간다리를 직접 `setting`하며 눈으로 확인해보고 싶었다. `webpack`을 사용하고나니까 `ESbuild`와 `Vite`에도 자연스레 관심이 가게 되었다. 다음 프로젝트는 둘 중 하나를 사용해서 빌드해보고 싶다. 여담으로 처음에 `webpack` 대신 `ESbuid`로 진행했으나, `jest`사용시 잦은 충돌과 오류로 인해 `webpack`을 사용했다.

---
### 프로젝트를 진행하며 어려운 점과 어떻게 해결했는지..?
프로젝트를 진행하며 제일 어려운 점은 `GitHub API` 호출이다. 프로젝트 기획 당시 `최근 3개년`의 커밋 개수, `Date Controller`를 이용해 달(month)을 넘기면 해당 월의 일일 총 커밋 개수를 호출하고 싶었는데, `GitHub API Free version` `Limit`이 1시간에 5,000회로 제한되어있었다. 요금을 내고 `Enterprise`와 같은 유료버전으로 전환하면 시간당 약 `15,000`회 요청까지 사용가능하지만 나는 한푼이 부족한 백수(💯 💧)기 때문에 과감하게 유료버전은 `pass`하고 무료버전에서 타협을 보기로 했다. 그래서 최근 3개년 커밋 API 요청과 특정 월의 일일 총 커밋 개수 기능을 없애고 최근 3개월 총 커밋 개수와 이번 달 일일 총 커밋개수 기능을 추가했다. 이렇게하니까 API 호출이 필요한 모든 기능에 한번 요청이 기존 약 `5 ~ 6,000`번에서 `2 ~ 3,000`번으로 줄어들었다. 그리고 각 페이지에 접속 할 때마다 `GitHub API`를 호출하여 데이터를 가져왔는데, 무거운 기능이 아닌 작은 기능을 호출하는데도 불구하고 데이터 로딩 시간이 `2~3초`정도 걸렸다. 내가 직접 테스트해보니 사용자 경험이 매우 떨어져서 `localStorage`에 기존의 데이터를 저장해놓고 페이지에 접속하면 새로운 데이터를 불러오기전까지 `localStorage` 데이터를 사용하는 방법과 각 페이지에 접속할 때마다 `DB`에 있는 값을 불러오는 방법을 생각했는데, 후자를 택하기로 했다. 결론적으로 데이터 로딩시간이 `0.5`초 미만으로 사용자 경험성이 올라갔다. 그리고 로그인을 하고나면 `/loading` 페이지로 이동하여 `GitHub API`를 호출하는 기능을 작성했는데, 한번 `/loading`페이지에 접속하고나서 1시간 이내로 들어오면 `API Limit`으로 인해 정상적으로 데이터를 가져오지 못했다. 그래서 `timeLimit`이라는 미들웨어를 만들어서 `1시간`이내로 재접속하면 `GitHub API`를 호출하지 않고 바로 `/main`으로 `redirect`하게 설정했다. `1시간`이내를 판별하는 방법은 `mongoDB`의 `updatedAt` `key`를 이용했다. 아래는 직접 만든 `timeLimit` 미들웨어의 코드이다.

```javascript
// Backend/middleware/time-limit.js
import { TARGET_TIME, ONE_MILLISECOND, ONE_MINUTE } from "../utils/date.js";
import { Commit } from "../model/index.js";
import { getUpdatedAtById } from "../utils/db.js";

export default async (req, res, next) => {
  const { user } = req;

  try {
    const updatedAt = await getUpdatedAtById(user, Commit);

    // commit 정보가 없을 때
    if (!updatedAt) {
      return next();
    }

    const calc = Math.floor(
      (new Date() - updatedAt) / (ONE_MILLISECOND * ONE_MINUTE),
    );

    if (calc >= TARGET_TIME) {
      return next();
    }
    return res.redirect("/main");
  } catch (err) {
    next(err);
  }
};
```

---
### 프로젝트의 아쉬운 점과 어떤 점을 보완했는지? 
엘리스 SW 엔지니어 트랙 수료식(<a href='https://ywtechit.tistory.com/444?category=973808'>이전 글 참고</a>)을 하면서 공식적인 2차 팀 프로젝트가 끝이나고 각 팀들이 만든 프로젝트를 심사위원분들께서 평가하는 `데모데이(demo-day)`까지 약 3주의 시간이 남았다. 사실 유지보수는 해도 그만 안해도 그만이지만 프로젝트 팀장으로서 열과 성을 다해 만든 프로젝트를 이대로 끝내긴 아쉽다는 생각이 들었다. 그래서 수료식 이후에 팀원들과 미팅을 잡고 깃팜 프로젝트 유지보수 참여 여부를 조사했고, 대부분의 팀원들이 유지보수에 참여하겠다는 의견이었다. (지금 생각해보니 유지보수는 의무가 아니기에 참여하지 않아도 될 일이었지만, 프로젝트를 위해 참여해주셔서 감사하다는 생각을 한다.) 각자 유지보수하고 싶은 기능들을 뽑아보니 15개정도로 추려졌고 여기서 내가 하고 싶은 유지보수는 다음과 같았다.

```
1. 서버 무중단 배포
2. Docker 및 Docker-compose 적용
3. Batch 적용
4. 전역상태관리 적용
5. PWA 적용
```

그런데, 5가지의 목록 모두 내가 해봤던 기능들이 아니기에 이걸 3주내에 끝낼 수 있을까?라는 걱정이 먼저 앞섰다. 그리고 본격적인 유지보수에 들어가기에 앞서 왜 이러한 기능들을 구현하고 싶은 걸까?라는 부분에 집중했다. 그렇게 한참을 고민하고나서 다음과 같은 결론에 이르렀다.

```
1. 서버 무중단 배포: 배포환경 terminal(개인 맥북)을 항상 켜둘 수는 없었고 만약, 내가 맥북을 재부팅 할 때마다 배포환경에 들어가서 배포 서버를 켜줘야하는 번거로움이 발생한다. `PM2`를 통해 해결했다.
2. Docker 및 Docker-compose 적용: 배포서버에 수정사항이 생기게 되면 서버를 내렸다가 다시 올려야하는데, 매번 동일한 작업(pm2 적용, Nginx 코드 수정, SSL 인증서 적용 등)을 하기엔 번거롭고 작업 효율이 매우 떨어질 것이라 생각했다. `docker`와 `docker-compose`를 사용하여 해결했다.
3. Batch 적용: 프로젝트의 기능 중 `3개월에 한번씩 data를 초기화하는 기능`이 있는데, 이를 위해 매번 3개월마다 한번씩 서버에 설정을 하게되면 매우 번거로웠다. `node-schedule` 패키지로 해결했다.
4. 전역상태관리 적용: 로그인을 한 유저와 로그인을 하지 않은 유저를 구별해서 로그인 하지 않은 유저가 기능을 이용하려 시도하면 강제로 로그인 페이지로 `redirect`하는 기능을 적용해야하는데, 전역상태를 사용하지 않으면 매 `page`마다 동일한 코드를 작성하게 되어 불필요하게 코드를 늘리게 되는 문제가 발생한다. `contextAPI`로 해결했다.
5. PWA 적용: 오프라인으로 접속했을 때 미리 다운로드 되어있는 데이터를 보여주고, 아이콘을 설치하여 APP처럼 바로가기를 통해 프로젝트를 제공하고 싶었고, 주소표시창이 없어져서 더 깔끔한 UI를 제공 할 수 있다. `manifest`와 `service-worker`로 해결했다.
```

무작정 최신기술이라서 적용해야겠다라는 생각보다는 왜? 사용해야하는지 이유와 함께 작성해보니 한결 마음이 편해졌다. 그리고 관련 서적과 관련 영상, 관련 코드를 내 프로젝트에 맞게 사용할 수 있을 때까지 찾아봤다. Docker 관련서적으로는 <a href='https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=273111259'>컨테이너 인프라 환경 구축을 위한 쿠버네티스/도커 </a>를 구매하여 읽어봤는데 `Docker`를 다루는 파트가 생각보다 적어서 많은 도움이 되지 않았다. `k8s`, `jenkins`를 공부하고 싶다면 이 서적을 추천한다. 맨 땅에 헤딩하며 삽질한지 2주결과 모든 기능을 적용 할 수 있었다.

---
### 마치며..
이번 프로젝트에서 얻은 경험은 앞으로 내가 현업에 뛰어들 때 도움이 될 것이란 생각이 들었다. 그 중에서 제일 기억에 남는 점은 이전에 배우지 않은 기술이라고해서 무작정 겁먹지 말자는 것이다. 내가 했던 고민은 이전에 어떤 개발자가 고민했었고, 내가 배우고 싶은 기술은 이전에 어떤 개발자가 나처럼 모르는 개발자를 가르쳐주기 위해 강의영상을 올려놓았기 때문이다. 모르면 여러번 관련 서적을 찾아보고, 관련 영상을 찾아보고, 관련 코드를 찾아보면 답을 찾을 수 있다. 아무것도 모른채로 해결하겠다는 의지 하나뿐인 나도 구현했으니 말이다. 어리석은 자는 실패하는사람이 아니라 실패를 두려워하는 사람이라는 말이 생각났다. 끝으로 나는 인터스텔라 영화를 좋아하는데 당시 영화를 보며 감명깊었던 명언을 쓰며 글을 마치겠다.

> We will find a way. We always have.

> 우리는 답을 찾을 것이다. 늘 그랬듯이

![](https://images.velog.io/images/abcd8637/post/68cf9cc7-45ef-4c33-9094-bacbf5b218b9/movie_image.jpeg)

Reference
1. https://docs.github.com/en/rest/overview/resources-in-the-rest-api
2. https://pm2.keymetrics.io
3. https://www.docker.com
4. https://docs.docker.com/compose
5. https://www.npmjs.com/package/node-schedule
6. https://ko.reactjs.org/docs/context.html
7. https://www.pwabuilder.com
8. https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=273111259
9. https://www.youtube.com/watch?v=gm_L69NHuHM&list=LL&index=33
10. https://www.youtube.com/watch?v=3xDAU5cvi5E&list=LL&index=34
11. https://movie.naver.com/movie/bi/mi/basic.naver?code=45290