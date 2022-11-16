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
