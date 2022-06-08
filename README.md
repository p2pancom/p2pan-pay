## 소개

p2pan-pay API는 P2Pan 에서 제공하는 코인 결제 API로 머천트가 사용자로부터 편리하게 코인 지불을 받을 수 있도록 서비스 및 정보를 제공합니다. API 호출은 표준 HTTP POST (application/json) 호출로 구현됩니다.

- 머천트 웹페이지: https://pay.p2pan.com
<br><br>

## API 엔드포인트

- 메인 노드

```
https://api.p2pan.com/v1
```

### 엔드포인트에 대한 일반 정보

- `GET` 엔드포인트의 경우 매개변수를 `query string` 으로 전송할 수 있습니다.
- `POST`, `PUT` 및 `DELETE` 끝점의 경우 매개변수는 `query string` 으로 전송되거나 `content-type` 이 `application/json` 요청으로 전송될 수 있습니다.
- 매개변수는 어떤 순서로든 보낼 수 있습니다.
- `query string` 과 `request body` 모두에 매개변수가 전송되면 `query string` 매개변수가 사용됩니다.

### HTTP 반환 코드

- HTTP 4XX 반환 코드는 잘못된 요청에 사용됩니다. 문제는 호출자 측에 있습니다.
- HTTP 403 반환 코드는 WAF 제한(웹 응용 프로그램 방화벽)을 위반했을 때 사용됩니다.
- HTTP 429 반환 코드는 요청 속도 제한을 위반할 때 사용됩니다.
- HTTP 418 반환 코드는 코드를 받은 후 요청을 계속 보내기 위해 IP가 자동 금지되었을 때 사용됩니다.
- HTTP 5XX 반환 코드는 내부 오류에 사용됩니다. 문제는 API 서버측입니다.

### 엔드포인트 인증

API 호출에는 머천트 페이지에서 발급받은 API KEY 를 요구 합니다. API KEY 는 API 엔드포인트 요청시 `X-API-Credential` 헤더로 전송합니다.

- HTTP Header:

```
X-API-Credential: API Key
```

<br>

## 지원하는 코인 및 토큰

### 지원하는 코인

- `BTC`&nbsp;&nbsp;&nbsp;Bitcoin
- `ETH`&nbsp;&nbsp;&nbsp;Ethereum
- `XRP`&nbsp;&nbsp;&nbsp;Ripple
- `ETC`&nbsp;&nbsp;&nbsp;Ethereum Classic
- `BCH`&nbsp;&nbsp;&nbsp;Bitcoin Cash
- `DASH`&nbsp;Dash
- `LTC`&nbsp;&nbsp;&nbsp;Litecoin
- `DOGE`&nbsp;Dogecoin
- `ZEC`&nbsp;&nbsp;&nbsp;Zcash

### 지원하는 토큰

- `USDT`&nbsp;Tether
- `USDC`&nbsp;USD Coin
- `DAI`&nbsp;&nbsp;&nbsp;Dai

<br>

## API 목록

### 입금 주소 생성

```
POST /payment/getAddress
```

- 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제ID 또는 결제CODE</td>
    </tr>
    <tr>
      <td>currency</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
  </tbody>
</table>
<br>

### 결제폼 생성

```
GET /payment/new
```

- 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>name</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>description</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>pricingType</td>
      <td>STRING</td>
      <td>예</td>
      <td>fixed</td>
    </tr>
    <tr>
      <td>localPrice</td>
      <td>OBJECT</td>
      <td>예</td>
      <td>{"amount":"", "currency":""}</td>
    </tr>
    <tr>
      <td>redirectUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>cancelUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

- 요청 본문 예시

> Content-Type: application/json

```json
{
  "name": "Payment Test",
  "description": "Test Description",
  "pricingType": "fixed",
  "localPrice": {
    "amount": "1000000",
    "currency": "KRW"
  },
  "metadata": {
    "user_id": "2293382",
    "order_id": "339128"
  }
}
```

- 결제폼 생성후 웹 결제 페이지를 사용자에게 제공하고 지불을 받을 수 있습니다.

```
https://pay.p2pan.com/payment/:id
```

경로 매개변수 id 는 `결제ID` 또는 `결제CODE` 로 사용됩니다.

- 예시

```
https://pay.p2pan.com/payment/AJ3SOZV9
```
<br>

### 결제폼 취소

```
POST /payment/cancel/:id
```

- 경로 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제ID 또는 결제CODE</td>
    </tr>
  </tbody>
</table>
<br>

### 결제폼 정보

```
GET /payment/get/:id
```

- 경로 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제ID 또는 결제CODE</td>
    </tr>
  </tbody>
</table>
<br>


### 출금 생성

```
POST /withdraw/new
```

- 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>currency</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>amount</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>address</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>addressTag</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>autoConfirm</td>
      <td>BOOLEAN</td>
      <td>아니요</td>
      <td>기본값 false</td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>
<br>

### 출금 취소

```
POST /withdraw/cancel/:id
```

- 경로 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금 ID</td>
    </tr>
  </tbody>
</table>
<br>

### 출금 정보

```
GET /withdraw/get/:id
```

- 경로 매개변수
<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금 ID</td>
    </tr>
  </tbody>
</table>
<br>

## 이벤트 웹훅

이벤트 웹훅은 결제창의 상태가 변경될 경우 지정한 웹훅 주소로 상태 정보를 전송합니다.
머천트는 해당 데이터를 받아 사용자가 입금한 내역을 확인하고 내부적으로 거래를 처리할 수 있습니다.

### 웹훅 데이터 보안

전송받은 웹훅 데이터가 스팸에 의한 것이 아닌 올바른 데이터 인지 확인합니다.

`X-Webhook-Signature: HMAC SHA256 Hash`

모든 웹훅 요청에는 원시 데이터에 대한 `HMAC SHA256` 값이 포함됩니다.

머천트의 `Webhook Secret Key` 를 키로 사용하여 암호화 됩니다.

머천트는 해당 헤더를 비교하여 올바르지 않을 경우 웹훅 데이터에 대한 처리를 거부해야 합니다.

### 웹훅 데이터 예시

모든 웹훅 요청은 `HTTP POST` 방식의 `application/json` 로 전송됩니다.

```json
{
  "id": "ac855633-ab15-13ec-a49d-f21acde77bfd",
  "resource": "event",
  "type": "payment:created",
  "createdAt": "2022-05-23T18:57:46.000Z",
  "data": {
    "id": "bb6d9079-ab11-13ac-249d-2w20cde77bfd",
    "code": "NUMD1YYA",
    "name": "Payment",
    "description": "Payment Test",
    "status": "NEW",
    "pricingType": "fixed",
    "pricing": {},
    "incoming": {},
    "metadata": {},
    "createdAt": "2022-05-23T18:57:45.000Z",
    "expiresAt": "2022-05-23T19:57:45.000Z",
    "localAmount": "10000.000000000000000000000000",
    "localCurrency": "KRW",
    "payCurrency": "ETH"
  }
}
```

- 웹훅 헤더 필드
<table>
  <thead>
    <tr>
      <td>필드 명</td>
      <td>필드 타입</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>웹훅 ID</td>
    </tr>
    <tr>
      <td>resource</td>
      <td>STRING</td>
      <td>event</td>
    </tr>
    <tr>
      <td>type</td>
      <td>STRING</td>
      <td>payment:created<br>payment:pending<br>payment:confirmed<br>payment:failed</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>STRING</td>
      <td>웹훅 생성 시간</td>
    </tr>
  </tbody>
</table>

- 웹훅 데이터 필드
<table>
  <thead>
    <tr>
      <td>필드 명</td>
      <td>필드 타입</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>웹훅 ID</td>
    </tr>
    <tr>
      <td>code</td>
      <td>STRING</td>
      <td>웹훅 CODE</td>
    </tr>
    <tr>
      <td>name</td>
      <td>STRING</td>
      <td>페이폼 이름</td>
    </tr>
    <tr>
      <td>description</td>
      <td>STRING</td>
      <td>페이폼 설명</td>
    </tr>
    <tr>
      <td>status</td>
      <td>STRING</td>
      <td>페이폼 상태</td>
    </tr>
    <tr>
      <td>pricing</td>
      <td>OBJECT</td>
      <td>{"BTC":"0.02293","ETH":"0.2193"}</td>
    </tr>
    <tr>
      <td>incoming</td>
      <td>OBJECT</td>
      <td>{"BTC":"0.00293"}</td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>커스텀 필드</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>STRING</td>
      <td>페이폼 생성시간</td>
    </tr>
    <tr>
      <td>expiresAt</td>
      <td>STRING</td>
      <td>페이폼 만료시간</td>
    </tr>
    <tr>
      <td>localAmount</td>
      <td>STRING</td>
      <td>결제 금액</td>
    </tr>
    <tr>
      <td>localCurrency</td>
      <td>STRING</td>
      <td>결제 통화</td>
    </tr>
    <tr>
      <td>payCurrency</td>
      <td>STRING</td>
      <td>결제한 코인</td>
    </tr>
    <tr>
      <td>pricingType</td>
      <td>STRING</td>
      <td>fixed</td>
    </tr>
  </tbody>
</table>
