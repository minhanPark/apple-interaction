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
