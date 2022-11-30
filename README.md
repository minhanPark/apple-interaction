# apple interaction

## 따라할 기술 및 개념 정리

### lorem200

```
lorem숫자
```

형태로 치면 lorem이 글자수(숫자)에 맞게 써짐

### rem과 em을 혼합해서 쓰는 이유

```css
html {
  font-family: "Noto Sans KR", sans-serif;
  font-size: 14px;
}

.description strong {
  float: left;
  font-size: 3rem;
  color: rgb(29, 29, 31);
  margin-right: 0.2em;
}
```

현재 html에서 font-size는 14px이다.  
그러면 밑에는 그 기준으로 3rem 즉 42px이 된다.  
여기서 em으로 margin-right를 준 이유는 em은 현재 폰트 사이즈를 기준으로 잡기 때문이다. 즉 여기서 42px이 1em이 되고 여기의 0.2인 8.4가 margin-right가 된다. **즉 폰트사이즈의 비율로 너비를 주려고 할 때 이런식으로 혼합해서 사용하면된다.**

### window 이벤트 load vs DomContentLoaded

load는 이미지 등 리소스도 다 다운로드 받은 다음에 실행하고, DomContentLoaded는 돔이 구성되면 실행된다.
그래서 DomContentLoaded가 실행시점은 더 빠르지만 이미지로 인터렉션이 중요하면 load가 좋음

### transform 속성중에 3d가 붙은 애들

3d가 붙은 애들은 하드웨어 가속이 보장이되서 3d가 있다면 3d를 사용하는 것이 좋다.

### 동영상 프레임 추출

구글에서 검색해서 자기한테 맞는거 사용하면 되고, 아이패드 앱에서도 뭔가 있는 듯 하다.  
이미지 압축까지 돌리고 나면 동영상과 비교했을 때 용량이 더 작아짐

### 고화질 비디오 부드럽게 처리하기

이미지를 로드한 후 캔버스에 이미지를 그리면 된다. 애플에서도 이런식으로 캔버스를 활용한다.

### canvas로 이미지 그리기

```js
objs.context.drawImage(objs.videoImages[sequence], 0, 0);
```

크기 맞춰서 위와 같이 그리면 됨.

캔버스 크기를 조정하는 방법은 2가지가 있음. 1개는 캔버스의 width, height를 자바스크립트로 수정하는 것이 있음. 이때는 캔버스가 가진 픽셀수 자체를 바꾸는 것이고, 하나는 css로 해서 scale로 수정하는 것이 있다. 여기서는 스케일로 수정했음(애플도 이렇게 했다고 하고, 성능에 좀 더 도움이 된다고 한다.)

```css
.sticky-elem-canvas {
  top: 0;
  height: 100%;
}

.sticky-elem-canvas canvas {
  position: absolute;
  top: 50%;
  left: 50%;
}
```

```js
const heightRatio = window.innerHeight / 1080;
sceneInfo[0].objs.canvas.style.transform = `translate3d(-50%, -50%, 0) scale(${heightRatio})`;
```

높이 비율을 맞춘다음 캔버스를 채운다.

### getBoundingClientRect

getBoundingClientRect 메소드는 해당 dom의 width, height, left, top, right, botton, x, y를 알려주며 width와 height는 뷰포트의 왼쪽 상단을 기준으로 한다.

### objs.canvas.offsetTop

offsetTop은 getBoundingClientRect처럼 스크롤에 따라서 영향을 받는것이 아니라 고정되어 있음. 그리고 부모 요소로부터 기준을 삼는데, 정확히 사용하려면 부모 요소의 포지션을 relative로 바꿔줘야함

### 부드러운 감속의 원리

acc를 0.1로 잡았을 때
현재지점 = 현재지점 + (목표지점 - 현재지점) \* acc 를 반복한다.  
그리고 requestAnimationFrame을 계속 반복 시킨다.  
목표지점과 현재지점의 차이가 1(바뀔 수 있음) 미만 차이가 나면 애니메이션을 멈춰주면 된다.

### orientationchange

```js
window.addEventListener("orientationchange", setLayout);
```

이벤트 중에 orientationchange는 폰을 가로 세로 전환할 때 발생하는 이벤트다.

### stroke 길이

```css
stroke-dasharray: 100;
```

svg에 stroke-dasharray을 주면 선을 dash 형태로 그릴 수 있다.  
직접 보고 선을 다 그릴려면 몇 인지도 알 수 있고, 자바스크립트로 접근하려면 아래처럼 하면 된다.

```js
document.querySelector("svg").getTotalLength();
```

### 로딩 깔끔하게 처리하기

처음에 body에 before-loading등의 클래스를 주고, overflow값을 hidden처리하면 body가 길어져도 스크롤이 생기지 않는다.

```css
body.before-load {
  overflow: hidden;
}
```

그리고 처음엔 로딩 컴포넌트 등을 보여주고, 'load' 이벤트가 완료된 시점에

```js
document.body.classList.remove("before-load");
```

위와 같이 body에서 before-load를 제거해준다.
해당 강의에서 한 방법은 before-loading에서 사라지는 순간에 opacity를 1에서 0으로 변경시켜주면서 transition을 통해서 부드럽게 처리해준다.

```js
document.querySelector(".loading").addEventListener("transitionend", (e) => {
  document.body.removeChild(e.currentTarget);
});
```

그 다음에 transitionend이 끝난 다음에 해당 컴포넌트를 삭제시켜주면 부드럽게 사라지는 듯한 느낌을 볼 수 있다.

### apple 사이트에서도 사용한 will-change

css에 will-change는 엘리먼트에 어떠한 변경을 할 것인지를 미리 브라우저에 알려주는 것이다. 이것을 사용하면 그 변경이 시작되기 전에 적절히 최적화할 수 있다. 즉, 페이지 출력에 악영향을 줄 수 있는 처리 비용을 줄일 수 있다는 것이다. 그로인해 효율적으로 엘리먼트의 변경 또는 렌더링을 처리할 수 있고 페이지는 순식간에 갱신돼 부드러운 화면처리가 가능하게 된다.

```css
#show-scene-0 #scroll-section-0 .sticky-elem,
#show-scene-1 #scroll-section-1 .sticky-elem,
#show-scene-2 #scroll-section-2 .sticky-elem,
#show-scene-3 #scroll-section-3 .sticky-elem {
  display: block;
  will-change: transform, opacity;
}
```

강의에서는 마지막에 위와 같이 사용함.

주의할점

- 너무 많은 요소에 will-change를 적용말라. 과도한 사용은 페이지 속도를 늦추거나 엄청난 자원을 소비한다.
- 스크립트 코드를 사용하여 변화발생의 전후로 will-change의 활성/비활성을 바꿔주는 것이 좋다.
- 만약 페이지가 잘 돌아간다면 단지 조금 더 빨리하기 위해 will-change를 사용말라. will-change의 초과사용은 브라우저가 가능한 변화에 대한 준비를 시도하기 때문에 과도한 메모리 사용과 더 복잡한 렌더링을 초래한다.
- 작업할 충분한 시간을 줘라. 변화가 발생하기 조금 전에라도 그 변화를 예상할 방법을 찾아 will-change를 설정하라.

### 스크립트 내부의 중단점 설정

```js
function hello(name) {
  let phrase = `Hello, ${name}!`;

  debugger; // <-- 여기서 실행이 멈춥니다.

  say(phrase);
}
```

위와 같이 중단점을 설정할 수 있음
