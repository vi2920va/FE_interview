# CSS - em-rem

## 설명

### 1. em

em 단위는 상위 요소 크기의 몇 배인지로 크기를 정합니다.

#### 1-1. 예제

body의 font-size는 html font-size의 1.5배인 24px이고  
a의 font-size는 body font-size의 2배인 48px이 됩니다.

```html
html { font-size: 16px; body { font-size: 1.5em // 24px a{ font-size: 2.0em // 48px } } }
```

### 2. rem

rem 단위는 문서의 최상위 요소, 즉 html 요소의 크기의 몇 배인지로 크기를 정합니다.

#### 2-1.예제

a의 font-size는 최상위 요소인 html font-size의 2배인 32px이 됩니다.

```html
html { font-size: 16px; body { font-size: 1.5em a { font-size: 2.0rem // 32px } } }
```

## 요약

- em은 상위 요소 기준으로 크기가 결정되는 단위 입니다.
- rem은 최상위 요소 기준으로 크기가 결정되는 단위 입니다.
