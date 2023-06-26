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

## General Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[참고](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Required| Description|
|---|---|---|---|
|senderKey| String| O | Sender key(40 characters) |
|templateCode|  String| O | Registered delivery template code(up to 20 characters) |
|requestDate| String | X| Date and time of request(yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 30 days later |
|senderGroupingKey| String | X| Sender's grouping key(up to 100 characters) |
|createUser| String | X| Registrant(saved as user UUID when sending from console)|
|recipientList| List|   O|  List of recipients(up to 1000 persons) |
|- recipientNo| String| O|  Recipient number(up to 15 characters) |
|- templateParameter|   Object| X|  Template parameter<br>(required, if it includes a variable to be replaced for template) |
|-- key|    String| X | Replacement key(#{key})|
|-- value| String | X | Value which is mapped for replacement key|
|- recipientGroupingKey|    String| X|  Recipient grouping key(up to 100 characters) |

* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b> Delivery restricted during night(20:50~08:00 on the following day)</b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages -d '{"senderkey":"{Sender key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replaced field}":"{replacement data}"}}]}'
```

#### Response

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Body area|
|- requestId | String | Request ID |
|- senderGroupingKey | String | Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### List Messages

#### Request

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter] No. 1 or(2, 3) is conditionally required

| Name |  Type| Required| Description|
|---|---|---|---|
|requestId| String| Conditionally required(no.1) | Request ID |
|startRequestDate|  String| Conditionally required(no.2) | Start date of delivery request(yyyy-MM-dd HH:mm)|
|endRequestDate|    String| Conditionally required(no.2) |    End date of delivery request(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| Conditionally required(no.3) | Start date of registration(mm:HH dd-MM-yyyy)|
|endCreateDate|  String| Conditionally required(no.3) | End date of registration(mm:HH dd-MM-yyyy) |
|recipientNo|   String| X | Recipient number |
|senderKey| String| X | Sender key |
|templateCode|  String| X | Template code|
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|  String| X|  Recipient's grouping key |
|messageStatus| String |    X | Request status(COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled)   |
|resultCode| String |   X | Delivery result(BRC01 -> Successful, BRC02 ->Failed)   |
|pageNum|   Integer|    X|  Page number(default: 1)|
|pageSize|  Integer|    X|  Number of queries(default: 15, Max: 1000)|

* Cannot query data requested for delivery which are dated before 90 days.
* The maximum available days for delivery request is 30 days.

#### Response
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|messageSearchResultResponse|   Object| Body area|
|- messages | List |    List of messages |
|-- requestId | String |    Request ID |
|-- recipientSeq | Integer |    Recipient sequence number |
|-- plusFriendId | String | PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- templateCode | String | Template code |
|-- recipientNo | String |  Recipient number |
|-- content | String |  Body message |
|-- requestDate | String |  Date and time of request |
|-- createDate | String | Registered date and time |
|-- receiveDate | String |  Date and time of receiving |
|-- messageStatus | String |    Request status(COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled ) |
|-- resultCode | String |   Result code of receiving |
|-- resultCodeName | String |   Result code name of receiving |
|-- createUser | String | Registrant(saved as user UUID when sending from console) |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String | Recipient's grouping key |
|- totalCount | Integer | Total Count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### Get Messages

#### Request

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Typ| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|requestId| String| Request ID |
|recipientSeq|  Integer|    Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|message|   Object| Message|
|- requestId | String | Request ID |
|- recipientSeq | Integer | Recipient sequence number |
|- plusFriendId | String |  PlusFriend ID |
|- senderKey    | String |  Sender Key    |
|- templateCode | String |  Template code |
|- recipientNo | String |   Recipient number |
|- content | String |   Body message |
|- requestDate | String |   Date and time of request |
|- receiveDate | String |   Date and time of receiving |
|- createDate | String | Registered date and time |
|- buttons | List | List of buttons |
|-- name | String | Button name |
|-- type | String | Button type(WL: Web Link, AL: App Link, BK:Bot Keyword, MD: Message Delivery, BF: Business Form, AC: Added Channel) |
|-- linkMo | String |   Mobile web link(required for the WL type) |
|-- linkPc | String |   PC web link(optional for the WL type) |
|-- schemeIos | String |    iOS app link(required for the AL type) |
|-- schemeAndroid | String |    Android app link(required for the AL type) |
|-- bizFormId|	Integer|	Business Form(required for the BF type) |
|- resultCode | String |    Result code of receiving |
|- resultCodeName | String |    Result code name of receiving |
|- createUser | String | Registrant(saved as user UUID when sending from console) |
|- senderGroupingKey | String | Sender's grouping key |
|- recipientGroupingKey | String |  Recipient grouping key |

## Messages
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|requestId| String| Request ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|recipientSeq|  String| X | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

## Templates

### Register Templates
#### Request
[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey |
|senderKey| String| Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| Name |  Type| Required| Description|
|---|---|---|---|
|templateCode|  String |    O | Template code(up to 20 characters) |
|templateName|  String |    O | Template name(up to 150 characters) |
|messageType| String | X |Types of template message(I: Image type, W: Wide Image type) |
|contentType| String | X |Types of template text(F: Fixed content, V: Variable content) |
|unsubscribeContent|	String |	O | 무료수신거부 전화번호/인증번호 |
|templateContent|	String |	O | Template body(up to 1000 characters) |
|templateImageLink | String |	X | Template Image Link |
|image | String |	X | 이미지 URL |
| buttons[i].type|	String |	X | Button type(WL: Web Link, AL: App Link, BK:Bot Keyword, MD: Message Delivery, BF: Business Form, AC: Added Channel) |
| buttons[i].name| String |	X |	Button name(up to 14 characters) |
| buttons[i].linkMo| String |	X |	Mobile web link(required for the WL type) |
| buttons[i].linkPc | String |	X |PC web link(optional for the WL type) |
| buttons[i].schemeAndroid | String | X |	Android app link(required for the AL type) |
| buttons[i].schemeIos | String | X |	iOS app link(required for the AL type) |
| buttons[i].bizFormId | Integer | X |	Business Form(required for the BF type) |

* contentType이 변수형(V)인 경우 템플릿 내용(templateContent)에 변수 입력 가능
* 변수명은 최대 20자 이내 한/영/숫자/허용된 특수기호('-', '_')로만 입력 가능
* 최대 20개의 변수명 입력 가능(중복 제외)
* 무료수신거부 전화번호에 인증번호가 없는 경우(예: 080-1111-2222)
* 무료수신거부 전화번호에 인증번호가 있는 경우는 / 로 구분해서 입력(예: 080-1111-2222/12345)
* 인증번호는 숫자 1~10자리 입니다.

* 이미지형은 버튼 수 최대 5개, 와이드형은 버튼 수 최대 2개까지 가능
* AC 버튼이 포함된 경우, 이미지형은 첫번째 버튼 / 와이드형은 마지막 버튼의 순서대로 입력
* AC 버튼은 버튼명이 "채널 추가"로 고정
* contentType이 변수형(V)인 경우 링크(linkMo, linkPc, schemeAndroid, schemeIos)에 변수 입력 가능.
* BF 버튼만 포함된 경우, 이미지형은 첫번째 버튼 / 와이드형은 마지막 버튼의 순서대로 입력
* AC 버튼과 BF 버튼이 동시에 쓰일 경우 BF 버튼은, 이미지형은 두번째 버튼 / 와이드형은 첫번째 버튼의 순서대로 입력
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

#### Response
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  }
}
```

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|

### List Templates

#### Request

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |  Type| Description|
|---|---|---|
|appkey|    String| Unique appkey|
|senderKey| String| Sender Key |
|templateCode|  String| Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |  Type| Required| Description|
|---|---|---|---|
|X-Secret-Key|  String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

#### Response
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

| Name |  Type| Description|
|---|---|---|
|header|    Object| Header area|
|- resultCode|  Integer|    Result code|
|- resultMessage|   String| Result message|
|- isSuccessful|    Boolean| Successful or not|
|template|	Object| Body area|
|- plusFriendId | String |	PlusFriend ID |
|- senderKey    | String | Sender Key    |
|- templateCode | String |	Template code |
|- templateName | String |	Template name |
|- messageType| String | Types of template message(I: Image type, W: Wide Image type) |
|- contentType| String | Types of template text(F: Fixed content, V: Variable content) |
|- templateContent | String |	Template body |
|- unsubscribeContent | String | 무료수신거부 전화번호/인증번호 |
|- templateImageLink | String | Template Image Link |
|- templateImageUrl | String |	Template Image URL |
|- buttons | List |	List of buttons |
|-- name | String |	Button name |
|-- type | String |	Button type(WL: Web Link, AL: App Link, BK:Bot Keyword, MD: Message Delivery, BF: Business Form, AC: Added Channel) |
|-- linkMo | String |	Mobile web link(required for the WL type) |
|-- linkPc | String |	PC web link(optional for the WL type) |
|-- schemeAndroid | String |	Android app link(required for the AL type) |
|-- schemeIos | String |	iOS app link(required for the AL type) |
|-- bizFormId | Integer |	Business Form(required for the BF type) |
|- kakaoStatus | String | Status code of Kakao Template(A: Normal, R: 대기(발송전), S: Blocked) |
|- createDate | String | Date of creation |
|- updateDate | String | Date of modification |
