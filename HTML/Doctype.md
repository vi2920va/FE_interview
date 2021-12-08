# HTML - Doctype

## 설명

- `<DOCTYPE>`은 HTML 문서에서 최상단에 `<!DOCTYPE html[버전 정보>` 형태로 선언됩니다.

### 1. HTML의 종류에 따른 DOCTYPE

HTML5

```html
<!DOCTYPE HTML>
```

HTML 4.01

```html
<!-- strict -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
   "http://www.w3.org/TR/html4/strict.dtd">

<!-- transitional --> 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
   "http://www.w3.org/TR/html4/loose.dtd">
  
<!-- transitional --> 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"
   "http://www.w3.org/TR/html4/frameset.dtd">
```

### 2. HTML5 특징

- HTML5의 경우 별도의 버전 표기 없이 `<!DOCTYPE html>` 의 형태로 선언할 수 있습니다.

- HTML5는 기존 HTML과 **호환성**을 보장하기 때문에 기존의 소스를 변경하지 않아도 됩니다.
- 신규 기능들의 도입으로 느슨훈 문법으로 인하여 **실용적 설계**가 가능합니다.
- 표현적인 요소와 내용을 완벽하게 분리했습니다.

## 요약

Doctype은 어떤 버전의 HTML 문서에서 최상단에 선언되며, 어떠한 HTML을 사용할 것인지 브라우저에게 알려주는 역할을 합니다.

## 참고

- [HTML5 강좌](https://theqoop.tistory.com/265)
