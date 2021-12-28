## ✏️ preview

deploy: https://elice-kdt-sw-1st-vm02.koreacentral.cloudapp.azure.com/
README: https://kdt-gitlab.elice.io/sw-001-project/team2/have-u-tried-this

### 어떤 프로젝트인가요..?
항상 혼자 프로젝트를 만들다가 처음으로 팀 프로젝트를 진행했다. 프로젝트 명은  `have-u-tried-this`인데 국내 지역별로 맛집을 공유할 수 있는 프로젝트이다. 다른 사이트와의 차이점이 있다면 식당별로 정리되어있지 않고 자치구까지 찾아볼 수 있는 점과 단순히 식당이 어떤지 리뷰하는것이 아니고 소셜 기능을 통해 소통할 수 있는 공간을 마련했다. 

---
### 내가 담당한 기능은..?
![](https://images.velog.io/images/abcd8637/post/0b3c2b3f-1983-4df8-92ab-bf9564607644/Dec-28-2021%2007-37-06.gif)

이미지 업로드와 드래그 앤 드롭기능을 담당했다. 추가로 단순히 이미지만 올리고 끝나는 것이 아니라 사진에 저장되어있는 `EXIF` 데이터를 읽고 해당 사진에 `geolocation` 정보가 들어있다면 위도와 경도의 좌표로 행정구역정보를 받을 수 있는 <a href='https://developers.kakao.com/tool/rest-api/open/get/v2-local-geo-coord2regioncode.%7Bformat%7D'>카카오API</a>와 연동하여 자동으로 사진 촬영위치에 `value`를 채워주는 기능이었다. `EXIF(EXchangealbe Image File format)`는 교환 이미지 파일 형식인데, `JPEG`, `TIFF 6.0`, `RIFF`, `WAV` 파일 포맷에서 이용되며, 사진에 대한 정보를 포함하는 메타데이터를 의미한다. 일본 전자산업진흥협회에서 개발되었다고 한다. (자세한 내용은 하단 reference 참조)

처음 더미이미지로 테스트했을 때 `EXIF` 데이터가 정상적으로 추출되지 않아서 코드를 잘못 작성한 줄 알았는데, 카카오톡이나 디스코드와 같이 `SNS`를 한번 거쳐간 사진은 개인정보 보호를 위해 `EXIF` 데이터를 삭제한다고 한다. 그래서 `google cloud`에 이미지를 올리고 `PC`로 다운로드를 받은 다음 진행했다. `MAC`기준으로 이미지파일 우측 버튼 클릭 후 `정보 가져오기`를 클릭했을 때 `추가 정보`에 다음과 같은 내용이 포함되어있다면 `EXIF` 데이터가 정상적으로 들어가있는 것이다. 

![](https://images.velog.io/images/abcd8637/post/0adec164-648f-4120-aad6-0631ccd51bb6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-28%2007.44.40.png)

`EXIF`에서 `geolocation` 값을 추출했다면 해당 값을 그대로 카카오 API에 넣으면 되는것이 아니라 좌표를 십진수로 변경해야한다. 우리가 알고있는 좌표 위도: `33° 27' 42.258" 북`, 경도: `126° 18' 42.672" 동`와 같은 형태는 도진수이고 위도: `33.46173889`, 경도: `126.31185278`와 같은 형태는 십진수이다. 좌표 변환하는 방법은 하단 reference 참조하거나 다음 포스팅을 참고하자.

프로젝트 발표를 보면서 다른 팀들은 대부분 구현하지 않았는데 `drag&drop`을 구현했다. 구현은 `MDN`을 참고했다. (하단 reference 참조) 그리고 파일 업로드 유효성을 통해 `20MB` 이하의 사진만 업로드 할 수 있고 파일 확장자는 `apng`, `bmp`, `jpeg`, `pjpeg`, `png`, `tiff`, `webp`로 업로드 할 수 있게 한정지었다. 나중에는 파일 업로드시 `Content-type(응답 내에 있는 헤더)` 속성을 프록시로 이용해 우회하여 원하지 않는 파일을 올리는 기법까지 고려하고 싶다.

여담으로 예시로 올린 햄버거 집은 <a href='https://map.naver.com/v5/search/%EC%A0%9C%EC%A3%BC%20%ED%94%BC%EC%A6%88%EB%B2%84%EA%B1%B0/place/1648347549?c=14060504.4835455,3956726.1165910,15,0,0,0,dh'>피즈 애월</a>인데 제주 한달살이 했을 당시 먹어본 햄버거 중에 제일 맛있었다. 랜디스도넛 제주직영점 바로 옆에있는데 랜디스도넛을 먹기위해 웨이팅을 하는데 줄어들 생각을 하지 않는다면 햄버거를 먹으면서 기다리자. 든든하다. 광고아니다. 내돈내산이다.

---
### 프로젝트를 진행하며 어려운 점과 어떻게 해결했는지..?
이번 프로젝트에서 가장 어려운 점은 `DOM`이다. 이전까지 혼자 진행했던 프로젝트는 `React`를 기반으로 진행했지만 지금은 바닐라 자바스크립트를 사용했다. `React`의 특징 중 하나인 `V-DOM`이 `DOM`관리를 수동으로 하나하나 작업하지 않고 자동화하고 추상화해준덕분에 편한 기능만 익숙해진 나머지 `DOM`을 직접 일일이 관리하려니까 어려움이 많았고 처음에는 `template engine`을 통해 `html`코드를 내려주는 방법을 생각했는데 결론적으로 `SPA`로 진행하다보니까 바뀐 `DOM`을 계속 파악하고 있어야 하는 점이 불편했다. 더불어 코드도 방대해졌다.

`DOM`관리는 `innerHTML` -> `createElement` -> `Factory function(hyperscript)` 순으로 점차 발전했다. 

처음 `innerHTML`을 사용했을 때의 장점은 `HTML` 마크업 문자열이기 때문에 간단하게 `DOM`조작이 가능했고 구현이 간단하고 직관적이었다. 그러나 `HTML` 코드에 `JS` 악성코드가 심어져 있다면 파싱과정에서 그대로 실행되기 때문에 위험하다. (`XSS`) 예를들어 `<script>alert(\"hi\")</script>`처럼 `script`태그는 `HTML5`에서 사용못하게 막아뒀다고해도 `<img src='' onerror='alert(\"hi\")'>`와 같은 코드는 `script`가 들어가지도 않았는데 `alert`창이 띄워진다. 또한 기존노드가 존재하고 자식노드를 추가할 때 원래 있던 기존노드까지 새로 만들어 렌더링한다. 즉, 바뀌지 않아도 되는 노드까지 모두 제거하고 처음부터 새롭게 자식 노드를 생성하여 `DOM`에 반영하는데 이는 효율적이지 않다. 마지막으로 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다.

두번째 `createElement`를 사용했을 때의 장점은 `innerHTML`에서 발생할 수 있는 `XSS`를 예방할 수 있고, `DOM`조작을 원한대로 할 수 있다. 그러나 유지보수가 어렵고, 코드가 직관적이지 않아 가독성이 떨어진다.

그래서 첫번째 방법과 두번째 방법의 절충안인 `hyperscript`구조를 채택했다. 이는 나중에 배울 `React`에서 사용하는 `JSX` 문법과 매우 유사하다. (정확하게 `hyperscript`는 `element`에 미리 정의된 `attribute`에 대해 `CSS`선택기를 사용할 수 있는 추가 태그 구문을 지원하지만 `JSX`에서는 그렇지 않다. (reference 5 참조.)) 이를 사용함으로써 순수 `DOM`을 생성하고, 코드의 가독성과 재사용성이 크게 향상됐다. 

```javascript
// we write this
<div id="foo">
	Hello!
	<br />
</div>

// transpile to this
h('div', { id: 'foo' },
	'Hello',
	h('br')
)

// which returns this
{
	nodeName: 'div',
	attributes: { id: 'foo' },
	children: [
		'Hello!',
		{ nodeName: 'br' }
	]
}
```

기존에 사용한 `innerHTML`, `createElement`, `hyperscript`를 사용한 코드를 보자 어떤 코드가 가독성이 제일 좋은가?

```javascript
// innerHTML
const container = `
  <main class="container">
    <form class="image-form" method="post" action="/api/posts">
      <input class="image-form__name" name="name" placeholder="이름을 작성해주세요">
      <button class="image-form__submit">제출</button>
    </form>
  </main>
`

// createElement
const container = document.createElement("section");
container.classList.add("container");
const form = document.createElement("form");
form.classList.add("image-form");
form.method="post";
form.action="/api/posts";
const name = document.createElement("input");
name.classList.add("image-form__name");
const btn = document.createElement("button");
btn.classList.add("image-form__submit");
btn.innerText="제출"

form.append(name, btn);
container.append(form);

// hyperscript
const container = el(
  "section",
  { className: "container" },
  el(
    "form",
    {
      className: "image-form",
      method: "POST",
      action: "/api/posts"
    },
    el(
      "input",
      {
        className: "image-form__name",
        name="name",
        placeholder="이름을 입력해주세요."
      }
    ),
    el(
      "button",
      {
        className: "image-form__submit",
      }
    )
  )
)
```

우리가 참고했던 `hyperscript` 코드에는 `event` 관련 처리가 없었는데 팀장님께서 `event` 관련 처리코드를 작성해주셔서 해결할 수 있었다. 그리고 `class` 대신 `className`을 사용했는데 그 이유는 `JS`에서 `class`가 예약어이기 때문이다.

```javascript
// hyperscript skeleton code
export default function el(nodeName, attributes, ...children) {
  const node =
    nodeName === "fragment"
      ? document.createDocumentFragment()
      : document.createElement(nodeName);

  Object.entries(attributes).forEach(([key, value]) => {
    if (key === "events") {
      Object.entries(value).forEach(([type, args]) => {
        if (Array.isArray(args)) {
          node.addEventListener(type, ...args);
        } else {
          node.addEventListener(type, args);
        }
      });

      return;
    }

    if (key in node) {
      try {
        node[key] = value;
      } catch (err) {
        node.setAttribute(key, value);
      }
    } else {
      node.setAttribute(key, value);
    }
  });

  children.forEach((childNode) => {
    if (!childNode) return;

    if (typeof childNode === "string") {
      if (childNode.includes("\n")) {
        node.innerText = childNode;
      } else {
        node.appendChild(document.createTextNode(childNode));
      }
    } else {
      node.appendChild(childNode);
    }
  });

  return node;
}
```

---
### 다음에 뭘 하고싶은지..?
코드가 점차 구조화되면서 바닐라 자바스크립트가 점점 재밌다는 생각이 들었다. 다음엔 바닐라 자바스크립트에 타입스크립트를 얹어서 프로젝트를 만들어보고 싶고 매번 상태관리를 꼭 사용해야하는것은 아니지만 상태관리를 연습해볼겸 `redux`를 한번 적용해보고 싶다.

---
### 마치며..
이렇게해서 2주라는 짧은 시간에 처음으로 팀 프로젝트를 진행했는데, 왜 개발자가 협업이 중요한지 알것 같다. 프론트엔드에서 보낸 데이터가 제대로 넘어갔는지 확인하기 위해 백엔드와 함께 살펴보기도 하고, 반대로 백엔드에서 보낸 데이터가 프론트로 제대로 넘어왔는지 확인하는 과정, 작은 기능을 수정하더라도 팀원들의 의견을 듣고 더 나은 방향으로 나아가기위한 토의, `commit`하는 과정에서 `conflict`가 났는지 확인하고 앞으로 `branch`를 생성할지 말지 코드 통일은 어떻게 통일할지 등등 작은 일에도 소통이 되어야 올바른 방향으로 나아갈 수 있기 때문이다. 이번 프로젝트를 통해 바닐라 자바스크립트 문법, 폴더 분리, 모듈화, 웹팩, 컴포넌트, SPA등을 배울 수 있었고 팀에게 감사하다는 말을 전하고 싶다.

reference
1. exif(교환 이미지 파일 형식): <a href='https://ko.wikipedia.org/wiki/%EA%B5%90%ED%99%98_%EC%9D%B4%EB%AF%B8%EC%A7%80_%ED%8C%8C%EC%9D%BC_%ED%98%95%EC%8B%9D'>위키백과</a>
2. exif-js: <a href='https://github.com/exif-js/exif-js'>깃허브</a>
3. 도진수좌표를 십진수좌표로 변환<a href='http://justkate.me/Getting-To-Know-Your-Photos/'>getting-to-know-photos</a>
4. drag & drop: <a href='https://developer.mozilla.org/ko/docs/Web/API/HTML_Drag_and_Drop_API'>MDN</a>
5. script elements inserted using innerHTML do not execute when they are inserted: <a href='https://www.w3.org/TR/2008/WD-html5-20080610/dom.html#innerhtml0'>w3.org</a>
6. Preact: Into the void 0: <a href='https://www.youtube.com/watch?v=LY6y3HbDVmg'>JSConf EU 2017</a>
7. How to Write Your Own JavaScript DOM Element Factory: <a href='https://kyleshevlin.com/how-to-write-your-own-javascript-dom-element-factory'>Kyle Shevlin</a>
