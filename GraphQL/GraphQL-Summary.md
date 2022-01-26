# GraphQL - Summary

## 설명

### GraphQL

GraphQL이란 Graph Query Language 라는 뜻으로 페이스북에서 2012년 만들어진 API를 위한 쿼리 언어 입니다.
`REST-ful Routing` 이라는 개념을 살펴보면 `REST-ful Routing` 이란 URL과 HTTP 요청 메소드를 이용해서 서버에서 제공뙤는 데이터 모음들을 사용하게 하는 데이터 모음집 입니다.
쉽게 설명하자면 `REST-ful API`와 관련된 정보들을 모아놓은 모음집이라고 이해하면 편할 것 같습니다. 하지만 일정한 규칙이 정해진 HTTP 요청 메소드들 (REST-ful API → 리소스가 표현되는 규칙을 )들의 경우 여러 데이터들이 중첩되거나 관련성이 높은데이터들에 대한 요청을 보내려고 할때 만들어지는 URL을 생각해보면 매우 복잡해집니다.
하지만 GraphQL은 반환되는 데이터 구조와 거의 흡사한 구조의 쿼리형태를 보내면서 `GET`으로 취하게되는 URL 명세의 한계를 뛰어넘게 됩니다. 클라이언트에서 받고 싶은 데이터들을 쿼리로 받아버리면 그만이라 따로 `GET` 관련 명세는 백엔드에서 따로 만들지 않아도 되어버립니다.

---

### 스칼라

스칼라 타입은 그래프 큐엘의 끝단을 정의하고 그래프 큐엘의 시작은 쿼리로 시작합니다. 뮤테이션은 쿼리와 같이 시작가능한데 POST 에 해당하는 서버의 데이터 수정을 요구하는 요청을 보낼때 사용합니다.
스칼라 타입은 4가지의 타입이 있습니다.

- Int
- Float
- String
- Boolean

### 인터페이스

- GraphQh
  ```graphql
  interface Character {
    id: ID!
    name: String!
    friends: [Character]
    apeearsIn: [Episode]!
  }
  ```
  Character를 상속하는 모든 타입은 이러한 인자와 리턴타입을 가지는 정확인 필드를 가져야 합니다.
  ```graphql
  type Human implements Character {
    id: ID!
    name: String!
    friends: [Character]
    apeearsIn: [Episode]!
    starships: [Starship]
    totalCredits: Int
  }
  ```
  위 코드를 보다시피 인터페이스의 모든 속성을 그대로 복붙해서 정의하고 추가적인 속성을 추가 정의 하는 모습을 볼 수 있습니다.
        ```graphql
        query HeroForEpisode($ep: Episode!) {
        	hero(episode: $ep) {
        		name
        		primaryFunction
        	}
        }
        ```


### 프래그 먼트

```graphql
fragment Doodream on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

쿼리나 뮤테이션에 선언된 변수는 프래그먼트에서도 정의해서 프래그먼트가 쓰일때 인수로 정의될수 있습니다.

프래그먼트는 같은 데이터구조가 반복될때 `...` 문법을 이용해서 반복되는 데이터구조를 대신 할 수 있습니다.

### 인라인 프래그먼트

인터페이스나 유니언 타입을 반환하는 필드를 쿼리하는 경우 인라인 프래그 먼트를 사용해야합니다.

```graphql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}
```

`... on` 이러한 문법은 hero 에서 반환된 Charactor가 Droid 타입인 경우에만 실행됩니다. Human 타입인 경우에도 마찬가지 입니다.

### 작업이름

작업타입은 쿼리, 뮤테이션, 구독(subscribe)이 될수 있습니다.

- subscribe는 웹소켓을 사용하여 실시간 양방향 통신을 구현할 떄 사용합니다.

작업 이름은 명시적인 작업의 이름으로서 디버깅이나 서버측에서 로깅할때 쿼리이름(작업이름)을ㅈ거어놓으면 내용을 훨씬 명시적으로 확인 가능합니다.

### 변수

클라이언트측 코드는 쿼리 문자열을 런타임에 동적으로 조작합니다. 즉, 쿼리 문자열이 런타임에 동적으로 바뀔 수 있다는 상황입니다.

```graphql
❗️이런상황에서 변할 수 있는 동적인자를 쿼리 문자열에 직접 전달하는 것은 변할 수 있는 쿼림 문자열의 상황에서 좋지는 않은 상황입니다.  - 뭔소리지?
```

GraphQL은 동적 값을 쿼리에서 없애고, 이를 별도로 전달하는 방법을 제공합니다. 쿼리의 어떤인자가 동적인지 나타내는 좋은 방법이 됩니다.

```graphql
query HeroNameAndFriends($episode: Episode = "JEDI") {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### 변수 정의

위 의 코드에서 `$episode` 변수의 타입은 `Episode` 가 됩니다. 일반적인 타입스크립트에서 정의하는 방식과 동일합니다.

선언된 모든 변수는 스칼라, 열거형, input object type 셋중에 하나여야 합니다. 복잡한 객체를 필드에 전달하려면 서버에서 일치하는 입력 타입을 알아야합니다.

또한 기본값으로 `=` 문법을 사용하여 지정할 수도 있습니다.

❗️ input object type이 뭘까? - 인자로 전달될 수 있는 특별한 종류의 객체타입! →스칼라 타입이 아니다.

### 뮤테이션

query는 서버측의 데이터를 일방적으로 GET 하는 것에 초점을 맞춘 것이였다면

- ❓ query를 생략가능한 경우는 뭘까

뮤테이션은 서버측 데이터를 수정하는 작업입니다.

REST에서는 데이터를 수정하는 작업을 POST 라고 명시적으로 표현을 하는 것처럼 GraphQL 도 이러한 작업이름을 뮤테이션이라고 명시함으로서 구분할 수 있습니다.

```graphql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
```

- 쿼리필드는 병렬로 실행되지만 뮤테이션 필드는 하니씩 차례대로 실행됩니다.

Tip! : graphQL은 연습할 때 playground 에서 해도되지만 로컬에서 쿼리를 계속 날려보는것이 도움읻 된다.

### 메타 필드

GraphQL 서비스에서 리턴될 타입을 모르는 상황이 발생하면

클라이언트에서 해당 데이터를 결정할 방법이 필요 →

GraphQL을 사용하면 쿼리의 어느 지점에서나 메타필드 `__typename`

을 요청 : 그 시점에서 객체타입의 이름을 얻을 수 있다.

### 입력타입

```graphql
input ReviewInput {
  starts: Int!
  commentary: String
}
```

```graphql
mutation CreateReviewForEpisode ($ep : Episode!, $review: ReviewInput!){
	createReview(episode : $ep, review: $review0{
		stars,
		commnentary
	}
}
```

보통 뮤테이션에서 많이 사용합니다. 스키마 언어에서 입력타입은 일반 객체타입과 정확하게 같습니다. 하지만 type 대신 input을 사용합니다.

타입 시스템을 사용하면 GraphQL 쿼리가 유효한지 여부를 미리 알수 있다. 이를 통해서 런타임 검사에 의존하지 않고도 유효하지않은 쿼리가 생성되었을때 서버와 클라이언트가 효과적으로 개발자에게 알릴 수있다.

```graphql
{
  hero {
    favoriteSpaceship
  }
}
```

### GraphQL 의 검증

타입 시스템을 사용하면 GraphQL 쿼리가 유효한지 여부를 미리 알수 있습니다. 이를 통해 런타임 검사에 의존하지 않고도 유효하지 않은 쿼리가 생성되었을 때 서버와 클라이언트가 효과적으로 개발자에게 알릴 수 있습니다.

→ 즉, 개발자 도구안에서 타입스크립트같이 유효성 검사를 실시간으로 지원하는 검증과정을 지원합니다.

### GraphQL 실행

유효성검사를 실행후 GraphQL의 쿼리는 GraphQL 서버에서 실행됩니다. 반환값은 JSON 형태로 반환됩니다.

쿼리가 실행되는 과정을 살펴봅시다 .

스키마

```graphql
type Query {
  human(id: ID!): Human
}
type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
type Starship {
  name: String
}
```

쿼리

```graphql
{
  human(id: 1002) {
    name
    appearsIn
    starships {
      name
    }
  }
}
```

타입의 각 필드는 GraphQL 서버개발자가 만든 resolver 함수에 의해 실행됩니다. 필드가 실행되면 해당 resolver( 서버개발자가 만든 )가 호출되어 다음 값을 생성합니다.

그리고 끝단은 언제나 스칼라 값으로 끝나게 되므로 필드가 스칼라 값을 반환하면 실행이 완료됩니다.

따라서 GraphQL 쿼리의 끝은 언제나 스칼라 값 입니다.

### 스키마 확인

GraphQL은 스키마를 통해서 어떤쿼리를 지원하는지에 대한 정보를 알 수 있습니다. → `introspection`

Query의 루트 타입에서 항상 사용할 수 잇는 `__schema` 필드를 쿼리하여 GraphQL에 요청할 수 있습니다. 사용 가능한 타입을 요청해봅시다.

스키마확인 쿼리

```graphql
{
  __schema {
    queryType {
      name
    }
  }
}
```

반환값

```json
{
  "data": {
    "__schema": {
      "queryType": {
        "name": "Query"
      }
    }
  }
}
```

`ofType`

- 스키마 확인

```graphql
__type(name: "Droid"){
	name
	descrption
}
```

쿼리변수는 variables 라는 추가 쿼리파라미터에서 JSON 인코딩 문자열로 보낼수 있다.

### 인증

GraphQL에서 보통 서버의 resolver 단에서 단일 인증을 통해 진행합니다.

### 캐싱

객체 식별자를 제공하면 클라이언트가 풍부한 캐시를 구축 할 수 있습니다. → API들의 URL이 클라이언트가 캐시를 구성하는데 사용할 수 있는 전역 고유 식별자 입니다. → 전역고유 ID를 사용해서 캐시기능을 사용할 수 있습니다.

### 전역 고유 ID (객체 식별자)

하지만, GraphQL에서는 고유 식별자를 제공하는 URL같은 원시값이 없기 때문에, 클라이언트가 API를 사용할 수 있도록 이러한 식별자를 노출하는 것이 좋습니다. 예) id

### 유용한 도구

[playground](https://www.graphqlbin.com/v2/6RQ6TM)

- 백엔드 URL 엔드포인트를 연결하면 쿼리를 작성하는 것에 따른 결과값을 보여주는 웹 도구

Altair GraphQL Client

- GraphQL의 POSTMAN 이라고 생각한다. 인증방식을 적용해서 쿼리 요청을 보낼 수도 있고, 기본적으로 playground의 기능들을 모두 가지고 있다고 생각한다.
- 사용가능한 쿼리와 뮤테이션을 자동완성 시켜줄 뿐 만아니라 명세까지는 아니지만 데이터 타입들의 정렬된 모습을 보여줌으로서 어떤 데이터 타입들이 있는지 전체적으로 볼수 있었다. (데이터베이스 테이블의 명세까지는 아니다.)

## 요약

- GraphQL은 기존의 REST API를 대체할 수 있는 쿼리언어 입니다.
- 쿼리의 끝단은 스칼라 데이터 타입이며 값이 스칼라 타입이 나오면 조회를 그만두고 반환하는 형태입니다.
- GET과 POST형태로 통신을 하는데 보통 작업방식에 따라 query는 GET mutation은 POST 형태의 API를 명시합니다. subscribe는 웹소켓 통신을 이용한 실시간 데이터 통신을 할때 사용합니다.

## 참고

- [공식문서](https://graphql-kr.github.io/)
