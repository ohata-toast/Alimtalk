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

* When contentType  is Variable(V) Type, can input the variables via Content(templateContent)
* Variable names can only be entered with one/zero/number/allowed special symbol('-', '_') and up to 20 characters
* Can enter the maximum 20 Variable names(Excluding duplication)
* When Free rejection phone number doesn’t have authentication number(e.g.: 080-1111-2222)
* When Free rejection phone number have authentication number, separately input with / *(e.g.: 080-1111-2222/12345)
* The authentication number is 1 to 10 digits.

* Available up to 5 buttons for image type and up to 2 buttons for wide type
* If AC button is included, enter the first button for the image type and the last button for the wide type in sequence
* Button name for AC button is fixed with ""Add Channel""
* If contentType is Variable type(V), can input variables in the link(linkMo, linkPc, schemeAndroid, schemeIos).
* If only BF button is included, input the first button for image type / the last button for wide type in sequence
* If AC button and BF button are used simultaneously, the BF button is entered in the order of the second button for image type and the first button for the wide type in sequence
* BF button has to be used by selecting one out of "Reserve from Talk," "Questionnaire from Talk" and "Apply from Talk"
* BF button link can be registered with Business Form ID generated by Kakao for Business and the following conditions have to be valid
  1) Business Form registration manager and channel manager have to be the same(regardless of administrator privileges)
  2) Business Form status has to be completed/executed and termination date has to be in the future than registration date
  3) If there is an option to Add Channels within Business Form, it has to be the same channel for sending messages channel

* image Details
  * Basic Image Policy
    * Recommended Size : 800px * 400px
    * Restricted Size: more than 500px in length. Length: Width ratio of more than 2:1 of and less than 3:4.
    * Support Files : jpg.png / Maximum 2MB

  * Wide speech balloon image Policy
    * Recommended Size : 800px * 600px
    * Restricted Size: more than 500px in length. Length: Width ratio of more than 2:1 of and less than 1:1.
    * File format and size : jpg, png / maximum 2MB

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
