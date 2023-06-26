## Notification > KakaoTalk Bizmessage > BrandTalk > API v2.2 Guide

## BrandTalk

#### [API Domain]

<table>
<thead>
<tr>
<th>Domain</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## 일반 메시지

### 메시지 치환 발송 요청

[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

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
        "recipientGroupingKey": String
    }]
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키(40자) |
|templateCode|	String|	O | 등록한 발송 템플릿 코드(최대 20자) |
|requestDate| String | X| 요청 일시(yyyy-MM-dd HH:mm:ss)<br>(입력하지 않을 경우 즉시 발송)<br>최대 30일 이후까지 예약 가능 |
|senderGroupingKey| String | X| 발신 그룹핑 키(최대 100자) |
|createUser| String | X| 등록자(콘솔에서 발송 시 사용자 UUID로 저장)|
|recipientList|	List|	O|	수신자 리스트(최대 1000명) |
|- recipientNo|	String|	O|	수신번호(최대 15자) |
|- templateParameter|	Object|	X|	템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
|-- key|	String|	X |	치환 키(#{key})|
|-- value| String |	X |	치환 키에 매핑되는 Value값|
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키(최대 100자) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>야간 발송 제한(20:50~다음 날 08:00)</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	본문 영역|
|- requestId | String |	요청 아이디 |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

### 메시지 리스트 조회

#### 요청

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

[Query parameter] 1번 or(2번, 3번) 조건 필수

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수(1번) | 요청 아이디 |
|startRequestDate|	String|	조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수(2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| 조건 필수(3번) | 등록 날짜 시작값(yyyy-MM-dd HH:mm)|
|endCreateDate|  String| 조건 필수(3번) | 등록 날짜 끝값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신번호 |
|senderKey|	String|	X |	발신 키 |
|templateCode|	String|	X |	템플릿 코드|
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 )	|
|resultCode| String |	X | 발송 결과( BRC01 -> 성공, BRC02 -> 실패 )	|
|pageNum|	Integer|	X|	페이지 번호(Default: 1)|
|pageSize|	Integer|	X|	조회 건수(Default: 15, Max: 1000)|

* 90일 이전 발송 요청 데이터는 조회되지 않습니다.
* 발송 요청 일시의 범위는 최대 30일입니다.

#### 응답
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "messageSearchResultResponse" : {
    "messages" : [
    {
      "requestId" :  String,
      "recipientSeq" : Integer,
      "plusFriendId" :  String,
      "senderKey" : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount" :  Integer
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|messageSearchResultResponse|	Object|	본문 영역|
|- messages | List |	메시지 리스트 |
|-- requestId | String |	요청 아이디 |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- senderKey    | String | 발신 키    |
|-- templateCode | String |	템플릿 코드 |
|-- recipientNo | String |	수신 번호 |
|-- requestDate | String |	요청 일시 |
|-- createDate | String | 등록 일시 |
|-- receiveDate | String |	수신 일시 |
|-- content | String |	본문 |
|-- messageStatus | String |	요청 상태( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### 메시지 단건 조회

#### 요청

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키 |
|requestId|	String|	요청 아이디 |
|recipientSeq|	Integer|	수신자 시퀀스 번호 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### 응답
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "message" : {
      "requestId" :  String,
      "recipientSeq" : Integer,
      "plusFriendId" :  String,
      "senderKey" : String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "content" :  String,
      "templateImageUrl": String,
      "templateImageLink": String,
      "buttons" : [
        {
          "name" :  String,
          "type" :  String,
          "linkMo" :  String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String,
          "bizFormId": Integer
        }
      ],
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|message|	Object|	메시지|
|- requestId | String |	요청 아이디 |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 ID |
|- senderKey    | String |  발신 키    |
|- templateCode | String |	템플릿 코드 |
|- recipientNo | String |	수신 번호 |
|- requestDate | String |	요청 일시 |
|- createDate | String | 등록 일시 |
|- receiveDate | String |	수신 일시 |
|- content | String |	본문 |
|- buttons | List |	버튼 리스트 |
|-- name | String |	버튼 이름 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, BK:봇 키워드, MD:메시지 전달, AC: 채널 추가, BF: 비지니스 폼) |
|-- linkMo | String |	모바일 웹 링크(WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크(WL 타입일 경우 선택 필드) |
|-- schemeAndroid | String |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|-- schemeIos | String |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
|-- bizFormId|	Integer|	버튼 클릭 시 실행할 비즈니스폼ID |
|- messageStatus | String |	요청 상태( COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|- senderGroupingKey | String | 발신 그룹핑 키 |
|- recipientGroupingKey | String |	수신자 그룹핑 키 |

## 메시지
### 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|
|requestId| String| 요청 ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|recipientSeq|	String|	X | 수신자 시퀀스 번호<br>(입력하지 않으면 요청 ID의 모든 발송 건을 취소) |

* 일반/인증 메시지 모두 동일한 API로 취소할 수 있습니다.

#### 응답
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

## 템플릿

### 템플릿 등록
#### 요청
[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키 |
|senderKey|	String|	발신 키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

[Request parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|templateCode|	String |	O | 템플릿 코드(최대 20자) |
|templateName|	String |	O | 템플릿명(최대 150자) |
|messageType| String | O |템플릿 메시지 유형(I: 이미지형, W: 와이드형) |
|contentType| String | O |텍스트 유형(F: 고정형, V: 변수형) |
|unsubscribeContent|	String |	O | 무료수신거부 전화번호/인증번호 |
|templateContent|	String |	O | 템플릿 본문(최대 1000자) |
|templateImageLink | String |	O | 템플릿 이미지 링크 |
|image | File |	O | 이미지 파일 |
| buttons[i].type|	String |	X | 버튼 타입(WL:웹링크, AL:앱링크, BK:봇 키워드, MD:메시지 전달, AC: 채널 추가, BF: 비지니스 폼) |
| buttons[i].name| String |	X |	버튼 이름(버튼이 있는 경우 필수, 최대 14자)|
| buttons[i].linkMo| String |	X |	모바일 웹 링크(WL 타입일 경우 필수 필드)|
| buttons[i].linkPc | String |	X |PC 웹 링크(WL 타입일 경우 선택 필드) |
| buttons[i].schemeAndroid | String | X |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
| buttons[i].schemeIos | String | X |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
| buttons[i].bizFormId | Integer | X |	버튼 클릭 시 실행할 비즈니스폼ID |

* contentType이 변수형(V)인 경우 템플릿 내용(templateContent)에 변수 입력 가능
* 변수명은 최대 20자 이내 한/영/숫자/허용된 특수기호('-', '_')로만 입력 가능
* 최대 20개의 변수명 입력 가능(중복 제외)
* 무료수신거부 전화번호에 인증번호가 없는 경우(예: 080-1111-2222)
* 무료수신거부 전화번호에 인증번호가 있는 경우는 / 로 구분해서 입력(예: 080-1111-2222/12345)
* 인증번호는 숫자 1~10자리 입니다.

* 이미지형은 버튼 수 최대 5개, 와이드형은 버튼 수 최대 2개까지 가능
* AC 버튼이 포함된 경우, 이미지형은 첫 번째 버튼 / 와이드형은 마지막 버튼의 순서대로 입력
* AC 버튼은 버튼명이 "채널 추가"로 고정
* contentType이 변수형(V)인 경우 링크(linkMo, linkPc, schemeAndroid, schemeIos)에 변수 입력 가능.
* BF 버튼만 포함된 경우, 이미지형은 첫 번째 버튼 / 와이드형은 마지막 버튼의 순서대로 입력
* AC 버튼과 BF 버튼이 동시에 쓰일 경우 BF 버튼은, 이미지형은 두번째 버튼 / 와이드형은 첫 번째 버튼의 순서대로 입력
* BF 버튼은 버튼명이 "톡에서 예약하기", "톡에서 설문하기", "톡에서 응모하기" 중 택1하여 사용하여야 함
* BF 버튼 링크는 카카오 for 비즈니스에서 생성한 비즈니스폼 ID로 등록이 가능하며, 아래 조건이 유효해야 함
  1) 비즈니스폼 등록 관리자와 채널 관리자가 일치하여야 함(관리자 권한 무관)
  2) 비즈니스폼 상태가 작성완료 / 실행 이고, 종료일이 등록일보다 미래여야 함
  3) 비즈니스폼 내 채널 추가 옵션이 있을 경우, 메시지 발송 채널과 일치하여야 함

* image 상세
  * 기본 이미지 정책
    * 권장사이즈 : 800px * 400px
    * 제한사이즈 : 가로 500px 이상. 가로:세로 비율 2:1 이상 3:4 이하.
    * 지원파일 : jpg.png / 최대 2MB

  * 와이드 말풍선 이미지 정책
    * 권장 사이즈 : 800px * 600px
    * 제한사이즈 : 가로 500px 이상. 가로:세로 비율 2:1 이상 1:1 이하.
    * 파일형식 및 크기 : jpg, png / 최대 2MB

#### 응답
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|

### 템플릿 조회

#### 요청

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|
|senderKey|	String|	발신 키 |
|templateCode|	String|	템플릿 코드 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 콘솔에서 생성할 수 있다.  |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "template": {
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "templateName": String,
        "messageType": String,
        "contentType": String,
        "templateContent": String,
        "unsubscribeContent": String,
        "templateImageLink": String,
        "templateImageUrl": String,
        "buttons": [
            {
                "name": String,
                "type": String,
                "linkMo": String,
                "linkPc": String,
                "schemeAndroid": String,
                "schemeIos": String,
                "bizFormId": Integer
            }
        ],
        "kakaoStatus": String,
        "createDate": String,
        "updateDate": String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|template|	Object|	본문 영역|
|- plusFriendId | String |	카카오톡 채널 검색용 ID 또는 발신 프로필 그룹명 |
|- senderKey    | String | 발신 키 |
|- templateCode | String |	템플릿 코드 |
|- templateName | String |	템플릿명 |
|- messageType| String | 템플릿 메시지 유형(I: 이미지형, W: 와이드형) |
|- contentType| String | 텍스트 유형(F: 고정형, V: 변수형) |
|- templateContent | String |	템플릿 본문 |
|- unsubscribeContent | String | 무료수신거부 전화번호/인증번호 |
|- templateImageLink | String | 템플릿 이미지 링크 |
|- templateImageUrl | String |	이미지 URL |
|- buttons | List |	버튼 리스트 |
|-- name | String |	버튼 이름 |
|-- type | String |	버튼 타입(WL:웹링크, AL:앱링크, BK:봇 키워드, MD:메시지 전달, AC: 채널 추가, BF: 비지니스 폼) |
|-- linkMo | String |	모바일 웹 링크(WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크(WL 타입일 경우 선택 필드) |
|-- schemeAndroid | String |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|-- schemeIos | String |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
|-- bizFormId | Integer |	버튼 클릭 시 실행할 비즈니스폼ID |
|- kakaoStatus | String | 템플릿 상태(A:정상, R:대기(발송전), S:중단) |
|- createDate | String | 생성일자 |
|- updateDate | String | 수정일자 |
