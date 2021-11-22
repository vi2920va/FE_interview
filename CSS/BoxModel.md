# CSS - BoxModel

## 설명
모든 HTML 요소들은 기본적으로 Box Model을 가지고 있습니다.   
Box Model은 margin, border, padding, content로 이루어져 있습니다.   
기본적으로 Box Model은 content 넓이를 요소의 width, height 값으로 설정합니다.   

- content : 텍스트나 이미지 등의 콘텐츠가 들어가있는 박스의 영역
- padding : content와 border사이의 간격
- border : content와 padding을 감싸는 테두리
- margin : border와 다른 요소사이의 간격

### box-sizing
- box-sizing : content-box는 기본 박스모델의 넓이값으로 계산
- box-sizing : border-box는 content,padding,border까지 요소의 width, height값으로 설정합니다.

## 요약
- Box Model은 모든 HTML요소가 가지고있는 박스이다.
- margin, border, padding, content 순으로 이루어져있다.
- Box Model의 width, height 값은 content 영역의 넓이 값으로 설정된다.
- box-sizing : border-box로 width, height 값을 content, padding, border까지의 넓이로 설정할 수 있다.