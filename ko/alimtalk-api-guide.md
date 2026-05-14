## Notification > KakaoTalk Bizmessage > 알림톡 > API v2.3 Guide

## 알림톡

#### [API Domain]

<table>
<thead>
<tr>
<th>Domain</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://kakaotalk-bizmessage.api.nhncloudservice.com</td>
</tr>
</tbody>
</table>

## Overview of v2.3 API

1. 알림톡 바로 연결, 아이템리스트, 톡 비즈 플러그인, 대표 링크, 비즈니스폼 버튼 기능이 추가되었습니다.
2. 알림톡 아이템 하이라이트 이미지 등록 API가 추가되었습니다.
3. 알림톡 플러그인 등록/수정/삭제/조회 API가 추가되었습니다.
4. 메시지 리스트 조회 API에서 buttons 필드가 삭제되었습니다.

## 일반 메시지

### 메시지 치환 발송 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름                       | 	타입     | 	필수 | 	설명                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | 콘솔에서 생성할 수 있습니다.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 중복 메시지 발송 요청 기준 key<br>10분간 동일한 key로 요청 시 해당 요청을 실패 처리합니다. |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String,
            "telNumber": String
          }
        ],
        "quickReplies": [
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 이름                     | 	타입      | 	필수 | 	설명                                                                                           |
|------------------------|----------|-----|-----------------------------------------------------------------------------------------------|
| senderKey              | 	String  | 	O  | 발신 키(40자)                                                                                     |
| templateCode           | 	String  | 	O  | 등록한 발송 템플릿 코드(최대 20자)                                                                         |
| requestDate            | String   | X   | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                            |
| senderGroupingKey      | String   | X   | 발신 그룹핑 키(최대 100자)                                                                             |
| createUser             | String   | X   | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                   |
| recipientList          | 	List    | 	O  | 	수신자 리스트(최대 1000명)                                                                            |
| - recipientNo          | 	String  | 	O  | 	수신번호(최대 15자)                                                                                 |
| - templateParameter    | 	Object  | 	X  | 	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수)                                                           |
| -- key                 | 	String  | 	X  | 	치환 키(#{key})                                                                                 |
| -- value               | String   | 	X  | 	치환 키에 매핑되는 Value값                                                                            |
| - resendParameter      | 	Object  | 	X  | 대체 발송 정보                                                                                      |
| -- isResend            | 	boolean | 	X  | 	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                      |
| -- resendType          | 	String  | 	X  | 	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                      |
| -- resendTitle         | 	String  | 	X  | 	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                              |
| -- resendContent       | 	String  | 	X  | 	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.)                        |
| -- resendSendNo        | String   | X   | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
| - buttons              | 	List    | 	X  | 버튼 추가 정보                                                                                      |
| -- ordering            | Integer  | X   | 	버튼 순서(버튼이 있는 경우 필수)                                                                          |
| -- chatExtra           | 	String  | 	X  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보                                                       |
| -- chatEvent           | 	String  | 	X  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                                                                  |
| -- relayId             | 	String  | 	X  | 플러그인 실행 시 X-Kakao-Plugin-Relay-Id 헤더를 통해 전달받을 값                                               |
| -- oneClickId          | 	String  | 	X  | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                      |
| -- productId           | 	String  | 	X  | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                      |
| -- target              | 	String  | 	X  | 	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송                                    |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| - quickReplies         | 	List    | 	X  | 바로연결 정보                                                                                       |
| -- ordering            | Integer  | X   | 	바로연결 순서(바로연결이 있는 경우 필수)                                                                      |
| -- chatExtra           | 	String  | 	X  | BC(상담톡 전환) / BT(봇 전환) 타입 시, 전달할 메타정보                                                          |
| -- chatEvent           | 	String  | 	X  | BT(봇 전환) 타입 시, 연결할 봇 이벤트명                                                                     |
| -- target              | 	String  | 	X  | 	웹 링크 타입일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송                                    |
| - recipientGroupingKey | 	String  | 	X  | 	수신자 그룹핑 키(최대 100자)                                                                           |
| messageOption          | Object   | 	X  | 메시지 옵션                                                                                        |
| - price                | Integer  | 	X  | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액(모먼트 광고에 해당)                                                   |
| - currencyType         | String   | 	X  | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액의 통화 단위 KRW, USD, EUR 등 국제 통화 코드 사용(모먼트 광고에 해당)                |
| statsId                | String   | 	X  | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                            |

* <b>요청 일시는 호출하는 시점부터 60일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>대체 발송은 SMS, LMS로 발송 가능하며, 국제 대체 발송은 SMS만 지원합니다.국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>
* <b>templateTitle과 templateItemHighlight.title 필드 맨 뒤에 치환자와 templateParameter를 이용해서 `\s` 문자를 추가할 경우 취소선 스타일을 적용할 수 있습니다.</b>
    * <b>단, 템플릿 등록 시 미리 \s를 필드에 추가해 두는 경우에는 적용되지 않습니다.</b>

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "senderGroupingKey": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String,
        "recipientGroupingKey": String
      }
    ]
  }
}
```

| 이름                      | 타입      | Not Null | 설명           |
|-------------------------|---------|:--------:|--------------|
| header                  | Object  |    O     | 헤더 영역        |
| - resultCode            | Integer |    O     | 결과 코드        |
| - resultMessage         | String  |    O     | 결과 메시지       |
| - isSuccessful          | Boolean |    O     | 성공 여부        |
| message                 | Object  |    X     | 본문 영역        |
| - requestId             | String  |    X     | 요청 아이디       |
| - senderGroupingKey     | String  |    X     | 발신 그룹핑 키     |
| - sendResults           | Object  |    O     | 발송 요청 결과     |
| -- recipientSeq         | Integer |    O     | 수신자 시퀀스 번호   |
| -- recipientNo          | String  |    X     | 수신 번호        |
| -- resultCode           | Integer |    O     | 발송 요청 결과 코드  |
| -- resultMessage        | String  |    O     | 발송 요청 결과 메시지 |
| -- recipientGroupingKey | String  |    X     | 수신자 그룹핑 키    |

### 메시지 전문 발송 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름                       | 	타입     | 	필수 | 	설명                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | 콘솔에서 생성할 수 있습니다.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 중복 메시지 발송 요청 기준 key<br>10분간 동일한 key로 요청 시 해당 요청을 실패 처리합니다. |

[Request Body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle": String,
            "templateHeader": String,
            "templateItem": {
              "list": [{
                "title": String,
                "description": String
              }],
              "summary": {
                "title": String,
                "description": String
              }
            },
            "templateItemHighlight": {
              "title": String,
              "description": String,
              "imageUrl": String
            },
            "templateRepresentLink": {
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            },
            "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "chatExtra": String,
                    "chatEvent": String,
                    "bizFormId": Integer,
                    "pluginId": String,
                    "relayId": String,
                    "oneClickId": String,
                    "productId": String,
                    "target": String,
                    "telNumber": String
                }
            ],
            "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "chatExtra": String,
                    "chatEvent": String,
                    "bizFormId": Integer,
                    "target": String
                }
            ],
            "resendParameter": {
              "isResend": boolean,
              "resendType": String,
              "resendTitle": String,
              "resendContent": String,
              "resendSendNo": String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 이름                      | 	타입      | 	필수 | 	설명                                                                                                                                                                        |
|-------------------------|----------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey               | 	String  | 	O  | 발신 키(40자)                                                                                                                                                                  |
| templateCode            | 	String  | 	O  | 등록한 발송 템플릿 코드(최대 20자)                                                                                                                                                      |
| requestDate             | String   | X   | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                         |
| senderGroupingKey       | String   | X   | 발신 그룹핑 키(최대 100자)                                                                                                                                                          |
| createUser              | String   | X   | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                |
| recipientList           | 	List    | 	O  | 	수신자 리스트(최대 1,000명)                                                                                                                                                        |
| - recipientNo           | 	String  | 	O  | 	수신번호(최대 15자)                                                                                                                                                              |
| - content               | 	String  | 	O  | 	내용(최대 1300자)                                                                                                                                                              |
| - templateTitle         | String   | X   | 제목(최대 50자)                                                                                                                                                                 |
| - templateHeader        | String   | X   | 템플릿 헤더(최대 16자)                                                                                                                                                             |
| - templateItem          | Object   | X   | 아이템                                                                                                                                                                        |
| -- list                 | List     | X   | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                     |
| --- title               | String   | X   | 타이틀(최대 6자)                                                                                                                                                                 |
| --- description         | String   | X   | 설명(최대 23자)                                                                                                                                                              |
| -- summary              | Object   | X   | 아이템 요약 정보                                                                                                                                                                  |
| --- title               | String   | X   | 타이틀(최대 6자)                                                                                                                                                                 |
| --- description         | String   | X   | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                              |
| - templateItemHighlight | Object   | X   | 아이템 하이라이트                                                                                                                                                                  |
| --- title               | String   | X   | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                            |
| --- description         | String   | X   | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                          |
| --- imageUrl            | String   | X   | 섬네일 이미지 주소                                                                                                                                                                 |
| - templateRepresentLink | Object   | X   | 대표 링크                                                                                                                                                                      |
| -- linkMo               | String   | 	X  | 	모바일 웹 링크(최대 500자)                                                                                                                                                         |
| -- linkPc               | String   | 	X  | PC 웹 링크(최대 500자)                                                                                                                                                           |
| -- schemeIos            | String   | X   | 	iOS 앱 링크(최대 500자)                                                                                                                                                         |
| -- schemeAndroid        | String   | X   | 	안드로이드 앱 링크(최대 500자)                                                                                                                                                       |
| - buttons               | 	List    | 	X  | 버튼 리스트(최대 5개)                                                                                                                                                              |
| -- ordering             | 	Integer | 	X  | 	버튼 순서(버튼이 있는 경우 필수)                                                                                                                                                       |
| -- type                 | String   | 	X  | 	버튼 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| -- name                 | String   | 	X  | 	버튼 이름(버튼이 있는 경우 필수, 최대 14자)                                                                                                                                               |
| -- linkMo               | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| -- linkPc               | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                      |
| -- chatExtra            | 	String  | 	X  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보                                                                                                                                    |
| -- chatEvent            | 	String  | 	X  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                                                                                                                                               |
| -- bizFormId            | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                    |
| -- pluginId             | 	String  | 	X  | 	플러그인 ID(최대 24자)                                                                                                                                                           |
| -- relayId              | 	String  | 	X  | 플러그인 실행 시 X-Kakao-Plugin-Relay-Id 헤더를 통해 전달받을 값                                                                                                                            |
| -- oneClickId           | 	String  | 	X  | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                                   |
| -- productId            | 	String  | 	X  | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                                   |
| -- target               | 	String  | 	X  | 	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송                                                                                                                 |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| - quickReplies          | 	List    | 	X  | 바로연결 리스트(최대 5개)                                                                                                                                                            |
| -- ordering             | 	Integer | 	X  | 	바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                   |
| -- type                 | String   | 	X  | 	바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                   |
| -- name                 | String   | 	X  | 	바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                           |
| -- linkMo               | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| -- linkPc               | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                      |
| -- bizFormId            | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                    |
| -- target               | 	String  | 	X  | 	웹 링크 타입일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송                                                                                                                 |
| - resendParameter       | 	Object  | 	X  | 대체 발송 정보                                                                                                                                                                   |
| -- isResend             | 	boolean | 	X  | 	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                   |
| -- resendType           | 	String  | 	X  | 	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                   |
| -- resendTitle          | 	String  | 	X  | 	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                           |
| -- resendContent        | 	String  | 	X  | 	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 대체 발송됩니다.)                                                                                                     |
| -- resendSendNo         | String   | X   | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                              |
| - recipientGroupingKey  | 	String  | 	X  | 	수신자 그룹핑 키(최대 100자)                                                                                                                                                        |
| messageOption           | Object   | 	X  | 메시지 옵션                                                                                                                                                                     |
| - price                 | Integer  | 	X  | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액(모먼트 광고에 해당)                                                                                                                                |
| - currencyType          | String   | 	X  | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액의 통화 단위 KRW, USD, EUR 등 국제 통화 코드 사용(모먼트 광고에 해당)                                                                                             |
| statsId                 | String   | 	X  | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                         |

* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>
* <b>요청 일시는 호출하는 시점부터 60일 후까지 설정 가능합니다.</b>
* <b>SMS 서비스에서 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 각종 필드 길이 제한 등)</b>
* <b>대체 발송은 SMS, LMS로 발송 가능하며, 국제 대체 발송은 SMS만 지원합니다.국제 수신자 번호일 경우, resendType(대체 발송 타입)을 SMS로 변경해야 정상적으로 대체 발송할 수 있습니다.</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>
* <b>발송 시점에 templateTitle과 templateItemHighlight.title 필드 맨 뒤에 `\s` 문자를 추가할 경우 취소선 스타일을 적용할 수 있습니다.</b>
    * <b>단, 템플릿 등록 시 미리 \s를 필드에 추가해 두는 경우에는 적용되지 않습니다.</b>

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/raw-messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "senderGroupingKey": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String,
        "recipientGroupingKey": String
      }
    ]
  }
}
```

| 이름                      | 타입      | Not Null | 설명           |
|-------------------------|---------|:--------:|--------------|
| header                  | Object  |    O     | 헤더 영역        |
| - resultCode            | Integer |    O     | 결과 코드        |
| - resultMessage         | String  |    O     | 결과 메시지       |
| - isSuccessful          | Boolean |    O     | 성공 여부        |
| message                 | Object  |    X     | 본문 영역        |
| - requestId             | String  |    X     | 요청 아이디       |
| - senderGroupingKey     | String  |    X     | 발신 그룹핑 키     |
| - sendResults           | Object  |    O     | 발송 요청 결과     |
| -- recipientSeq         | Integer |    O     | 수신자 시퀀스 번호   |
| -- recipientNo          | String  |    X     | 수신 번호        |
| -- resultCode           | Integer |    O     | 발송 요청 결과 코드  |
| -- resultMessage        | String  |    O     | 발송 요청 결과 메시지 |
| -- recipientGroupingKey | String  |    X     | 수신자 그룹핑 키    |

### 메시지 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter] 1번 or(2번, 3번) 조건 필수

| 이름                   | 	타입      | 	필수        | 	설명                                                   |
|----------------------|----------|------------|-------------------------------------------------------|
| requestId            | 	String  | 	조건 필수(1번) | 요청 아이디                                                |
| startRequestDate     | 	String  | 	조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)                       |
| endRequestDate       | 	String  | 조건 필수(2번)  | 	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm)                       |
| startCreateDate      | String   | 조건 필수(3번)  | 등록 날짜 시작값(yyyy-MM-dd HH:mm)                           |
| endCreateDate        | String   | 조건 필수(3번)  | 등록 날짜 끝값(yyyy-MM-dd HH:mm)                            |
| recipientNo          | 	String  | 	X         | 	수신번호                                                 |
| senderKey            | 	String  | 	X         | 	발신 키                                                 |
| templateCode         | 	String  | 	X         | 	템플릿 코드                                               |
| senderGroupingKey    | String   | X          | 발신 그룹핑 키                                              |
| recipientGroupingKey | 	String  | 	X         | 	수신자 그룹핑 키                                            |
| messageStatus        | String   | 	X         | 요청 상태( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	 |
| resultCode           | String   | 	X         | 발송 결과( MRC01 -> 성공 MRC02 -> 실패 )	                     |
| createUser           | String   | X          | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                           |
| pageNum              | 	Integer | 	X         | 	페이지 번호(Default: 1)                                   |
| pageSize             | 	Integer | 	X         | 	조회 건수(Default: 15, Max: 1000)                        |

* 90일 이전 발송 요청 데이터는 조회되지 않습니다.
* 발송 요청 일시의 범위는 최대 30일입니다.

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messageSearchResultResponse": {
    "messages": [
    {
      "requestId": String,
      "recipientSeq": Integer,
      "plusFriendId": String,
      "senderKey": String,
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount": Integer
  }
}
```

| 이름                          | 타입      | Not Null | 설명                                                                                                                                                                     |
|-----------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                      | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode                | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage             | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful              | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| messageSearchResultResponse | Object  |    X     | 본문 영역                                                                                                                                                                  |
| - messages                  | List    |    O     | 메시지 리스트                                                                                                                                                                |
| -- requestId                | String  |    O     | 요청 아이디                                                                                                                                                                 |
| -- recipientSeq             | Integer |    O     | 수신자 시퀀스 번호                                                                                                                                                             |
| -- plusFriendId             | String  |    O     | 플러스친구 ID                                                                                                                                                               |
| -- senderKey                | String  |    O     | 발신 키                                                                                                                                                                   |
| -- templateCode             | String  |    O     | 템플릿 코드                                                                                                                                                                 |
| -- recipientNo              | String  |    O     | 수신 번호                                                                                                                                                                  |
| -- content                  | String  |    X     | 본문                                                                                                                                                                     |
| -- requestDate              | String  |    O     | 요청 일시                                                                                                                                                                  |
| -- createDate               | String  |    O     | 등록 일시                                                                                                                                                                  |
| -- receiveDate              | String  |    X     | 수신 일시                                                                                                                                                                  |
| -- resendStatus             | String  |    O     | 대체 발송 상태 코드(RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| -- resendStatusName         | String  |    O     | 대체 발송 상태 코드명                                                                                                                                                           |
| -- messageStatus            | String  |    O     | 요청 상태( COMPLETED -\> 성공, FAILED -\> 실패, CANCEL -\> 취소 )                                                                                                                |
| -- createUser               | String  |    X     | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                            |
| -- resultCode               | String  |    X     | 수신 결과 코드                                                                                                                                                               |
| -- resultCodeName           | String  |    X     | 수신 결과 코드명                                                                                                                                                              |
| -- senderGroupingKey        | String  |    X     | 발신 그룹핑 키                                                                                                                                                               |
| -- recipientGroupingKey     | String  |    X     | 수신자 그룹핑 키                                                                                                                                                              |
| - totalCount                | Integer |    X     | 총개수                                                                                                                                                                    |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입      | 	설명         |
|--------------|----------|-------------|
| appkey       | 	String  | 	고유의 앱키     |
| requestId    | 	String  | 	요청 아이디     |
| recipientSeq | 	Integer | 	수신자 시퀀스 번호 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "message": {
      "requestId": String,
      "recipientSeq": Integer,
      "plusFriendId": String,
      "senderKey": String,
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "templateTitle": String,
      "templateSubtitle": String,
      "templateExtra": String,
      "templateAd": String,
      "templateHeader": String,
      "templateItem": {
        "list": [{
          "title": String,
          "description": String
        }],
        "summary": {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight": {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink": {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "requestDate": String,
      "receiveDate": String,
      "createDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
      "buttons": [
        {
          "ordering": Integer,
          "type": String,
          "name": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "bizFormId": Integer,
          "pluginId": String,
          "relayId": String,
          "oneClickId": String,
          "productId": String,
          "target": String,
          "telNumber": String
        }
      ],
      "quickReplies": [
        {
          "ordering": Integer,
          "type": String,
          "name": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "bizFormId": Integer,
          "target": String
          }
      ],
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

-----

| 이름                      | 타입      | Not Null | 설명                                                                                                                                                                     |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode            | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage         | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful          | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| message                 | Object  |    X     | 메시지                                                                                                                                                                    |
| - requestId             | String  |    O     | 요청 아이디                                                                                                                                                                 |
| - recipientSeq          | Integer |    O     | 수신자 시퀀스 번호                                                                                                                                                             |
| - plusFriendId          | String  |    O     | 플러스친구 ID                                                                                                                                                               |
| - senderKey             | String  |    O     | 발신 키                                                                                                                                                                   |
| - templateCode          | String  |    O     | 템플릿 코드                                                                                                                                                                 |
| - recipientNo           | String  |    X     | 수신 번호                                                                                                                                                                  |
| - content               | String  |    X     | 본문                                                                                                                                                                     |
| - templateTitle         | String  |    X     | 템플릿 제목                                                                                                                                                                 |
| - templateSubtitle      | String  |    X     | 템플릿 보조 문구                                                                                                                                                              |
| - templateExtra         | String  |    X     | 템플릿 부가 내용                                                                                                                                                              |
| - templateAd            | String  |    X     | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구                                                                                                                                            |
| - templateHeader        | String  |    X     | 템플릿 헤더(최대 16자)                                                                                                                                                         |
| - templateItem          | Object  |    X     | 아이템                                                                                                                                                                    |
| -- list                 | List    |    X     | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                 |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description         | String  |    X     | 설명(최대 23자)                                                                                                                                                          |
| -- summary              | Object  |    X     | 아이템 요약 정보                                                                                                                                                              |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description         | String  |    X     | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                          |
| - templateItemHighlight | Object  |    X     | 아이템 하이라이트                                                                                                                                                              |
| --- title               | String  |    X     | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                        |
| --- description         | String  |    X     | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                      |
| --- imageUrl            | String  |    X     | 섬네일 이미지 주소                                                                                                                                                             |
| - templateRepresentLink | Object  |    X     | 대표 링크                                                                                                                                                                  |
| -- linkMo               | String  |    X     | 모바일 웹 링크(최대 500자)                                                                                                                                                      |
| -- linkPc               | String  |    X     | PC 웹 링크(최대 500자)                                                                                                                                                       |
| -- schemeIos            | String  |    X     | iOS 앱 링크(최대 500자)                                                                                                                                                      |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(최대 500자)                                                                                                                                                    |
| - requestDate           | String  |    O     | 요청 일시                                                                                                                                                                  |
| - receiveDate           | String  |    X     | 수신 일시                                                                                                                                                                  |
| - createDate            | String  |    O     | 등록 일시                                                                                                                                                                  |
| - resendStatus          | String  |    O     | 대체 발송 상태 코드(RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName      | String  |    O     | 대체 발송 상태 코드명                                                                                                                                                           |
| - resendResultCode      | String  |    X     | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId       | String  |    X     | 대체 발송 SMS 요청 ID                                                                                                                                                        |
| - messageStatus         | String  |    O     | 요청 상태( COMPLETED -\> 성공, FAILED -\> 실패, CANCEL -\> 취소 )                                                                                                                |
| - resultCode            | String  |    X     | 수신 결과 코드                                                                                                                                                               |
| - resultCodeName        | String  |    X     | 수신 결과 코드명                                                                                                                                                              |
| - createUser            | String  |    X     | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                            |
| - buttons               | List    |    X     | 버튼 리스트                                                                                                                                                                 |
| -- ordering             | Integer |    X     | 버튼 순서                                                                                                                                                                  |
| -- type                 | String  |    X     | 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| -- name                 | String  |    X     | 버튼 이름                                                                                                                                                                  |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드)                                                                                                                                              |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드)                                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                              |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                            |
| -- chatExtra            | String  |    X     | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보                                                                                                                                |
| -- chatEvent            | String  |    X     | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                                                                                                                                           |
| -- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                 |
| -- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                        |
| -- relayId              | String  |    X     | 플러그인 실행 시 X-Kakao-Plugin-Relay-Id 헤더를 통해 전달받을 값                                                                                                                        |
| -- oneClickId           | String  |    X     | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                               |
| -- productId            | String  |    X     | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                               |
| -- target               | String  |    X     | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크\<br\>기본 인앱 링크로 발송                                                                                                            |
| - quickReplies          | List    |    X     | 바로연결 리스트(최대 5개)                                                                                                                                                        |
| -- ordering             | Integer |    X     | 바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                |
| -- type                 | String  |    X     | 바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                |
| -- name                 | String  |    X     | 바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                        |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                   |
| -- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                        |
| -- target               | String  |    X     | 웹 링크 타입일 경우, "target":"out" 속성 추가 시 아웃 링크\<br\>기본 인앱 링크로 발송                                                                                                            |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| - messageOption         | Object  |    X     | 메시지 옵션                                                                                                                                                                 |
| -- price                | Integer |    X     | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액(모먼트 광고에 해당)                                                                                                                            |
| -- currencyType         | String  |    X     | 사용자에게 전달될 메시지 내 포함된 가격/금액/결제 금액의 통화 단위 KRW, USD, EUR 등 국제 통화 코드 사용(모먼트 광고에 해당)                                                                                         |
| - senderGroupingKey     | String  |    X     | 발신 그룹핑 키                                                                                                                                                               |
| - recipientGroupingKey  | String  |    X     | 수신자 그룹핑 키                                                                                                                                                              |

## 인증 메시지

<span id="precautions-authword"></span>

1. 인증 메시지 발송 시 포함되어야 할 인증 문구 안내

| 구분     | 인증 문구                                      |
|--------|--------------------------------------------|
| 인증 메시지 | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- 예시 1-1) 인증 메시지 API 요청시 전문(템플릿 치환자 포함)에 인증 문구가 포함되어 있지 않은 경우 발송 실패됩니다.
- 예시 1-2) 인증 문구가 영문인 경우 대소문자 구분 없이 유효성 검사가 진행됩니다.

### 메시지 치환 발송 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름                       | 	타입     | 	필수 | 	설명                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | 콘솔에서 생성할 수 있습니다.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 중복 메시지 발송 요청 기준 key<br>10분간 동일한 key로 요청 시 해당 요청을 실패 처리합니다. |

[Request body]
[위와 동일](./alimtalk-api-guide/#_3)

### 메시지 전문 발송 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름                       | 	타입     | 	필수 | 	설명                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | 콘솔에서 생성할 수 있습니다.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 중복 메시지 발송 요청 기준 key<br>10분간 동일한 key로 요청 시 해당 요청을 실패 처리합니다. |

[Request Body]
[위와 동일](./alimtalk-api-guide/#_5)

### 메시지 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]
[위와 동일](./alimtalk-api-guide/#_7)

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입      | 	설명         |
|--------------|----------|-------------|
| appkey       | 	String  | 	고유의 앱키     |
| requestId    | 	String  | 	요청 아이디     |
| recipientSeq | 	Integer | 	수신자 시퀀스 번호 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
```

#### 응답

[위와 동일](./alimtalk-api-guide/#_9)

## 메시지

### 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| requestId | String  | 요청 ID   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름           | 	타입     | 	필수 | 	설명                                         |
|--------------|---------|-----|---------------------------------------------|
| recipientSeq | 	String | 	X  | 수신자 시퀀스 번호<br>(입력하지 않으면 요청 ID의 모든 발송 건을 취소) |

* 일반/인증 메시지 모두 동일한 API로 취소할 수 있습니다.

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

[예시]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### 메시지 결과 업데이트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름                  | 	타입      | 	필수 | 	설명                                 |
|---------------------|----------|-----|-------------------------------------|
| startUpdateDate     | 	String  | 	O  | 결과 업데이트 조회 시작 시간(yyyy-MM-dd HH:mm)  |
| endUpdateDate       | 	String  | O   | 	결과 업데이트 조회 종료 시간(yyyy-MM-dd HH:mm) |
| alimtalkMessageType | 	String  | X   | 	알림톡 메시지 타입(NORMAL, AUTH)           |
| pageNum             | 	Integer | 	X  | 	페이지 번호(기본: 1)                      |
| pageSize            | 	Integer | 	X  | 	조회 건수(Default: 15, Max: 1000)      |

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messages": [
    {
      "requestId": String,
      "recipientSeq": Integer,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String
    }
  ]
}
```

| 이름                 | 타입      | Not Null | 설명                                                                                                                                                                     |
|--------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header             | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode       | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage    | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful     | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| messages           | List    |    X     | 메시지 리스트                                                                                                                                                                |
| - requestId        | String  |    O     | 요청 ID                                                                                                                                                                  |
| - recipientSeq     | Integer |    O     | 수신자 시퀀스 번호                                                                                                                                                             |
| - requestDate      | String  |    O     | 요청 일시                                                                                                                                                                  |
| - createDate       | String  |    O     | 생성 일시                                                                                                                                                                  |
| - receiveDate      | String  |    X     | 수신 일시                                                                                                                                                                  |
| - resendStatus     | String  |    O     | 대체 발송 상태 코드(RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName | String  |    O     | 대체 발송 상태 코드명                                                                                                                                                           |
| - resendResultCode | String  |    X     | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId  | String  |    X     | 대체 발송 SMS 요청 ID                                                                                                                                                        |
| - messageStatus    | String  |    O     | 요청 상태(COMPLETED -\> 성공, FAILED -\> 실패, CANCEL -\> 취소 )                                                                                                                 |
| - resultCode       | String  |    X     | 수신 결과 코드                                                                                                                                                               |
| - resultCodeName   | String  |    X     | 수신 결과 코드명                                                                                                                                                              |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### 메시지 결과 업데이트 건수 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results/count
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름                  | 	타입      | 	필수 | 	설명                                 |
|---------------------|----------|-----|-------------------------------------|
| startUpdateDate     | 	String  | 	O  | 결과 업데이트 조회 시작 시간(yyyy-MM-dd HH:mm)  |
| endUpdateDate       | 	String  | O   | 	결과 업데이트 조회 종료 시간(yyyy-MM-dd HH:mm) |
| alimtalkMessageType | 	String  | X   | 	알림톡 메시지 타입(NORMAL, AUTH)           |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "totalCount": Integer
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |
| totalCount      | Integer |    O     | 총 건수   |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results/count?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### SMS/LMS 대체 발송 상태 코드

| 이름    | 	설명                                  |
|-------|--------------------------------------|
| RSC01 | 	대체 발송 미대상                           |
| RSC02 | 	대체 발송 대상(발송 결과 실패 시, 대체 발송이 진행됩니다.) |
| RSC03 | 	대체 발송 중                             |
| RSC04 | 	대체 발송 성공                            |
| RSC05 | 	대체 발송 실패                            |

## 대량 발송

### 대량 발송 요청 목록 조회

#### 요청

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	설명       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	고유의 비밀 키 |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.

| 이름               | 	타입               | 최대 길이 | 	필수 | 	설명       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | 요청 ID     |
| startRequestDate | String            | -     | O   | 발송 날짜 시작  |
| endRequestDate   | String            | -     | O   | 발송 날짜 종료  |
| startCreateDate  | 	String           | -     | 	O  | 	등록 날짜 시작 |
| endCreateDate    | 	String           | -     | 	O  | 	등록 날짜 종료 |
| pageNum          | optional, Integer | -     | X   | 페이지 번호    |
| pageSize         | optional, Integer | 1000  | X   | 검색 수      |

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "body": {
    "messages": [
      {
        "requestId": String,
        "requestDate": String,
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "masterStatusCode": String,
        "content": String,
        "fileId": String,
        "autoSendYn": String,
        "statsId": String,
        "createDate": String,
        "createUser": String
      }
    ],
    "totalCount": Integer
  }
}
```

| 이름                  | 타입      | Not Null | 설명                                                                             |
|---------------------|---------|:--------:|--------------------------------------------------------------------------------|
| header              | Object  |    O     | 헤더 영역                                                                          |
| - resultCode        | Integer |    O     | 결과 코드                                                                          |
| - resultMessage     | String  |    O     | 결과 메시지                                                                         |
| - isSuccessful      | Boolean |    O     | 성공 여부                                                                          |
| body                | Object  |    X     | 본문 영역                                                                          |
| - messages          | Object  |    X     | 메시지 리스트                                                                        |
| -- requestId        | String  |    O     | 요청 ID                                                                          |
| -- requestDate      | String  |    O     | 요청 날짜                                                                          |
| -- plusFriendId     | String  |    O     | 플러스 친구 ID                                                                      |
| -- senderKey        | String  |    O     | 발신 키(40자)                                                                      |
| -- masterStatusCode | String  |    O     | 대량 발송 상태 코드(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content          | String  |    X     | 내용                                                                             |
| -- fileId           | String  |    X     | 첨부 파일 ID                                                                       |
| -- templateCode     | String  |    O     | 템플릿 코드(최대 20자)                                                                 |
| -- autoSendYn       | String  |    X     | 자동 발송 여부                                                                       |
| -- statsId          | String  |    X     | 통계 ID                                                                          |
| -- createDate       | String  |    O     | 생성 날짜                                                                          |
| -- createUser       | String  |    X     | 생성 사용자(콘솔에서 발송 시 사용자 UUID로 저장)                                                 |
| - totalCount        | Integer |    X     | 총개수                                                                            |

### 대량 발송 수신자 목록 조회

#### 요청

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appKey    | 	String | 	고유의 앱키 |
| requestId | 	String | 	요청 ID  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	설명       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	고유의 비밀 키 |

| 이름               | 	타입               | 최대 길이 | 	필수 | 	설명       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | 요청 ID     |
| startRequestDate | String            | -     | X   | 발송 날짜 시작  |
| endRequestDate   | String            | -     | X   | 발송 날짜 종료  |
| startCreateDate  | 	String           | -     | 	X  | 	등록 날짜 시작 |
| endCreateDate    | 	String           | -     | 	X  | 	등록 날짜 종료 |
| pageNum          | optional, Integer | -     | X   | 페이지 번호    |
| pageSize         | optional, Integer | 1000  | X   | 검색 수      |

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답

```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
    },
    "body": {
        "recipients": [
            {
                "requestId": String,
                "recipientSeq": Integer,
                "recipientNo": String,
                "requestDate": String,
                "receiveDate": String,
                "messageStatus": String,
                "resultCode": String,
                "resultCodeName": String
            }
        ],
        "totalCount": Integer
    }
}
```

| 이름                | 타입      | Not Null | 설명         |
|-------------------|---------|:--------:|------------|
| header            | Object  |    O     | 헤더 영역      |
| - resultCode      | Integer |    O     | 결과 코드      |
| - resultMessage   | String  |    O     | 결과 메시지     |
| - isSuccessful    | Boolean |    O     | 성공 여부      |
| body              | Object  |    X     | 본문 영역      |
| - messages        | Object  |    X     | 메시지 리스트    |
| -- requestId      | String  |    O     | 요청 ID      |
| -- recipientSeq   | String  |    O     | 수신자 시퀀스 번호 |
| -- recipientNo    | String  |    X     | 수신 번호      |
| -- requestDate    | String  |    O     | 요청 날짜      |
| -- receiveDate    | String  |    X     | 수신 날짜      |
| -- messageStatus  | String  |    O     | 메시지 상태     |
| -- resultCode     | String  |    X     | 결과 코드      |
| -- resultCodeName | String  |    X     | 결과 코드 내용   |
| - totalCount      | Integer |    X     | 총개수        |

### 대량 발송 수신자 조회

#### 요청

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명    |
|--------------|---------|--------|
| appKey       | 	String | 고유의 앱키 |
| requestId    | 	String | 요청 ID  |
| recipientSeq | String  | 수신자 순서 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	설명       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	고유의 비밀 키 |

| 이름               | 	타입               | 최대 길이 | 	필수 | 	설명       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | 요청 ID     |
| startRequestDate | String            | -     | X   | 발송 날짜 시작  |
| endRequestDate   | String            | -     | X   | 발송 날짜 종료  |
| startCreateDate  | 	String           | -     | 	X  | 	등록 날짜 시작 |
| endCreateDate    | 	String           | -     | 	X  | 	등록 날짜 종료 |
| pageNum          | optional, Integer | -     | X   | 페이지 번호    |
| pageSize         | optional, Integer | 1000  | X   | 검색 수      |

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/1" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답

```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
    },
    "body": {
        "requestId": String,
        "recipientSeq": Integer,
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "recipientNo": String,
        "content": String,
        "templateTitle": String,
        "templateSubtitle": String,
        "templateExtra": String,
        "templateAd": String,
        "templateHeader": String,
        "templateItem": {
          "list": [{
            "title": String,
            "description": String
          }],
          "summary": {
            "title": String,
            "description": String
          }
        },
        "templateItemHighlight": {
          "title": String,
          "description": String,
          "imageUrl": String
        },
        "templateRepresentLink": {
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
        },
        "requestDate": String,
        "receiveDate": String,
        "createDate": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "messageStatus": String,
        "resultCode": String,
        "resultCodeName": String,
        "createUser": String,
        "buttons": [
          {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormId": Integer,
            "pluginId": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String,
            "telNumber": String
          }
        ],
        "quickReplies": [
          {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormId": Integer,
            "target": String
            }
        ]
    }
}
```

| 이름                      | 타입      | Not Null | 설명                                                                                                                                                                     |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode            | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage         | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful          | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| body                    | Object  |    X     | 본문 영역                                                                                                                                                                  |
| - requestId             | String  |    O     | 요청 ID                                                                                                                                                                  |
| - recipientSeq          | String  |    O     | 수신자 시퀀스 번호                                                                                                                                                             |
| - plusFriendId          | String  |    O     | 플러스 친구 ID                                                                                                                                                              |
| - senderKey             | String  |    O     | 전송자 ID                                                                                                                                                                 |
| - templateCode          | String  |    O     | 템플릿 코드(최대 20자)                                                                                                                                                         |
| - recipientNo           | String  |    X     | 수신 번호                                                                                                                                                                  |
| - content               | String  |    X     | 내용                                                                                                                                                                     |
| - tempalteTitle         | String  |    X     | 템플릿 제목(최대 50자, Android: 2줄, 23자 이상 말줄임 처리, IOS: 2줄, 27자 이상 말줄임 처리)                                                                                                     |
| - templateSubtitle      | String  |    X     | 템플릿 보조 문구(최대 50자, Android: 18자 이상 말줄임 처리, IOS: 21자 이상 말줄임 처리)                                                                                                          |
| - templateExtra         | String  |    X     | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수)                                                                                                                             |
| - templateAd            | String  |    X     | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구                                                                                                                                            |
| - templateHeader        | String  |    X     | 템플릿 헤더(최대 16자)                                                                                                                                                         |
| - templateItem          | Object  |    X     | 아이템                                                                                                                                                                    |
| -- list                 | List    |    X     | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                 |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description         | String  |    X     | 설명(최대 23자)                                                                                                                                                          |
| -- summary              | Object  |    X     | 아이템 요약 정보                                                                                                                                                              |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description         | String  |    X     | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                          |
| - templateItemHighlight | Object  |    X     | 아이템 하이라이트                                                                                                                                                              |
| --- title               | String  |    X     | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                        |
| --- description         | String  |    X     | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                      |
| --- imageUrl            | String  |    X     | 섬네일 이미지 주소                                                                                                                                                             |
| - templateRepresentLink | Object  |    X     | 대표 링크                                                                                                                                                                  |
| -- linkMo               | String  |    X     | 모바일 웹 링크(최대 500자)                                                                                                                                                      |
| -- linkPc               | String  |    X     | PC 웹 링크(최대 500자)                                                                                                                                                       |
| -- schemeIos            | String  |    X     | iOS 앱 링크(최대 500자)                                                                                                                                                      |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(최대 500자)                                                                                                                                                    |
| - requestDate           | String  |    O     | 요청 날짜                                                                                                                                                                  |
| - receiveDate           | String  |    X     | 수신 날짜                                                                                                                                                                  |
| - createDate            | String  |    O     | 생성 날짜                                                                                                                                                                  |
| - resendStatus          | String  |    O     | 대체 발송 상태 코드(RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName      | String  |    O     | 대체 발송 상태명                                                                                                                                                              |
| - resendResultCode      | String  |    O     | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId       | String  |    X     | 대체 발송 요청 ID                                                                                                                                                            |
| - messageStatus         | String  |    O     | 대량 수신자 발송 상태 코드(READY, COMPLETED, FAILED, CANCEL)                                                                                                                      |
| - resultCode            | String  |    X     | 결과 상태 코드                                                                                                                                                               |
| - resultCodeName        | String  |    X     | 결과 상태명                                                                                                                                                                 |
| - createUser            | String  |    X     | 생성 사용자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                         |
| - buttons               | List    |    X     | 버튼 리스트                                                                                                                                                                 |
| -- ordering             | Integer |    X     | 버튼 순서                                                                                                                                                                  |
| -- type                 | String  |    X     | 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| -- name                 | String  |    X     | 버튼 이름                                                                                                                                                                  |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드)                                                                                                                                              |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드)                                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                              |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                            |
| -- chatExtra            | String  |    X     | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보                                                                                                                                |
| -- chatEvent            | String  |    X     | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명                                                                                                                                           |
| -- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                 |
| -- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                        |
| -- relayId              | String  |    X     | 플러그인 실행 시 X-Kakao-Plugin-Relay-Id 헤더를 통해 전달받을 값                                                                                                                        |
| -- oneClickId           | String  |    X     | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                               |
| -- productId            | String  |    X     | 원클릭 결제 플러그인에서 사용하는 결제 정보                                                                                                                                               |
| -- target               | String  |    X     | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크\<br\>기본 인앱 링크로 발송                                                                                                            |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| - quickReplies          | List    |    X     | 바로연결 리스트(최대 5개)                                                                                                                                                        |
| -- ordering             | Integer |    X     | 바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                |
| -- type                 | String  |    X     | 바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                |
| -- name                 | String  |    X     | 바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                        |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                   |
| -- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                        |
| -- target               | String  |    X     | 웹 링크 타입일 경우, "target":"out" 속성 추가 시 아웃 링크\<br\>기본 인앱 링크로 발송                                                                                                            |

## 템플릿

### 템플릿 카테고리 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "categories": [
    {
      "name": String,
      "subCategories": [
        {
          "code": String,
          "name": String,
          "groupName": String,
          "inclusion": String,
          "exclusion": String
        }
      ]
    }
  ]
}
```

| 이름              | 타입      | Not Null | 설명                       |
|-----------------|---------|:--------:|--------------------------|
| header          | Object  |    O     | 헤더 영역                    |
| - resultCode    | Integer |    O     | 결과 코드                    |
| - resultMessage | String  |    O     | 결과 메시지                   |
| - isSuccessful  | Boolean |    O     | 성공 여부                    |
| categories      | List    |    X     | 카테고리 리스트                 |
| - name          | String  |    X     | 카테고리 이름                  |
| - subCategories | List    |    X     | 서브 카테고리 리스트              |
| -- code         | String  |    X     | 카테고리 코드(템플릿 등록/수정 시, 사용) |
| -- name         | String  |    X     | 카테고리 이름                  |
| -- groupName    | String  |    X     | 카테고리 그룹명                 |
| -- inclusion    | String  |    X     | 카테고리 적용 대상 템플릿 설명        |
| -- exclusion    | String  |    X     | 카테고리 제외 대상 템플릿 설명        |

### 템플릿 등록

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| senderKey | 	String | 	발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "templateCode": String,
  "templateName": String,
  "templateContent": String,
  "templateMessageType": String,
  "templateEmphasizeType": String,
  "templateExtra": String,
  "templateTitle": String,
  "templateSubtitle": String,
  "templateHeader": String,
  "templateItem": {
    "list": [{
      "title": String,
      "description": String
    }],
    "summary": {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight": {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink": {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName": String,
  "templateImageUrl": String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer,
      "pluginId": String,
      "telNumber": String
    }
  ],
  "quickReplies": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer
    }
  ]
}
```

| 이름                    | 	타입      | 	필수 | 	설명                                                                                                                                                                                                                                             |
|-----------------------|----------|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateCode          | 	String  | 	O  | 템플릿 코드(최대 20자)                                                                                                                                                                                                                                  |
| templateName          | 	String  | 	O  | 템플릿명(최대 150자)                                                                                                                                                                                                                                   |
| templateContent       | 	String  | 	O  | 템플릿 본문(최대 1300자)                                                                                                                                                                                                                                |
| templateMessageType   | String   | X   | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 채널 추가형, MI: 복합형, default: BA)                                                                                                                                                                               |
| templateEmphasizeType | String   | X   | 템플릿 강조 표시 타입(NONE: 기본, TEXT: 강조 표시, IMAGE: 이미지형, ITEM_LIST: 아이템리스트형, default: NONE)<br>- TEXT: templateTitle, templateSubtitle 필드 필수<br>- IMAGE: templateImageName, templateImageUrl 필드 필수<br>ITEM_LIST: 이미지, 헤더, 아이템 하이라이트, 아이템 리스트 중 1개 이상 필수 |
| templateExtra         | String   | X   | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수)                                                                                                                                                                                                      |
| tempalteTitle         | String   | X   | 템플릿 제목(최대 50자, Android: 2줄, 23자 이상 말줄임 처리, IOS: 2줄, 27자 이상 말줄임 처리)                                                                                                                                                                              |
| templateSubtitle      | String   | X   | 템플릿 보조 문구(최대 50자, Android: 18자 이상 말줄임 처리, IOS: 21자 이상 말줄임 처리)                                                                                                                                                                                   |
| templateHeader        | String   | X   | 템플릿 헤더(최대 16자)                                                                                                                                                                                                                                  |
| templateItem          | Object   | X   | 아이템                                                                                                                                                                                                                                             |
| - list                | List     | X   | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                                                                                          |
| -- title              | String   | X   | 타이틀(최대 6자)                                                                                                                                                                                                                                      |
| -- description        | String   | X   | 설명(최대 23자)                                                                                                                                                                                                                                   |
| - summary             | Object   | X   | 아이템 요약 정보                                                                                                                                                                                                                                       |
| -- title              | String   | X   | 타이틀(최대 6자)                                                                                                                                                                                                                                      |
| -- description        | String   | X   | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                                                                                                   |
| templateItemHighlight | Object   | X   | 아이템 하이라이트                                                                                                                                                                                                                                       |
| - title               | String   | X   | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                                                                                                 |
| - description         | String   | X   | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                                                                                               |
| - imageUrl            | String   | X   | 섬네일 이미지 주소                                                                                                                                                                                                                                      |
| templateRepresentLink | Object   | X   | 대표 링크                                                                                                                                                                                                                                           |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(최대 500자)                                                                                                                                                                                                                              |
| - linkPc              | String   | 	X  | PC 웹 링크(최대 500자)                                                                                                                                                                                                                                |
| - schemeIos           | String   | X   | 	iOS 앱 링크(최대 500자)                                                                                                                                                                                                                              |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(최대 500자)                                                                                                                                                                                                                            |
| templateImageName     | String   | 	X  | 이미지명(업로드한 파일명)                                                                                                                                                                                                                                  |
| templateImageUrl      | String   | 	X  | 이미지 URL                                                                                                                                                                                                                                         |
| securityFlag          | Boolean  | X   | 보안 템플릿 여부<br>OTP등 보안 메시지 일 경우 설정<br>발신 당시의 메인 디바이스를 제외한 모든 디바이스에 메시지 텍스트 미노출(default: false)                                                                                                                                                    |
| categoryCode          | String   | X   | 템플릿 카테고리 코드(템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 기타일 경우, 최하위 우선순위로 심사                                                                                                                                                                   |
| buttons               | 	List    | 	X  | 버튼 리스트(최대 5개)                                                                                                                                                                                                                                   |
| - ordering            | 	Integer | 	X  | 버튼 순서(1~5)                                                                                                                                                                                                                                      |
| - type                | String   | 	X  | 	버튼 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기)                                                                      |
| - name                | String   | 	X  | 	버튼 이름(버튼이 있는 경우 필수, 최대 14자)                                                                                                                                                                                                                    |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                                                                                         |
| - pluginId            | 	String  | 	X  | 	플러그인 ID(최대 24자)                                                                                                                                                                                                                                |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| quickReplies          | 	List    | 	X  | 바로연결 리스트(최대 5개)                                                                                                                                                                                                                                 |
| - ordering            | 	Integer | 	X  | 	바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                                                                                        |
| - type                | String   | 	X  | 	바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                                                                                        |
| - name                | String   | 	X  | 	바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                                                                                                |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                                                                                         |

* 채널 추가형(AD) 또는 복합형(MI) 메시지 유형 템플릿 등록 시 templateAd 값이 고정됩니다.
* 채널 추가형(AD) 또는 복합형(MI) 메시지 유형 템플릿 등록 시 채널 추가(AC) 버튼이 첫 번째 순서에 위치해야 합니다.
* 채널 추가(AC) 버튼의 버튼명은 "채널 추가"로 고정하여 등록해야 합니다.

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 수정

#### 요청

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "templateName": String,
  "templateContent": String,
  "templateMessageType": String,
  "templateEmphasizeType": String,
  "templateExtra": String,
  "templateTitle": String,
  "templateSubtitle": String,
  "templateHeader": String,
  "templateItem": {
    "list": [{
      "title": String,
      "description": String
    }],
    "summary": {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight": {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink": {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName": String,
  "templateImageUrl": String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer,
      "pluginId": String,
      "telNumber": String
    }
  ],
  "quickReplies": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer
    }
  ]
}
```

| 이름                    | 	타입      | 	필수 | 	설명                                                                                                                                                                        |
|-----------------------|----------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName          | 	String  | 	O  | 템플릿명(최대 150자)                                                                                                                                                              |
| templateContent       | 	String  | 	O  | 템플릿 본문(최대 1300자)                                                                                                                                                           |
| templateMessageType   | String   | X   | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 채널 추가형, MI: 복합형)                                                                                                                       |
| templateEmphasizeType | String   | X   | 템플릿 강조 표시 타입(NONE: 기본, TEXT: 강조 표시, IMAGE: 이미지형, default: NONE)<br>- TEXT: templateTitle, templateSubtitle 필드 필수<br>- IMAGE: templateImageName, templateImageUrl 필드 필수     |
| templateExtra         | String   | X   | 템플릿 부가 정보(템플릿 메시지 유형이 [부가 정보형/복합형]일 경우 필수)                                                                                                                                 |
| tempalteTitle         | String   | X   | 템플릿 제목(최대 50자, Android: 2줄, 23자 이상 말줄임 처리, IOS: 2줄, 27자 이상 말줄임 처리)                                                                                                         |
| templateSubtitle      | String   | X   | 템플릿 보조 문구(최대 50자, Android: 18자 이상 말줄임 처리, IOS: 21자 이상 말줄임 처리)                                                                                                              |
| templateHeader        | String   | X   | 템플릿 헤더(최대 16자)                                                                                                                                                             |
| templateItem          | Object   | X   | 아이템                                                                                                                                                                        |
| - list                | List     | X   | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                     |
| -- title              | String   | X   | 타이틀(최대 6자)                                                                                                                                                                 |
| -- description        | String   | X   | 설명(최대 23자)                                                                                                                                                              |
| - summary             | Object   | X   | 아이템 요약 정보                                                                                                                                                                  |
| -- title              | String   | X   | 타이틀(최대 6자)                                                                                                                                                                 |
| -- description        | String   | X   | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                              |
| templateItemHighlight | Object   | X   | 아이템 하이라이트                                                                                                                                                                  |
| - title               | String   | X   | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                            |
| - description         | String   | X   | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                          |
| - imageUrl            | String   | X   | 섬네일 이미지 주소                                                                                                                                                                 |
| templateRepresentLink | Object   | X   | 대표 링크                                                                                                                                                                      |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(최대 500자)                                                                                                                                                         |
| - linkPc              | String   | 	X  | PC 웹 링크(최대 500자)                                                                                                                                                           |
| - schemeIos           | String   | X   | 	iOS 앱 링크(최대 500자)                                                                                                                                                         |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(최대 500자)                                                                                                                                                       |
| templateImageName     | String   | 	X  | 이미지명(업로드한 파일명)                                                                                                                                                             |
| templateImageUrl      | String   | 	X  | 이미지 URL                                                                                                                                                                    |
| securityFlag          | Boolean  | X   | 보안 템플릿 여부<br>OTP등 보안 메시지 일 경우 설정<br>발신 당시의 메인 디바이스를 제외한 모든 디바이스에 메시지 텍스트 미노출(default: false)                                                                               |
| categoryCode          | String   | X   | 템플릿 카테고리 코드(템플릿 카테고리 조회 API 참고, default: 999999)<br>카테고리 기타일 경우, 최하위 우선순위로 심사                                                                                              |
| buttons               | 	List    | 	X  | 버튼 리스트(최대 5개)                                                                                                                                                              |
| - ordering            | 	Integer | 	X  | 버튼 순서(1~5)                                                                                                                                                                 |
| - type                | String   | 	X  | 	버튼 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| - name                | String   | 	X  | 	버튼 이름(버튼이 있는 경우 필수, 최대 14자)                                                                                                                                               |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| - linkPc              | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                          |
| - schemeIos           | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                      |
| - bizFormId           | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                    |
| - pluginId            | 	String  | 	X  | 	플러그인 ID(최대 24자)                                                                                                                                                           |
| -- telNumber           | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| quickReplies          | 	List    | 	X  | 바로연결 리스트(최대 5개)                                                                                                                                                            |
| - ordering            | 	Integer | 	X  | 	바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                   |
| - type                | String   | 	X  | 	바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                   |
| - name                | String   | 	X  | 	바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                           |
| - linkMo              | String   | 	X  | 	모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| - linkPc              | String   | 	X  | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                          |
| - schemeIos           | String   | X   | 	iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                        |
| - schemeAndroid       | String   | X   | 	안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                      |
| - bizFormId           | 	Integer | 	X  | 	비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                    |

* 채널 추가형(AD)"과 "복합형(MI)" 메시지 유형 템플릿 수정 시, templateAd 값이 고정됩니다.
* 채널 추가형(AD)과 복합형(MI) 메시지 유형 템플릿 수정 시, 채널 추가(AC) 버튼이 첫 번째 순서에 위치해야 합니다.
* 채널 추가(AC) 버튼의 버튼명은 "채널 추가"로 고정하여, 수정해야 합니다.

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 삭제

#### 요청

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

* 승인된 템플릿 삭제 시, NHN Cloud 내에서만 삭제됩니다.(3일간 미발송한 템플릿만 삭제 가능)
* 승인된 템플릿의 경우, 카카오톡 비즈메시지의 제약 때문에 카카오 내부 데이터는 삭제할 수 없습니다.
* 카카오에 남아 있는 템플릿은 1년 미사용 시 휴면 처리되고, 휴면 상태가 1년간 지속되면 삭제 처리됩니다.(카카오에서 템플릿이 휴면 전환되거나 삭제되면 담당자에게 알림이 발송됩니다.)

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 문의하기

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "comment": String
}
```

| 이름      | 	타입     | 	필수 | 	설명   |
|---------|---------|-----|-------|
| comment | 	String | 	O  | 문의 내용 |

* 반려 상태의 템플릿에 문의를 남길 경우, 검수 중(REQ) 상태로 변경됩니다.

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 파일 첨부하여 템플릿 문의하기

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "comment": String,
  "attachments": File
}
```

| 이름          | 	타입        | 	필수 | 	설명                                                                                                                                        |
|-------------|------------|-----|--------------------------------------------------------------------------------------------------------------------------------------------|
| comment     | 	String    | 	O  | 문의 내용                                                                                                                                      |
| attachments | List<File> | X   | 첨부 파일 목록(최대 10개)<br>- 지원 확장자 ( .png, .jpg, .jpeg, .gif, .pdf, .hwp, .doc, .docx )<br>- 각 개별 파일의 최대 크기 50mb<br>- 전체 업로드 가능한 파일의 최대 크기 100mb |

* 반려 상태의 템플릿에 문의를 남길 경우, 검수 중(REQ) 상태로 변경됩니다.

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 채널 추가형으로 변경

#### 요청

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appKey}/senders/{senderKey}/templates/{templateCode}/convert-add-channel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

* 그룹 발신 프로필에 등록된 템플릿은 채널 추가형으로 전환할 수 없습니다.

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 단건 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | String  | 템플릿 코드  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

| 템플릿 상태 코드 | 설명   |
|-----------|------|
| TSC01     | 요청   |
| TSC02     | 검수 중 |
| TSC03     | 승인   |
| TSC04     | 반려   |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templates": {
      "plusFriendId": String,
      "senderKey": String,
      "plusFriendType": String,
      "templateCode": String,
      "kakaoTemplateCode": String,
      "templateName": String,
      "templateMessageType": String,
      "templateEmphasizeType": String,
      "templateContent": String,
      "templateExtra": String,
      "templateAd": String,
      "templateTitle": String,
      "templateSubtitle": String,
      "templateHeader": String,
      "templateItem": {
        "list": [{
          "title": String,
          "description": String
        }],
        "summary": {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight": {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink": {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "templateImageName": String,
      "templateImageUrl": String,
      "buttons": [
        {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer,
            "pluginId": String,
            "telNumber": String
        }
      ],
      "quickReplies": [
        {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer
        }
      ],
      "comments": [
          {
              "id": Integer,
              "content": String,
              "userName": String,
              "createdAt": String,
              "attachment": [{
                "originalFileName": String,
                "filePath": String
              }],
              "status": String
          }  
      ],
      "status": String,
      "statusName": String,
      "securityFlag": Boolean,
      "categoryCode": String,
      "block": Boolean,
      "dormant": Boolean,
      "createDate": String,
      "updateDate": String
  }
}
```

---

| 이름                      | 타입      | Not Null | 설명                                                                                                                                                                               |
|-------------------------|---------|:--------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | 헤더 영역                                                                                                                                                                            |
| - resultCode            | Integer |    O     | 결과 코드                                                                                                                                                                            |
| - resultMessage         | String  |    O     | 결과 메시지                                                                                                                                                                           |
| - isSuccessful          | Boolean |    O     | 성공 여부                                                                                                                                                                            |
| templates               | Object    |    X     | 템플릿 리스트                                                                                                                                                                          |
| - plusFriendId          | String  |    O     | 카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명                                                                                                                                                     |
| - senderKey             | String  |    O     | 발신 키                                                                                                                                                                             |
| - plusFriendType        | String  |    O     | 플러스친구 타입(NORMAL, GROUP)                                                                                                                                                          |
| - templateCode          | String  |    O     | 템플릿 코드                                                                                                                                                                           |
| - kakaoTemplateCode     | String  |    O     | 원본 템플릿 코드                                                                                                                                                                        |
| - templateName          | String  |    O     | 템플릿명                                                                                                                                                                             |
| - templateMessageType   | String  |    X     | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 채널 추가형, MI: 복합형)                                                                                                                             |
| - templateEmphasizeType | String  |    X     | 템플릿 강조 표시 타입(NONE: 기본, TEXT: 강조 표시, IMAGE: 이미지형, ITEM_LIST: 아이템리스트형)                                                                                                             |
| - templateContent       | String  |    X     | 템플릿 본문                                                                                                                                                                           |
| - templateExtra         | String  |    X     | 템플릿 부가 정보                                                                                                                                                                        |
| - templateAd            | String  |    X     | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구                                                                                                                                                      |
| - tempalteTitle         | String  |    X     | 템플릿 제목                                                                                                                                                                           |
| - templateSubtitle      | String  |    X     | 템플릿 보조 문구                                                                                                                                                                        |
| - templateHeader        | String  |    X     | 템플릿 헤더(최대 16자)                                                                                                                                                                   |
| - templateItem          | Object  |    X     | 아이템                                                                                                                                                                              |
| -- list                 | List    |    X     | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                           |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                                       |
| --- description         | String  |    X     | 설명(최대 23자)                                                                                                                                                                    |
| -- summary              | Object  |    X     | 아이템 요약 정보                                                                                                                                                                        |
| --- title               | String  |    X     | 타이틀(최대 6자)                                                                                                                                                                       |
| --- description         | String  |    X     | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                                    |
| - templateItemHighlight | Object  |    X     | 아이템 하이라이트                                                                                                                                                                        |
| -- title                | String  |    X     | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                                  |
| -- description          | String  |    X     | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                                |
| -- imageUrl             | String  |    X     | 섬네일 이미지 주소                                                                                                                                                                       |
| - templateRepresentLink | Object  |    X     | 대표 링크                                                                                                                                                                            |
| -- linkMo               | String  |    X     | 모바일 웹 링크(최대 500자)                                                                                                                                                                |
| -- linkPc               | String  |    X     | PC 웹 링크(최대 500자)                                                                                                                                                                 |
| -- schemeIos            | String  |    X     | iOS 앱 링크(최대 500자)                                                                                                                                                                |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(최대 500자)                                                                                                                                                              |
| - templateImageName     | String  |    X     | 이미지명(업로드한 파일명)                                                                                                                                                                   |
| - templateImageUrl      | String  |    X     | 이미지 URL                                                                                                                                                                          |
| - buttons               | List    |    X     | 버튼 리스트                                                                                                                                                                           |
| -- ordering             | Integer |    X     | 버튼 순서(1~5)                                                                                                                                                                       |
| -- type                 | String  |    X     | 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| -- name                 | String  |    X     | 버튼 이름                                                                                                                                                                            |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드)                                                                                                                                                        |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드)                                                                                                                                                         |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                                        |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                                      |
| -- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                           |
| -- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                                  |
| -- telNumber            | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                                                                                                       |
| - quickReplies          | List    |    X     | 바로연결 리스트(최대 5개)                                                                                                                                                                  |
| -- ordering             | Integer |    X     | 바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                          |
| -- type                 | String  |    X     | 바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                          |
| -- name                 | String  |    X     | 바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                                  |
| -- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                               |
| -- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                                |
| -- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                               |
| -- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                             |
| -- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                           |
| - comments              | List    |    X     | 검수 결과                                                                                                                                                                            |
| -- id                   | Integer |    X     | 문의 아이디                                                                                                                                                                           |
| -- content              | String  |    X     | 문의 내용                                                                                                                                                                            |
| -- userName             | String  |    X     | 작성자                                                                                                                                                                              |
| -- createAt             | String  |    O     | 등록 날짜                                                                                                                                                                            |
| -- attachment           | List    |    X     | 첨부 파일                                                                                                                                                                            |
| --- originalFileName    | String  |    X     | 첨부 파일명                                                                                                                                                                           |
| --- filePath            | String  |    X     | 첨부 파일 경로                                                                                                                                                                         |
| -- status               | String  |    X     | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변, REQ: 검수 중)                                                                                                                             |
| - status                | String  |    O     | 템플릿 상태                                                                                                                                                                           |
| - statusName            | String  |    X     | 템플릿 상태명                                                                                                                                                                          |
| - securityFlag          | Boolean |    X     | 보안 템플릿 여부                                                                                                                                                                        |
| - categoryCode          | String  |    X     | 템플릿 카테고리 코드                                                                                                                                                                      |
| - block                 | Boolean |    X     | 차단 여부                                                                                                                                                                            |
| - dormant               | Boolean |    X     | 휴면 여부                                                                                                                                                                            |
| - createDate            | String  |    O     | 생성일자                                                                                                                                                                             |
| - updateDate            | String  |    X     | 수정일자                                                                                                                                                                             |


### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| senderKey | 	String | 	발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름             | 	타입      | 	필수 | 	설명                            |
|----------------|----------|-----|--------------------------------|
| templateCode   | 	String  | 	X  | 	템플릿 코드                        |
| templateName   | 	String  | 	X  | 	템플릿 이름                        |
| templateStatus | String   | 	X  | 템플릿 상태 코드                      |
| pageNum        | 	Integer | 	X  | 	페이지 번호(Default: 1)            |
| pageSize       | 	Integer | 	X  | 	조회 건수(Default: 15, Max: 1000) |

| 템플릿 상태 코드 | 설명   |
|-----------|------|
| TSC01     | 요청   |
| TSC02     | 검수 중 |
| TSC03     | 승인   |
| TSC04     | 반려   |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates?templateStatus={템플릿 상태 코드}"
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templateListResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "kakaoTemplateCode": String,
              "templateName": String,
              "templateMessageType": String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra": String,
              "templateAd": String,
              "templateTitle": String,
              "templateSubtitle": String,
              "templateHeader": String,
              "templateItem": {
                "list": [{
                  "title": String,
                  "description": String
                }],
                "summary": {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight": {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink": {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName": String,
              "templateImageUrl": String,
              "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer,
                    "pluginId": String,
                    "telNumber": String
                }
              ],
              "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer
                }
              ],
              "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                  }  
              ],
              "status": String,
              "statusName": String,
              "securityFlag": Boolean,
              "categoryCode": String,
              "createDate": String,
              "updateDate": String
          }
      ],
      "totalCount": Integer
  }
}
```

---

| 이름                       | 타입      | Not Null | 설명                                                                                                                                                                     |
|--------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                   | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode             | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage          | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful           | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| templateListResponse     | Object  |    X     | 본문 영역                                                                                                                                                                  |
| - templates              | List    |    X     | 템플릿 리스트                                                                                                                                                                |
| -- plusFriendId          | String  |    O     | 카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명                                                                                                                                           |
| -- senderKey             | String  |    O     | 발신 키                                                                                                                                                                   |
| -- plusFriendType        | String  |    O     | 플러스친구 타입(NORMAL, GROUP)                                                                                                                                                |
| -- templateCode          | String  |    O     | 템플릿 코드                                                                                                                                                                 |
| -- kakaoTemplateCode     | String  |    O     | 원본 템플릿 코드                                                                                                                                                              |
| -- templateName          | String  |    O     | 템플릿명                                                                                                                                                                   |
| -- templateMessageType   | String  |    X     | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 채널 추가형, MI: 복합형)                                                                                                                   |
| -- templateEmphasizeType | String  |    X     | 템플릿 강조 표시 타입(NONE: 기본, TEXT: 강조 표시, IMAGE: 이미지형, ITEM_LIST: 아이템리스트형)                                                                                                   |
| -- templateContent       | String  |    X     | 템플릿 본문                                                                                                                                                                 |
| -- templateExtra         | String  |    X     | 템플릿 부가 정보                                                                                                                                                              |
| -- templateAd            | String  |    X     | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구                                                                                                                                            |
| -- tempalteTitle         | String  |    X     | 템플릿 제목                                                                                                                                                                 |
| -- templateSubtitle      | String  |    X     | 템플릿 보조 문구                                                                                                                                                              |
| - templateHeader         | String  |    X     | 템플릿 헤더(최대 16자)                                                                                                                                                         |
| - templateItem           | Object  |    X     | 아이템                                                                                                                                                                    |
| -- list                  | List    |    X     | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                 |
| --- title                | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description          | String  |    X     | 설명(최대 23자)                                                                                                                                                          |
| -- summary               | Object  |    X     | 아이템 요약 정보                                                                                                                                                              |
| --- title                | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description          | String  |    X     | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                          |
| - templateItemHighlight  | Object  |    X     | 아이템 하이라이트                                                                                                                                                              |
| -- title                 | String  |    X     | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                        |
| -- description           | String  |    X     | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                      |
| -- imageUrl              | String  |    X     | 섬네일 이미지 주소                                                                                                                                                             |
| - templateRepresentLink  | Object  |    X     | 대표 링크                                                                                                                                                                  |
| -- linkMo                | String  |    X     | 모바일 웹 링크(최대 500자)                                                                                                                                                      |
| -- linkPc                | String  |    X     | PC 웹 링크(최대 500자)                                                                                                                                                       |
| -- schemeIos             | String  |    X     | iOS 앱 링크(최대 500자)                                                                                                                                                      |
| -- schemeAndroid         | String  |    X     | 안드로이드 앱 링크(최대 500자)                                                                                                                                                    |
| -- templateImageName     | String  |    X     | 이미지명(업로드한 파일명)                                                                                                                                                         |
| -- templateImageUrl      | String  |    X     | 이미지 URL                                                                                                                                                                |
| -- buttons               | List    |    X     | 버튼 리스트                                                                                                                                                                 |
| --- ordering             | Integer |    X     | 버튼 순서(1~5)                                                                                                                                                             |
| --- type                 | String  |    X     | 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| --- name                 | String  |    X     | 버튼 이름                                                                                                                                                                  |
| --- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드)                                                                                                                                              |
| --- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드)                                                                                                                                               |
| --- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                              |
| --- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                            |
| --- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                 |
| --- pluginId             | String  |    X     | 플러그인 ID(최대 24자)                                                                                                                                                        |
| --- telNumber            | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| -- quickReplies          | List    |    X     | 바로연결 리스트(최대 5개)                                                                                                                                                        |
| --- ordering             | Integer |    X     | 바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                |
| --- type                 | String  |    X     | 바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                |
| --- name                 | String  |    X     | 바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                        |
| --- linkMo               | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| --- linkPc               | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                      |
| --- schemeIos            | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| --- schemeAndroid        | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                   |
| --- bizFormId            | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                 |
| -- comments              | List    |    X     | 검수 결과                                                                                                                                                                  |
| --- id                   | Integer |    X     | 문의 아이디                                                                                                                                                                 |
| --- content              | String  |    X     | 문의 내용                                                                                                                                                                  |
| --- userName             | String  |    X     | 작성자                                                                                                                                                                    |
| --- createAt             | String  |    O     | 등록 날짜                                                                                                                                                                  |
| --- attachment           | List    |    X     | 첨부 파일                                                                                                                                                                  |
| ---- originalFileName    | String  |    X     | 첨부 파일명                                                                                                                                                                 |
| ---- filePath            | String  |    X     | 첨부 파일 경로                                                                                                                                                               |
| --- status               | String  |    X     | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변, REQ: 검수 중)                                                                                                                   |
| -- status                | String  |    O     | 템플릿 상태                                                                                                                                                                 |
| -- statusName            | String  |    X     | 템플릿 상태명                                                                                                                                                                |
| -- securityFlag          | Boolean |    X     | 보안 템플릿 여부                                                                                                                                                              |
| -- categoryCode          | String  |    X     | 템플릿 카테고리 코드                                                                                                                                                            |
| -- createDate            | String  |    O     | 생성일자                                                                                                                                                                   |
| -- updateDate            | String  |    X     | 수정일자                                                                                                                                                                   |
| - totalCount             | Integer |    X     | 총개수                                                                                                                                                                    |

### 템플릿 수정 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 	타입     | 	설명     |
|--------------|---------|---------|
| appkey       | 	String | 	고유의 앱키 |
| senderKey    | 	String | 	발신 키   |
| templateCode | 	String | 	템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templateModificationsResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateMessageType": String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra": String,
              "templateAd": String,
              "templateTitle": String,
              "templateSubtitle": String,
              "templateHeader": String,
              "templateItem": {
                "list": [{
                  "title": String,
                  "description": String
                }],
                "summary": {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight": {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink": {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName": String,
              "templateImageUrl": String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "telNumber": String
                }
              ],
              "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer
                }
              ],
              "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                  }  
              ],
              "status": String,
              "statusName": String,
              "securityFlag": Boolean,
              "categoryCode": String,
              "activated": boolean,
              "createDate": String,
              "updateDate": String
          }
      ],
      "totalCount": Integer
  }
}
```

---

| 이름                            | 타입      | Not Null | 설명                                                                                                                                                                     |
|-------------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                        | Object  |    O     | 헤더 영역                                                                                                                                                                  |
| - resultCode                  | Integer |    O     | 결과 코드                                                                                                                                                                  |
| - resultMessage               | String  |    O     | 결과 메시지                                                                                                                                                                 |
| - isSuccessful                | Boolean |    O     | 성공 여부                                                                                                                                                                  |
| templateModificationsResponse | Object  |    X     | 본문 영역                                                                                                                                                                  |
| - templates                   | List    |    X     | 템플릿 리스트                                                                                                                                                                |
| -- plusFriendId               | String  |    O     | 카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명                                                                                                                                           |
| -- senderKey                  | String  |    O     | 발신 키                                                                                                                                                                   |
| -- plusFriendType             | String  |    O     | 플러스친구 타입(NORMAL, GROUP)                                                                                                                                                |
| -- templateCode               | String  |    O     | 템플릿 코드                                                                                                                                                                 |
| -- templateName               | String  |    O     | 템플릿명                                                                                                                                                                   |
| -- templateMessageType        | String  |    X     | 템플릿 메시지 유형(BA: 기본형, EX: 부가 정보형, AD: 채널 추가형, MI: 복합형)                                                                                                                   |
| -- templateEmphasizeType      | String  |    X     | 템플릿 강조 표시 타입(NONE: 기본, TEXT: 강조 표시, IMAGE: 이미지형, )                                                                                                                     |
| -- templateContent            | String  |    O     | 템플릿 본문                                                                                                                                                                 |
| -- templateExtra              | String  |    X     | 템플릿 부가 정보                                                                                                                                                              |
| -- templateAd                 | String  |    X     | 템플릿 내 수신 동의 요청 또는 간단한 광고 문구                                                                                                                                            |
| -- tempalteTitle              | String  |    X     | 템플릿 제목                                                                                                                                                                 |
| -- templateSubtitle           | String  |    X     | 템플릿 보조 문구                                                                                                                                                              |
| - templateHeader              | String  |    X     | 템플릿 헤더(최대 16자)                                                                                                                                                         |
| - templateItem                | Object  |    X     | 아이템                                                                                                                                                                    |
| -- list                       | List    |    X     | 아이템 리스트(최소 2개, 최대 10개)                                                                                                                                                 |
| --- title                     | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description               | String  |    X     | 설명(최대 23자)                                                                                                                                                          |
| -- summary                    | Object  |    X     | 아이템 요약 정보                                                                                                                                                              |
| --- title                     | String  |    X     | 타이틀(최대 6자)                                                                                                                                                             |
| --- description               | String  |    X     | 설명(변수 및 화폐 단위, 숫자, 쉼표, 마침표만 사용 가능, 최대 14자)                                                                                                                          |
| - templateItemHighlight       | Object  |    X     | 아이템 하이라이트                                                                                                                                                              |
| -- title                      | String  |    X     | 타이틀(최대 30자, 섬네일 이미지가 있을 경우 21자)                                                                                                                                        |
| -- description                | String  |    X     | 설명(최대 19자, 섬네일 이미지가 있을 경우 13자)                                                                                                                                      |
| -- imageUrl                   | String  |    X     | 섬네일 이미지 주소                                                                                                                                                             |
| - templateRepresentLink       | Object  |    X     | 대표 링크                                                                                                                                                                  |
| -- linkMo                     | String  |    X     | 모바일 웹 링크(최대 500자)                                                                                                                                                      |
| -- linkPc                     | String  |    X     | PC 웹 링크(최대 500자)                                                                                                                                                       |
| -- schemeIos                  | String  |    X     | iOS 앱 링크(최대 500자)                                                                                                                                                      |
| -- schemeAndroid              | String  |    X     | 안드로이드 앱 링크(최대 500자)                                                                                                                                                    |
| -- templateImageName          | String  |    X     | 이미지명(업로드한 파일명)                                                                                                                                                         |
| -- templateImageUrl           | String  |    X     | 이미지 URL                                                                                                                                                                |
| -- buttons                    | List    |    X     | 버튼 리스트                                                                                                                                                                 |
| --- ordering                  | Integer |    X     | 버튼 순서(1~5)                                                                                                                                                             |
| --- type                      | String  |    X     | 버튼 타입(WL: 웹 링크, AL: 앱 링크, DS: 배송 조회, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 봇 전환, AC: 채널 추가, BF: 비즈니스폼, P1: 이미지 보안 전송 플러그인 ID, P2: 개인정보 이용 플러그인 ID, P3: 원클릭 결제 플러그인 ID, TN: 전화하기) |
| --- name                      | String  |    X     | 버튼 이름                                                                                                                                                                  |
| --- linkMo                    | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드)                                                                                                                                              |
| --- linkPc                    | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드)                                                                                                                                               |
| --- schemeIos                 | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                              |
| --- schemeAndroid             | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드)                                                                                                                                            |
| --- telNumber                 | 	String  | 	X  | TN(전화하기) 타입 버튼 시, 전달할 전화번호                                                                    |
| -- quickReplies               | List    |    X     | 바로연결 리스트(최대 5개)                                                                                                                                                        |
| --- ordering                  | Integer |    X     | 바로연결 순서(바로연결이 있는 경우 필수)                                                                                                                                                |
| --- type                      | String  |    X     | 바로연결 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, BC: 상담톡 전환, BT: 봇 전환, BF: 비즈니스폼)                                                                                                |
| --- name                      | String  |    X     | 바로연결 이름(바로연결이 있는 경우 필수, 최대 14자)                                                                                                                                        |
| --- linkMo                    | String  |    X     | 모바일 웹 링크(WL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| --- linkPc                    | String  |    X     | PC 웹 링크(WL 타입일 경우 선택 필드, 최대 500자)                                                                                                                                      |
| --- schemeIos                 | String  |    X     | iOS 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                     |
| --- schemeAndroid             | String  |    X     | 안드로이드 앱 링크(AL 타입일 경우 필수 필드, 최대 500자)                                                                                                                                   |
| --- bizFormId                 | Integer |    X     | 비즈니스폼 ID(BF 타입일 경우 필수)                                                                                                                                                 |
| -- comments                   | List    |    X     | 검수 결과                                                                                                                                                                  |
| --- id                        | Integer |    X     | 문의 아이디                                                                                                                                                                 |
| --- content                   | String  |    X     | 문의 내용                                                                                                                                                                  |
| --- userName                  | String  |    X     | 작성자                                                                                                                                                                    |
| --- createAt                  | String  |    O     | 등록 날짜                                                                                                                                                                  |
| --- attachment                | List    |    X     | 첨부 파일                                                                                                                                                                  |
| ---- originalFileName         | String  |    X     | 첨부 파일명                                                                                                                                                                 |
| ---- filePath                 | String  |    X     | 첨부 파일 경로                                                                                                                                                               |
| --- status                    | String  |    X     | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변, REQ: 검수 중)                                                                                                                   |
| -- status                     | String  |    O     | 템플릿 상태                                                                                                                                                                 |
| -- statusName                 | String  |    O     | 템플릿 상태명                                                                                                                                                                |
| -- securityFlag               | Boolean |    X     | 보안 템플릿 여부                                                                                                                                                              |
| -- categoryCode               | String  |    X     | 템플릿 카테고리 코드                                                                                                                                                            |
| -- activated                  | Boolean |    X     | 활성화 여부                                                                                                                                                                 |
| -- createDate                 | String  |    O     | 생성일자                                                                                                                                                                   |
| -- updateDate                 | String  |    X     | 수정일자                                                                                                                                                                   |
| - totalCount                  | Integer |    X     | 총개수                                                                                                                                                                    |

### 템플릿 이미지 등록

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request parameter]

| 이름   | 	타입   | 	필수 | 	설명     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	이미지 파일 |

[예시]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| 이름                  | 타입      | Not Null | 설명             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | 헤더 영역          |
| - resultCode        | Integer |    O     | 결과 코드          |
| - resultMessage     | String  |    O     | 결과 메시지         |
| - isSuccessful      | Boolean |    O     | 성공 여부          |
| templateImage       | Object  |    X     | 본문 영역          |
| - templateImageName | String  |    X     | 이미지명(업로드한 파일명) |
| - templateImageUrl  | String  |    X     | 이미지 URL        |

### 템플릿 아이템 하이라이트 이미지 등록

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight
Content-Type: multipart/form-data
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request parameter]

| 이름   | 	타입   | 	필수 | 	설명     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	이미지 파일 |

[예시]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight" -F "file=@alimtalk-template-image.jpeg"
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| 이름                  | 타입      | Not Null | 설명             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | 헤더 영역          |
| - resultCode        | Integer |    O     | 결과 코드          |
| - resultMessage     | String  |    O     | 결과 메시지         |
| - isSuccessful      | Boolean |    O     | 성공 여부          |
| templateImage       | Object  |    X     | 본문 영역          |
| - templateImageName | String  |    X     | 이미지명(업로드한 파일명) |
| - templateImageUrl  | String  |    X     | 이미지 URL        |

### 템플릿 플러그인 등록

#### 요청

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| senderKey | 	String | 	발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "pluginType": String,
  "pluginId": String,
  "callbackUrl": String
}
```

| 이름          | 	타입     | 	필수 | 	설명                                                        |
|-------------|---------|-----|------------------------------------------------------------|
| pluginType  | 	String | 	O  | 플러그인 타입(SECURE_IMAGE: 보안 이미지 전송, ONE_TIME_PROFILE: 개인정보 이용) |
| pluginId    | 	String | 	O  | 플러그인 아이디                                                   |
| callbackUrl | 	String | 	O  | 플러그인 버튼 클릭 시, 수신받을 콜백 URL                                  |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 플러그인 수정

#### 요청

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명       |
|-----------|---------|-----------|
| appkey    | 	String | 	고유의 앱키   |
| senderKey | 	String | 	발신 키     |
| pluginId  | 	String | 	플러그인 아이디 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request Body]

```
{
  "pluginType": String,
  "callbackUrl": String
}
```

| 이름          | 	타입     | 	필수 | 	설명                                                        |
|-------------|---------|-----|------------------------------------------------------------|
| pluginType  | 	String | 	O  | 플러그인 타입(SECURE_IMAGE: 보안 이미지 전송, ONE_TIME_PROFILE: 개인정보 이용) |
| callbackUrl | 	String | 	O  | 플러그인 버튼 클릭 시, 수신받을 콜백 URL                                  |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 플러그인 삭제

#### 요청

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명       |
|-----------|---------|-----------|
| appkey    | 	String | 	고유의 앱키   |
| senderKey | 	String | 	발신 키     |
| pluginId  | 	String | 	플러그인 아이디 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 템플릿 플러그인 조회

#### 요청

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| senderKey | 	String | 	발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "plugins": [
    {
      "pluginId": String,
      "pluginType": String,
      "pluginTypeName": String,
      "callbackUrl": String,
      "modifiable": boolean,
      "deletable": boolean
    }
  
  ]
}
```

| 이름               | 타입      | Not Null | 설명                                                         |
|------------------|---------|:--------:|------------------------------------------------------------|
| header           | Object  |    O     | 헤더 영역                                                      |
| - resultCode     | Integer |    O     | 결과 코드                                                      |
| - resultMessage  | String  |    O     | 결과 메시지                                                     |
| - isSuccessful   | Boolean |    O     | 성공 여부                                                      |
| plugins          | List    |    X     | 플러그인 리스트                                                   |
| - pluginId       | String  |    X     | 플러그인 아이디                                                   |
| - pluginType     | String  |    X     | 플러그인 타입(SECURE_IMAGE: 보안 이미지 전송, ONE_TIME_PROFILE: 개인정보 이용) |
| - pluginTypeName | String  |    X     | 플러그인 이름                                                    |
| - callbackUrl    | String  |    X     | 플러그인 버튼 클릭 시, 수신받을 콜백 URL                                  |
| - modifiable     | Boolean |    X     | 수정 가능 여부                                                   |
| - deletable      | Boolean |    X     | 삭제 가능 여부                                                   |

## 대체 발송 관리

### SMS AppKey 등록

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request body]

```
{
    "resendAppKey": String
}
```

| 이름           | 	타입     | 	필수 | 	설명                    |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | 대체 발송으로 설정할 SMS 서비스 앱키 |

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |

### 대체 발송 설정 등록

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| 이름           | 	타입      | 	필수 | 	설명                                                                                       |
|--------------|----------|-----|-------------------------------------------------------------------------------------------|
| senderKey    | 	String  | 	O  | 발신 키                                                                                      |
| isResend     | 	Boolean | 	O  | 발송 실패 시, 문자 대체발송 여부<br>Console에서 대체 발송 설정 시, default로 대체 발송 됩니다.                          |
| resendSendNo | 	String  | 	O  | 대체 발송 발신번호<br><span style="color:red">(SMS 상품에 등록된 발신번호가 아닐 경우, 대체발송이 실패할 수 있습니다.)</span> |

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | 헤더 영역  |
| - resultCode    | Integer |    O     | 결과 코드  |
| - resultMessage | String  |    O     | 결과 메시지 |
| - isSuccessful  | Boolean |    O     | 성공 여부  |
