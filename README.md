# 개요

카카오테크 캠퍼스 2기 활동 중 진행한 프로젝트입니다.
- 주제: 카카오톡 선물하기 클론 코딩
- 기간: 2024.07~2024.08
- 인원: 1인

<h1>목차</h1>
<ul>
  <li>6주차: <a href="#spring-gift-point">spring-gift-point</a></li>
  <li>5주차: <a href="#spring-gift-order">spring-gift-order</a> / <a href="https://github.com/jjt4515/spring-gift-order">작업 GitHub</a></li>
  <li>4주차: <a href="#spring-gift-enhancement">spring-gift-enhancement</a> / <a href="https://github.com/jjt4515/spring-gift-enhancement">작업 GitHub</a></li>
  <li>3주차: <a href="#spring-gift-jpa">spring-gift-jpa</a> / <a href="https://github.com/jjt4515/spring-gift-jpa">작업 GitHub</a></li>
  <li>2주차: <a href="#spring-gift-wishlist">spring-gift-wishlist</a> / <a href="https://github.com/jjt4515/spring-gift-wishlist">작업 GitHub</a></li>
  <li>1주차: <a href="#spring-gift-product">spring-gift-product</a> / <a href="https://github.com/jjt4515/spring-gift-product">작업 GitHub</a></li>
</ul>

<details open id="spring-gift-point">
<summary><h1>spring-gift-point</h1></summary>

## step 1

### 기능 요구 사항
작성한 API 문서를 기반으로 팀 내에서 지금까지 만든 API를 검토하고 통일하여 변경 사항을 반영한다.

- 팀 내에서 일관된 기준을 정하여 API 명세를 결정한다.
- 때로는 클라이언트의 편의를 위해 API 명세를 결정하는 것이 좋다.

### 구현 기능 목록
- API 명세 통일

## step 2

### 기능 요구 사항
지금까지 만든 선물하기 서비스를 배포하고 클라이언트와 연동할 수 있어야 한다.

- 지속적인 배포를 위한 배포 스크립트를 작성한다.
- 클라이언트와 API 연동 시 발생하는 보안 문제에 대응한다.
  - 서버와 클라이언트의 Origin이 달라 요청을 처리할 수 없는 경우를 해결한다.
- HTTPS는 필수는 아니지만 팀 내에서 논의하고 필요한 경우 적용한다.

### 구현 기능 목록
- 서버 배포
- 배포를 위한 쉘 스크립트 작성
- CORS 설정

## step 3

### 기능 요구 사항
상품 구매에 사용할 수 있는 포인트 기능을 구현한다.

- 포인트는 사용자별로 보유한다.
- 포인트 차감 방법 등 나머지 기능에 대해서는 팀과 논의하여 정책을 결정하고 구현한다.
- e.g.
  - 5만 원 이상 주문 시 총 금액의 10%가 할인된다.
  - 현금 영수증을 받으려면 휴대전화 번호를 입력해야 한다.
- API 명세는 팀과 협의하여 결정하고 구현한다.
- 관리자 화면에서 포인트를 충전할 수 있다.

### 구현 기능 목록
- 결제 처리 API
  - 5만원 이상 주문 시 10% 할인
  - 현금 영수증 받으려면 휴대전화 번호 입력 필수
  - 결제하면 최종 금액의 10%는 포인트 적립
  - 결제 시 포인트 사용 가능
    
- 주문내역 조회 API
  - 멤버별 주문 내역 조회
    
- 결제 금액 조회 API
  
  - 결제 직전 페이지에서 결제 금액 조회
    
- 포인트 조회 API
  - 멤버별로 포인트 조회 
</details>

<details id="spring-gift-order">
<summary><h1>spring-gift-order</h1></summary>

## step1

### 기능 요구 사항
카카오 로그인을 통해 인가 코드를 받고, 인가 코드를 사용해 토큰을 받은 후 향후 카카오 API 사용을 준비한다.

- 카카오계정 로그인을 통해 인증 코드를 받는다.
- 토큰 받기를 읽고 액세스 토큰을 추출한다.
- 앱  키, 인가 코드가 절대 유출되지 않도록 한다.
  - 특히 시크릿 키는 GitHub나 클라이언트 코드 등 외부에서 볼 수 있는 곳에 추가하지 않는다.
- (선택) 인가 코드를 받는 방법이 불편한 경우 카카오 로그인 화면을 구현한다.

### 구현 기능 목록
- 카카오 로그인 페이지 작성
- 카카오 로그인 콜백 처리
- 카카오 로그인 서비스 코드 작성
- 테스트 코드 작성

## step2

### 기능 요구 사항
카카오톡 메시지 API를 사용하여 주문하기 기능을 구현한다.

- 주문할 때 수령인에게 보낼 메시지를 작성할 수 있다.
- 상품 옵션과 해당 수량을 선택하여 주문하면 해당 상품 옵션의 수량이 차감된다.
- 해당 상품이 위시 리스트에 있는 경우 위시 리스트에서 삭제한다.
- 나에게 보내기를 읽고 주문 내역을 카카오톡 메시지로 전송한다.
  - 메시지는 메시지 템플릿의 기본 템플릿이나 사용자 정의 템플릿을 사용하여 자유롭게 작성한다.

### API 명세

Request

POST /api/orders HTTP/1.1

Authorization: Bearer {token}

Content-Type: application/json


{

    "optionId": 1,
    "quantity": 2,
    "message": "Please handle this order with care."
    
}

Response

HTTP/1.1 201 Created

Content-Type: application/json

{

    "id": 1,
    "optionId": 1,
    "quantity": 2,
    "orderDateTime": "2024-07-21T10:00:00",
    "message": "Please handle this order with care."
    
}

### 구현 기능 목록

- 카카오 API 설정
- 주문 API 구현
  - 주문 시 주문 내역 카카오톡 메시지로 전송
  - 상품 옵션 선택 및 수량 차감
  - 위시 리스트에서 해당 상품 삭제
- 주문 폼 작성
- 응답 및 예외 처리
- 테스트 코드 작성

## step3

### 기능 요구 사항
API 사양에 관해 클라이언트와 어떻게 소통할 수 있을까? 어떻게 하면 편하게 소통할 수 있을지 고민해 보고 그 방법을 구현한다.

### 구현 기능 목록
- Swagger 사용하여 API 작성하기

</details>

<details id="spring-gift-enhancement">
<summary><h1>spring-gift-enhancement</h1></summary>

## step1

### 기능 요구 사항
상품 정보에 카테고리를 추가한다. 상품과 카테고리 모델 간의 관계를 고려하여 설계하고 구현한다.

- 상품에는 항상 하나의 카테고리가 있어야 한다.
- 상품 카테고리는 수정할 수 있다.
- 관리자 화면에서 상품을 추가할 때 카테고리를 지정할 수 있다.
- 카테고리는 1차 카테고리만 있으며 2차 카테고리는 고려하지 않는다.
- 카테고리의 예시는 아래와 같다.
  - 교환권, 상품권, 뷰티, 패션, 식품, 리빙/도서, 레저/스포츠, 아티스트/캐릭터, 유아동/반려, 디지털/가전, 카카오프렌즈, 트렌드 선물, 백화점, ...
 
### 구현 기능 목록

- Category
  - 엔티티 추가
  - Service 구현
  - Controller 구현
    
- Product
  - 엔티티 수정
  - Service 수정
  - Controller 수정
  - Dto 수정
    
- 테스트 코드 작성

<br>


## step2

### 기능 요구 사항

상품 정보에 옵션을 추가한다. 상품과 옵션 모델 간의 관계를 고려하여 설계하고 구현한다.

- 상품에는 항상 하나 이상의 옵션이 있어야 한다.
  - 옵션 이름은 공백을 포함하여 최대 50자까지 입력할 수 있다.
  - 특수 문자
    - 가능: ( ), [ ], +, -, &, /, _
    - 그 외 특수 문자 사용 불가
  - 옵션 수량은 최소 1개 이상 1억 개 미만이다.
- 중복된 옵션은 구매 시 고객에게 불편을 줄 수 있다. 동일한 상품 내의 옵션 이름은 중복될 수 없다.
- (선택) 관리자 화면에서 옵션을 추가할 수 있다.

### 구현 기능 목록

- Option
  - 엔티티 작성
    - 옵션 유효성 검사
  - 서비스 작성
  - 레포지토리 작성
- Product
  - 엔티티 수정 
    - Option과 Product 연관관계 설정
  - 서비스 작성
    - 상품 등록 시 옵션 작성
- 예외처리
- 테스트 작성

## step3

### 기능 요구 사항

상품 옵션의 수량을 지정된 숫자만큼 빼는 기능을 구현한다.

- 별도의 HTTP API를 만들 필요는 없다.
- 서비스 클래스 또는 엔티티 클래스에서 기능을 구현하고 나중에 사용할 수 있도록 한다.

### 구현 기능 목록

- Option 엔티티에 수량 감소 메소드 추가
- 옵션 수량 감소 서비스 메소드 추가
- 테스트 코드 작성

</details>

<details id="spring-gift-jpa">
<summary><h1>spring-gift-jpa</h1></summary>

## step1

### 기능 요구 사항

지금까지 작성한 JdbcTemplate 기반 코드(https://github.com/jjt4515/spring-gift-wishlist/tree/step3) 를 
JPA로 리팩터링하고 실제 도메인 모델을 어떻게 구성하고 객체와 테이블을 어떻게 매핑해야 하는지 알아본다.

엔티티 클래스와 리포지토리 클래스를 작성해 본다.
@DataJpaTest를 사용하여 학습 테스트를 해 본다.

### 구현 기능 목록

- JPA 설정
  - application.properties 파일 설정
    - H2 데이터베이스 설정
    - Hibernate SQL 로그 설정

- 엔티티 클래스 작성
  - Member 엔티티 클래스 작성
  - Product 엔티티 클래스 작성
  - Wish 엔티티 클래스 작성
  - Token 엔티티 클래스 작성
      
- 리포지토리 클래스 작성
  - MemberJpaRepository 작성
  - ProductJpaRepository 작성
  - WishJpaRepository 작성
  - TokenJpaRepository 작성

- 테스트 코드 작성
  - MemberRepository 테스트 작성
  - MemberService 테스트 작성
  - ProductRepository 테스트 작성
  - ProductService 테스트 작성
  - WishRepository 테스트 작성
  - WishService 테스트 작성
  - TokenRepository 테스트 작성
  - TokenService 테스트 작성

## step 2

### 기능 요구 사항

지금까지 작성한 JdbcTemplate 기반 코드를 JPA로 리팩터링하고 실제 도메인 모델을 어떻게 구성하고 객체와 테이블을 어떻게 매핑해야 하는지 알아본다.

객체의 참조와 테이블의 외래 키를 매핑해서 객체에서는 참조를 사용하고 테이블에서는 외래 키를 사용할 수 있도록 한다.

### 구현 기능 목록

- 엔티티 간의 연관 관계 매핑

    - Member와 WishlistItem 간의 일대다 관계 매핑
    - Product와 WishlistItem 간의 일대다 관계 매핑
    - TokenAuth와 Member 간의 일대일 관계 매핑

## step3

### 기능 요구 사항

상품과 위시 리스트 보기에 페이지네이션을 구현한다.

대부분의 게시판은 모든 게시글을 한 번에 표시하지 않고 여러 페이지로 나누어 표시한다. 정렬 방법을 설정하여 보고 싶은 정보의 우선 순위를 정할 수도 있다.
페이지네이션은 원하는 정렬 방법, 페이지 크기 및 페이지에 따라 정보를 전달하는 방법이다.

### 구현 기능 목록

- 상품 목록 페이지네이션
    - 상품 리스트를 페이지별로 조회할 수 있어야 함.
    - 한 페이지에 표시될 상품의 수는 설정 가능해야 함.
    - 상품은 특정 기준으로 정렬할 수 있어야 함 (예: 이름, 가격).

- 위시 리스트 페이지네이션
    - 회원의 위시 리스트를 페이지별로 조회할 수 있어야 함.
    - 한 페이지에 표시될 위시 아이템의 수는 설정 가능해야 함.
    - 위시 리스트는 특정 기준으로 정렬할 수 있어야 함 (예: 추가된 날짜).

</details>

<details id="spring-gift-wishlist">
<summary><h1>spring-gift-wishlist</h1></summary>
  
## step1

### 기능 요구 사항
상품을 추가하거나 수정하는 경우, 클라이언트로부터 잘못된 값이 전달될 수 있다. 잘못된 값이 전달되면 클라이언트가 어떤 부분이 왜 잘못되었는지 인지할 수 있도록 응답을 제공한다.

- 상품 이름은 공백을 포함하여 최대 15자까지 입력할 수 있다.
- 특수 문자
  - 가능: ( ), [ ], +, -, &, /, _
  - 그 외 특수 문자 사용 불가
- "카카오"가 포함된 문구는 담당 MD와 협의한 경우에만 사용할 수 있다.

### 구현 기능 목록

- validation
  - 상품이름 글자수 최대 15자
  - 상품이름 특수 문자 일부만 사용가능( ), [ ], +, -, &, /, _
  - 상품이름에 "카카오"가 포함된 문구 제한
  - 가격은 양의 정수
- 예외처리
  - 존재하지 않는 상품인 경우
  - 상품 데이터가 유효하지 않는 경우
 
## step2

### 기능 요구 사항
사용자가 회원 가입, 로그인, 추후 회원별 기능을 이용할 수 있도록 구현한다.

- 회원은 이메일과 비밀번호를 입력하여 가입한다.
- 토큰을 받으려면 이메일과 비밀번호를 보내야 하며, 가입한 이메일과 비밀번호가 일치하면 토큰이 발급된다.
- 토큰을 생성하는 방법에는 여러 가지가 있다. 방법 중 하나를 선택한다.
- (선택) 회원을 조회, 추가, 수정, 삭제할 수 있는 관리자 화면을 구현한다.

### 구현 기능 목록
- 멤버 회원가입
  - 토큰 반환
  - 예외 처리 
- 멤버 로그인
  - 토큰 반환
  - 예외 처리
 
## step3

### 기능 요구 사항
이전 단계에서 로그인 후 받은 토큰을 사용하여 사용자별 위시 리스트 기능을 구현한다.

- 위시 리스트에 등록된 상품 목록을 조회할 수 있다.
- 위시 리스트에 상품을 추가할 수 있다.
- 위시 리스트에 담긴 상품을 삭제할 수 있다.

### 구현 기능 목록
- 멤버별 위시 리스트
  - 위시 리스트 상품 목록 조회
  - 위시 리스트 상품 추가
  - 위시 리스트 상품 삭제
- 예외 처리

</details>

<details id="spring-gift-product">
<summary><h1>spring-gift-product</h1></summary>

### 상품 관리 프로젝트

<hr>

<br>


### 구현 기능 목록

<hr>

### STEP1 (CRUD 작성)
1. 상품 조회
2. 상품 추가
3. 상품 수정
4. 상품 삭제

### STEP2 (페이지 작성)
1. 상품 리스트
2. 상품 추가 폼
3. 상품 수정 폼

### STEP3 (데이터 베이스 연동)
1. H2 DB 연동

<br>

### junit으로 테스트 코드 작성

</details>
