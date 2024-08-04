# spring-gift-point

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
