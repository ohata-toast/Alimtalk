## Notification > KakaoTalk Bizmessage > AlimTalk > API v2.3 Guide

## AlimTalk

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

## Overview of v2.3 API
1. Added Quick Reply, Item List, Talk Biz plugin, Main Link, and Business Form button.

2. Added the AlimTalk item highlight image registration API.
3. Added APIs to register, modify, delete, and retrieve AlimTalk plugins.
4. Removed the buttons field from the API to retrieve message list.

## General Messages

### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

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
          "isResend" : boolean,
          "resendType" : String,
          "resendTitle" : String,
          "resendContent" : String,
          "resendSendNo" : String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String
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

| Name |	Type|	Required|	Description|
|---|---|---|---|
|senderKey|	String|	O | Sender key(40 characters) |
|templateCode|	String|	O | Registered delivery template code(up to 20 characters) |
|requestDate| String | X| Date and time of request(yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 30 days later |
|senderGroupingKey| String | X| Sender's grouping key(up to 100 characters) |
|createUser| String | X| Registrant(saved as user UUID when sending from console)|
|recipientList|	List|	O|	List of recipients(up to 1000 persons) |
|- recipientNo|	String|	O|	Recipient number(up to 15 characters) |
|- templateParameter|	Object|	X|	Template parameter<br>(required, if it includes a variable to be replaced for template) |
|-- key|	String|	X |	Replacement key(#{key})|
|-- value| String |	X |	Value which is mapped for replacement key|
|- resendParameter|	Object|	X| Alternative delivery information |
|-- isResend|	boolean|	X|	Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console. |
|-- resendType|	String|	X|	Alternative delivery type(SMS,LMS)<br>Categorized by the length of template body if value is unavailable. |
|-- resendTitle|	String|	X|	Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.) |
|-- resendContent|	String|	X|	Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|-- resendSendNo | String| X| Sender number for alternative delivery<br><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|- buttons|	List|	X| Additional information for buttons |
|-- ordering            | Integer  | X        |	Button sequence(required, if there is a button)|
|-- chatExtra|	String|	X| Meta information to send for BC(Bot for Consultation) or BT(Bot Transfer) type buttons |
|-- chatEvent|	String|	X| Bot event name to connect for BT(Bot Transfer) type button |
|-- relayId|	String|	X| Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed |
|-- oneClickId|	String|	X| Payment information used in the one click payment plugin |
|-- productId|	String|	X| Payment information used in the one click payment plugin |
|-- target|	String|	X |	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- quickReplies|	List|	X| Quick reply information |
|-- ordering            | Integer  | X        |	Quick reply order(required when quick reply exists)|
|-- chatExtra|	String|	X| Meta information to send for BC(Bot for Consultation) or BT(Bot Transfer) type |
|-- chatEvent|	String|	X| Bot event name to connect for BT(Bot Transfer) type |
|-- target|	String|	X |	In the case of a web link type, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- recipientGroupingKey|	String|	X|	Recipient grouping key(up to 100 characters) |
|messageOption | Object |	X | Message Option |
|- price | Integer |	X | Price/amount/payment amount included in message(message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |	X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message(message to be delivered to the user)(related to moment advertisement) |
| statsId | String |	X | Statistics ID(not included in the delivery search conditions, up to 8 characters) |

* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS(e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>The SMS Service supports international SMS only. For international receiver numbers, the resendType(alternative delivery type) must be changed to SMS to allow sending without fail. </b>
* <b>Title or content for alternative delivery that exceeds specified byte size may be cut for delivery.(see [[Caution](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] for reference)</b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","templateParameter":{"{치환자 필드}":"{치환 데이터}"}}]}'
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|message|	Object|	Body area|
|- requestId | String |	Request ID |
|- senderGroupingKey | String |	Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

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
            "templateTitle" : String,
            "templateHeader" : String,
            "templateItem" : {
              "list" : [{
                "title": String,
                "description": String
              }],
              "summary" : {
                "title": String,
                "description": String
              }
            },
            "templateItemHighlight" : {
              "title": String,
              "description": String,
              "imageUrl": String
            },
            "templateRepresentLink" : {
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            },
            "buttons" : [
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
                    "bizFormId": String,
                    "pluginId": String,
                    "relayId": String,
                    "oneClickId": String,
                    "productId": String,
                    "target": String
                }
            ],
            "quickReplies" : [
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
                    "bizFormId": String,
                    "target": String
                }
            ],
            "resendParameter": {
              "isResend" : boolean,
              "resendType" : String,
              "resendTitle" : String,
              "resendContent" : String,
              "resendSendNo" : String
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

| Name |	Type|	Required|	Description|
|---|---|---|---|
|senderKey|	String|	O | Sender key(40 characters) |
|templateCode|	String|	O | Registered delivery template code(up to 20 characters) |
|requestDate| String | X| Date and time of request(yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 30 days later |
|senderGroupingKey| String | X| Sender's grouping key(up to 100 characters) |
|createUser| String | X| Registrant(saved as user UUID when sending from console)|
|recipientList|	List|	O|	List of recipients(up to 1,000 persons) |
|- recipientNo|	String|	O|	Recipient number(up to 15 characters) |
|- content|	String|	O|	Message(up to 1000 characters) |
|- templateTitle| String| X| Title(up to 50 characters) |
|- templateHeader| String| X| Template header(up to 16 characters) |
|- templateItem | Object | X| Item |
|-- list | List | X | Item list(at least 2, up to 10) |
|--- title | String | X | Title(up to 6 characters) |
|--- description | String | X | Description(up to 23 characters) |
|-- summary | Object | X | Item summary information |
|--- title | String | X | Title(up to 6 characters) |
|--- description | String | X | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
|- templateItemHighlight | Object | X| Item highlight |
|--- title | String | X | Title(up to 30 characters, 21 characters with a thumbnail image) |
|--- description | String | X | Description(up to 19 characters, 13 with a thumbnail image) |
|--- imageUrl | String | X | Thumbnail image address |
|- templateRepresentLink | Object | X| Main link |
|-- linkMo| String |	X |	Mobile web link(up to 500 characters)|
|-- linkPc | String |	X |PC web link(up to 500 characters) |
|-- schemeIos | String | X |	iOS app link(up to 500 characters) |
|-- schemeAndroid | String | X |	Android app link(up to 500 characters) |
|- buttons|	List |	X | List of buttons(up to 5) |
|-- ordering|	Integer|	X |	Button sequence(required, if there is a button)|
|-- type| String |	X |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
|-- name| String |	X |	Button name(required if there is a button, up to 14 characters)|
|-- linkMo| String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
|-- linkPc | String |	X |PC web link(optional for the WL type, up to 500 characters) |
|-- schemeIos | String | X |	iOS app link(required for the AL type, up to 500 characters) |
|-- schemeAndroid | String | X |	Android app link(required for the AL type, up to 500 characters) |
|-- chatExtra|	String|	X| Meta information to send for BC(Bot for Consultation) or BT(Bot Transfer) type buttons |
|-- chatEvent|	String|	X| Bot event name to connect for BT(Bot Transfer) type button |
|-- bizFormId|	Integer|	X |	Business form ID(required for BF type) |
|-- pluginId|	String|	X |	Plugin ID(up to 24 characters) |
|-- relayId|	String|	X| Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed |
|-- oneClickId|	String|	X| Payment information used in the one click payment plugin |
|-- productId|	String|	X| Payment information used in the one click payment plugin |
|-- target|	String|	X |	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- quickReplies|	List |	X | Quick reply list(up to 5) |
|-- ordering|	Integer|	X |	Quick reply order(required  when quick reply exists)|
|-- type| String |	X |	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form) |
|-- name| String |	X |	Quick reply name(required when quick reply exists, up to 14 characters)|
|-- linkMo| String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
|-- linkPc | String |	X |PC web link(optional for the WL type, up to 500 characters) |
|-- schemeIos | String | X |	iOS app link(required for the AL type, up to 500 characters) |
|-- schemeAndroid | String | X |	Android app link(required for the AL type, up to 500 characters) |
|-- pluginId|	String|	X |	Plugin ID(up to 24 characters) |
|-- target|	String|	X |	In the case of a web link type, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- resendParameter|	Object|	X| Alternative delivery information |
|-- isResend|	boolean|	X|	Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console. |
|-- resendType|	String|	X|	Alternative delivery type(SMS,LMS)<br>Categorized by the length of template message, if value is unavailable. |
|-- resendTitle|	String|	X|	Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.) |
|-- resendContent|	String|	X|	Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.) |
|-- resendSendNo | String| X| Sender number for alternative delivery<br><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|- recipientGroupingKey|	String|	X|	Recipient's grouping key(up to 100 characters) |
| messageOption | Object |	X | Message Option |
|- price | Integer |	X | Price/amount/payment amount included in message(message to be delivered to user)(related to moment advertisement) |
|- currencyType | String |	X| Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message(message to be delivered to the user)(related to moment advertisement) |
| statsId | String |	X | Statistics ID(not included in the delivery search conditions, up to 8 characters) |

* <b>Enter data completed with replacement for the body and button. </b>
* <b>Request date and time can be set up to 90 days since a point of calling.</b>
* <b>Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS(e.g. Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>The SMS Service supports international SMS only. For international receiver numbers, the resendType(alternative delivery type) must be changed to SMS to allow sending without fail. </b>
* <b>Title or content for alternative delivery that exceeds specified byte size may be cut for delivery.(see [[Caution](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] for reference)</b>

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/raw-messages -d '{"senderKey":"{발신 키}","templateCode":"{템플릿 코드}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{수신번호}","content":"{내용}","buttons":[{"ordering":"{버튼 순서}","type":"{버튼 타입}","name":"{버튼 이름}","linkMo":"{모바일 웹 링크}"}]}]}'
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|message|	Object|	Body area|
|- requestId | String |	Request ID |
|- senderGroupingKey | String |	Sender's grouping key |
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
GET  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Typ|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Typ|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Query parameter] No. 1 or(2, 3) is conditionally required

| Name |	Typ|	Required|	Description|
|---|---|---|---|
|requestId|	String|	Conditionally required(no.1) | Request ID |
|startRequestDate|	String|	Conditionally required(no.2) | Start date of delivery request(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| Conditionally required(no.2) |	End date of delivery request(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| Conditionally required(no.3) | Start date of registration(mm:HH dd-MM-yyyy)|
|endCreateDate|  String| Conditionally required(no.3) | End date of registration(mm:HH dd-MM-yyyy) |
|recipientNo|	String|	X |	Recipient number |
|senderKey|	String|	X |	Sender Key |
|templateCode|	String|	X |	Template code|
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|	String|	X|	Recipient's grouping key |
|messageStatus| String |	X | Request status(COMPLETED -> successful, FAILED -> failed, CANCEL -> cancelled )	|
|resultCode| String |	X | Delivery result(MRC01 -> Successful, MRC02 ->Failed)	|
|createUser| String | X| Registrant(saved as user UUID when sending from console)|
|pageNum|	Integer|	X|	Page number(default: 1)|
|pageSize|	Integer|	X|	Number of queries(default: 15, Max: 1000)|

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
      "content" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|messageSearchResultResponse|	Object|	Body area|
|- messages | List |	List of messages |
|-- requestId | String |	Request ID |
|-- recipientSeq | Integer |	Recipient sequence number |
|-- plusFriendId | String |	PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- templateCode | String |	Template code |
|-- recipientNo | String |	Recipient number |
|-- content | String |	Body message |
|-- requestDate | String |	Date and time of request |
|-- createDate | String | Registered date and time |
|-- receiveDate | String |	Date and time of receiving |
|-- resendStatus | String |	Status code of resending(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|-- resendStatusName | String |	Status code name of resending |
|-- messageStatus | String |	Request status(COMPLETED -> successful, FAILED -> failed, CANCEL -> cancelled ) |
|-- createUser | String | Registrant(saved as user UUID when sending from console) |
|-- resultCode | String |	Result code of receiving |
|-- resultCodeName | String |	Result code name of receiving |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String |	Recipient grouping key |
|- totalCount | Integer | Total Count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |
|requestId|	String|	Request ID |
|recipientSeq|	Integer|	Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### Response
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
      "content" :  String,
      "templateTitle" : String,
      "templateSubtitle" : String,
      "templateExtra" : String,
      "templateAd" : String,
      "templateHeader" : String,
      "templateItem" : {
        "list" : [{
          "title": String,
          "description": String
        }],
        "summary" : {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight" : {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink" : {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "requestDate" :  String,
      "receiveDate" : String,
      "createDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" : String,
      "resendRequestId" : String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
      "buttons" : [
        {
          "ordering" :  Integer,
          "type" :  String,
          "name" :  String,
          "linkMo" :  String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "bizFormId": String,
          "pluginId": String,
          "relayId": String,
          "oneClickId": String,
          "productId": String,
          "target": String
        }
      ],
      "quickReplies" : [
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
          "bizFormId": String,
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

| Name |	Type| 	Description                                                                                                                                                                                                       |
|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|header|	Object| 	Header area                                                                                                                                                                                                       |
|- resultCode|	Integer| 	Result code                                                                                                                                                                                                       |
|- resultMessage|	String| Result message                                                                                                                                                                                                     |
|- isSuccessful|	Boolean| Successful or not                                                                                                                                                                                                  |
|message|	Object| 	Message                                                                                                                                                                                                           |
|- requestId | String | 	Request ID                                                                                                                                                                                                        |
|- recipientSeq | Integer | 	Recipient sequence number                                                                                                                                                                                         |
|- plusFriendId | String | 	PlusFriend ID                                                                                                                                                                                                     |
|- senderKey    | String | Sender Key                                                                                                                                                                                                         |
|- templateCode | String | 	Template code                                                                                                                                                                                                     |
|- recipientNo | String | 	Recipient number                                                                                                                                                                                                  |
|- content | String | 	Body message                                                                                                                                                                                                      |
|- templateTitle | String | Template title                                                                                                                                                                                                     |
|- templateSubtitle | String | Auxiliary template phrase                                                                                                                                                                                          |
|- templateExtra | String | Additional template information                                                                                                                                                                                    |
|- templateAd | String | Request for consent of receiving within template or simple ad phrases                                                                                                                                              |
|- templateHeader| String| Template header(up to 16 characters)                                                                                                                                                                               |
|- templateItem | Object | Item                                                                                                                                                                                                               |
|-- list | List | Item list(at least 2, up to 10)                                                                                                                                                                                    |
|--- title | String | Title(up to 6 characters)                                                                                                                                                                                          |
|--- description | String | Description(up to 23 characters)                                                                                                                                                                                   |
|-- summary | Object | Item summary information                                                                                                                                                                                           |
|--- title | String | Title(up to 6 characters)                                                                                                                                                                                          |
|--- description | String | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                  |
|- templateItemHighlight | Object | Item highlight                                                                                                                                                                                                     |
|--- title | String | Title(up to 30 characters, 21 characters with a thumbnail image)                                                                                                                                                   |
|--- description | String | Description(up to 19 characters, 13 with a thumbnail image)                                                                                                                                                        |
|--- imageUrl | String | Thumbnail image address                                                                                                                                                                                            |
|- templateRepresentLink | Object | Main link                                                                                                                                                                                                          |
|-- linkMo| String | 	Mobile web link(up to 500 characters)                                                                                                                                                                             |
|-- linkPc | String | PC web link(up to 500 characters)                                                                                                                                                                                  |
|-- schemeIos | String | 	iOS app link(up to 500 characters) |
|-- schemeAndroid | String | 	Android app link(up to 500 characters) |
|- requestDate | String | 	Date and time of request                                                                                                                                                                                          |
|- receiveDate | String | 	Date and time of receiving                                                                                                                                                                                        |
|- createDate | String | Registered date and time                                                                                                                                                                                           |
|- resendStatus | String | 	Status code of resending(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below)    |
|- resendStatusName | String | 	Status code name of resending                                                                                                                                                                                     |
|- resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api)                                                                                               |
|- resendRequestId | String | Resending SMS request ID                                                                                                                                                                                           |
|- messageStatus | String | 	Request status(COMPLETED -> successful, FAILED -> failed, CANCEL -> cancelled )                                                                                                                                   |
|- resultCode | String | 	Result code of receiving                                                                                                                                                                                          |
|- resultCodeName | String | 	Result code name of receiving                                                                                                                                                                                     |
|- createUser | String | Registrant(saved as user UUID when sending from console)                                                                                                                                                           |
|- buttons | List | 	List of buttons                                                                                                                                                                                                   |
|-- ordering | Integer | 	Button sequence                                                                                                                                                                                                   |
|-- type| String |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
|-- name | String | 	Button name                                                                                                                                                                                                       |
|-- linkMo | String | 	Mobile web link(required for the WL type)                                                                                                                                                                         |
|-- linkPc | String | 	PC web link(optional for the WL type)                                                                                                                                                                             |
|-- schemeIos | String | 	iOS app link(required for the AL type)                                                                                                                                                                            |
|-- schemeAndroid | String | 	Android app link(required for the AL type)                                                                                                                                                                        |
|-- chatExtra|	String| 	Meta information to send for BC(Bot for Consultation) or BT(Bot Transfer) type buttons                                                                                                                            |
|-- chatEvent|	String| Bot event name to connect for BT(Bot Transfer) type button                                                                                                                                                         |
|-- bizFormId|	Integer|	Business form ID(required for BF type) |
|-- pluginId|	String| 	Plugin ID(up to 24 characters) |
|-- relayId|	String|  Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed |
|-- oneClickId|	String| 	 Payment information used in the one click payment plugin |
|-- productId|	String| 	 Payment information used in the one click payment plugin |
|-- target|	String| 	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                         |
|- quickReplies|	List | 	 Quick reply list(up to 5) |
|-- ordering|	Integer| 	Quick reply order(required  when quick reply exists)|
|-- type| String |	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form) |
|-- name| String |	Quick reply name(required  when quick reply exists, up to 14 characters)|
|-- linkMo| String | 	Mobile web link(required for the WL type, for up to 500 characters)|
|-- linkPc | String | 	PC web link(required for the WL type, for up to 500 characters) |
|-- schemeIos | String |	iOS app link(required for the AL type, for up to 500 characters) |
|-- schemeAndroid | String |	Android app link(required for the AL type, for up to 500 characters) |
|-- pluginId|	String| 	Plugin ID(up to 24 characters) |
|-- target|	String| 	In the case of a web link type, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- messageOption | Object | 	Message Option                                                                                                                                                                                                    |
|-- price | Integer | 	Price/amount/payment amount included in message(message to be delivered to user)(related to moment advertisement)                                                                                                 |
|-- currencyType | String | 	Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message(message to be delivered to the user)(related to moment advertisement) |
|- senderGroupingKey | String | Sender's grouping key                                                                                                                                                                                              |
|- recipientGroupingKey | String | 	Recipient's grouping key                                                                                                                                                                                          |

## Authentication Messages

<span id="precautions-authword"></span>
1. Guide for authentication words required to be included for Authentication Messages API

| Category  | Authentication Words |
| --- | --- |
| Authentication Messages | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- Example 1-1) Delivery shall fail if the full text(including template replacement) does not include authentication words, in the request of Authentication Messages API(for emergency)
- Example 1-2) Validity for English words shall be checked regardless of small or capital letters


### Request of Sending Replaced Messages

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request body]
[Same as the above](./sender-console-guide/#request-of-sending-replaced-messages)

### Request of Sending Full Text

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]
[Same as the above](./sender-console-guide/#request-of-sending-full-text)

### List Messages

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Query parameter]
[Same as the above](./sender-console-guide/#list-messages)

### Get Messages

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |
|requestId|	String|	Request ID |
|recipientSeq|	Integer|	Recipient sequence number |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
```

#### Response
[Same as the above](./sender-console-guide/#get-messages)

## Message
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|requestId| String| Request ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|recipientSeq|	String|	X | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

* Both general and authentication messages can be canceled by the same API.

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### Query Updates of Message Result

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|startUpdateDate|	String|	O | Start date of querying result updates(yyyy-MM-dd HH:mm)|
|endUpdateDate|	String| O |	End date of querying result updates(yyyy-MM-dd HH:mm) |
|alimtalkMessageType|	String| X |	AlimTalk message type(NORMAL, AUTH) |
|pageNum|	Integer|	X|	Page number(default: 1)|
|pageSize|	Integer|	X|	Number of queries(default: 15, Max: 1000)|

#### Response
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "messages" : [
    {
      "requestId" :  String,
      "recipientSeq" : Integer,
      "requestDate" :  String,
      "createDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" :  String,
      "resendRequestId" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String
    }
  ]
}
```

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|messages|	List|	List of messages|
|- requestId | String |	Request ID |
|- recipientSeq | Integer |	Recipient sequence number |
|- requestDate | String |	Date and time of request |
|- createDate  | String |	Date and time of creation |
|- receiveDate | String |	Date and time of receiving |
|- resendStatus | String |	Status code of resending(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below) |
|- resendStatusName | String |	Status code name of resending |
|- resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api) |
|- resendRequestId | String | ID requesting of resending SMS |
|- messageStatus | String |	Request status(COMPLETED -> Successful, FAILED -> Failed, CANCEL -> Canceled) |
|- resultCode | String |	Result code of receiving |
|- resultCodeName | String |	Result code name of receiving |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### Status Code of SMS/LMS Resending
| Name |	Description|
|---|---|
|RSC01|	No target of resending|
|RSC02|	Target of resending(If sending fails, resending is performed.)|
|RSC03|	Resending in progress|
|RSC04|	Resending successful|
|RSC05|	Resending failed|

## Mass Delivery
### List Mass Delivery Requests

#### Request
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appKey|	String|	Unique appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
|X-Secret-Key|	String|	Unique secret key |

[Query parameter]
* One of the following is required: requestId, startRequestDate + endRequestDate, or startCreateDate + endCreateDate.

| Name |	Type| Max. Length |	Required|	Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | O | Start date of delivery |
| endRequestDate | String | - | O | End date of delivery |
| startCreateDate |	String| - |	O |	Start date of registration |
| endCreateDate |	String| - |	O |	End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
```
{
  "header": {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
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

| Name |	Type|	Description|
|---|---|---|
| header | Object |	Header area |
| - resultCode |	Integer |	Result code |
| - resultMessage |	String | Result message |
| - isSuccessful |	Boolean | Successful or not |
| body | Object | Body area |
| - messages | Object | List of messages |
| -- requestId | String | Request ID |
| -- requestDate | String | Date of request |
| -- createDate | String | Date of creation |
| -- createUser | String | Date of creation |
| -- plusFriendId | String | PlusFriend ID |
| -- senderKey | String| Sender key(40 characters) |
| -- masterStatusCode | String | Mass delivery status code(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | Content |
| -- fileId | String | Attachment ID |
| -- templateCode |	String | Template code(up to 20 characters) |
| -- autoSendYn | String | Auto sending or not |
| -- statsId | String | Statistics ID |
| -- createDate | String | Date of creation |
| -- createUser | String | User who created the request(saved as user UUID when sending from console) |
| - totalCount | Integer | Total count |


### List Mass Delivery Recipients

#### Request
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
| appKey |	String |	Unique appkey |
| requestId |	String |	Request ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
| X-Secret-Key |	String|	Unique secret key |


| Name |	Type| Max. Length |	Required|	Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate |	String| - |	X |	Start date of registration |
| endCreateDate |	String| - |	X |	End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
```
{
    "header": {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
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

| Name |	Type|	Description|
|---|---|---|
| header | Object |	Header area |
| - resultCode |	Integer |	Result code |
| - resultMessage |	String | Result message |
| - isSuccessful |	Boolean | Successful or not |
| body | Object | Body area |
| - messages | Object | List of messages |
| -- requestId | String | Request ID |
| -- recipientSeq | String | Recipient sequence number |
| -- recipientNo | String | Recipient number |
| -- requestDate | String | Date of request |
| -- receiveDate | String | Date of receiving |
| -- messageStatus | String | Message status |
| -- resultCode | String | Result code |
| -- resultCodeName | String | Result code content |
| - totalCount | Integer | Total count |

### Get a Mass Delivery Recipient

#### Request
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
| appKey |	String | Unique appkey |
| requestId |	String | Request ID |
| recipientSeq | String | Recipient sequence |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
|X-Secret-Key|	String|	Unique secret key |


| Name |	Type| Max. Length |	Required|	Description|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate |	String| - |	X |	Start date of registration |
| endCreateDate |	String| - |	X |	End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/1" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
```
{
    "header": {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
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
        "templateHeader" : String,
        "templateItem" : {
          "list" : [{
            "title": String,
            "description": String
          }],
          "summary" : {
            "title": String,
            "description": String
          }
        },
        "templateItemHighlight" : {
          "title": String,
          "description": String,
          "imageUrl": String
        },
        "templateRepresentLink" : {
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
        "buttons" : [
          {
            "ordering" :  Integer,
            "type" :  String,
            "name" :  String,
            "linkMo" :  String,
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
            "target": String
          }
        ],
        "quickReplies" : [
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
    }
}
```

| Name |	Type| 	Description                                                                                                                                                                                                      |
|---|---|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header | Object | 	Header area                                                                                                                                                                                                      |
| - resultCode |	Integer | 	Result code                                                                                                                                                                                                      |
| - resultMessage |	String | Result message                                                                                                                                                                                                    |
| - isSuccessful |	Boolean | Successful or not                                                                                                                                                                                                 |
| body | Object | Body area                                                                                                                                                                                                         |
| - requestId | String | Request ID                                                                                                                                                                                                        |
| - recipientSeq | String | Recipient sequence number                                                                                                                                                                                         |
| - plusFriendId | String | PlusFriend ID                                                                                                                                                                                                     |
| - senderKey | String | Sender ID                                                                                                                                                                                                         |
| - templateCode |	String | Template code(up to 20 characters)                                                                                                                                                                                |
| - recipientNo | String | Recipient number                                                                                                                                                                                                  |
| - content | String | Content                                                                                                                                                                                                           |
| - tempalteTitle| String | Template title(No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters)              |
| - templateSubtitle| String | Auxiliary template phrase(No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters)                                                 |
| - templateExtra | String | Additional template information(Required, if template message type is[Ad Included/Mixed Purposes])                                                                                                                |
| - templateAd | String | Request for consent of receiving within template or simple ad phrases                                                                                                                                             |
| - templateHeader| String|  Template header(up to 16 characters) |
| - templateItem | Object |  Item |
| -- list | List | Item list(at least 2, up to 10) |
| --- title | String | Title(up to 6 characters) |
| --- description | String |  Description(up to 23 characters) |
| -- summary | Object |  Item summary information |
| --- title | String |  Title(up to 6 characters) |
| --- description | String |  Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
| - templateItemHighlight | Object |  Item highlight |
| --- title | String |  Title(up to 30 characters, 21 characters with a thumbnail image) |
| --- description | String |  Description(up to 19 characters, 13 with a thumbnail image) |
| --- imageUrl | String |  Thumbnail image address |
| - templateRepresentLink | Object |  Main link |
| -- linkMo| String | 	Mobile web link(up to 500 characters)|
| -- linkPc | String | PC web link(up to 500 characters) |
| -- schemeIos | String |	iOS app link(up to 500 characters) |
| -- schemeAndroid | String | 	Android app link(up to 500 characters) |
| - requestDate | String | Date of request                                                                                                                                                                                                   |
| - receiveDate | String | Date of receiving                                                                                                                                                                                                 |
| - createDate | String | Date of creation                                                                                                                                                                                                  |
| - resendStatus | String | Status code of resending(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([Refer to [Status code of resending table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-api-guide/#smslms)] below)    |
| - resendStatusName | String | Status name of resending                                                                                                                                                                                          |
| - resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/en/Notification/SMS/en/error-code/#api)                                                                                              |
| - resendRequestId | String | Resending SMS request ID                                                                                                                                                                                          |
| - messageStatus | String | Mass recipient delivery status code(READY, COMPLETED, FAILED, CANCEL)                                                                                                                                             |
| - resultCode | String | Result status code                                                                                                                                                                                                |
| - resultCodeName | String | Result status name                                                                                                                                                                                                |
| - createUser | String | User who created the request(saved as user UUID when sending from console)                                                                                                                                        |
| - buttons | List | 	List of buttons                                                                                                                                                                                                  |
| -- ordering | Integer | 	Button sequence                                                                                                                                                                                                  |
| -- type| String |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
| -- name | String | 	Button name                                                                                                                                                                                                      |
| -- linkMo | String | 	Mobile web link(required for the WL type)                                                                                                                                                                        |
| -- linkPc | String | 	PC web link(optional for the WL type)                                                                                                                                                                            |
| -- schemeIos | String | 	iOS app link(required for the AL type)                                                                                                                                                                           |
| -- schemeAndroid | String | 	Android app link(required for the AL type)                                                                                                                                                                       |
| -- chatExtra|	String| 	Meta information to send for BC(Bot for Consultation) or BT(Bot Transfer) type buttons                                                                                                                           |
| -- chatEvent|	String| Bot event name to connect for BT(Bot Transfer) type button                                                                                                                                                        |
| -- bizFormId|	Integer| 	Business form ID(required for BF type)                                                                                                                                                                           |
| -- pluginId|	String| 	Plugin ID(up to 24 characters)                                                                                                                                                                                   |
| -- relayId|	String| Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed                                                                                                                                   |
| -- oneClickId|	String| Payment information used in the one click payment plugin                                                                                                                                                          |
| -- productId|	String| Payment information used in the one click payment plugin                                                                                                                                                          |
| -- target|	String| 	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                        |
| - quickReplies|	List | Quick reply list(up to 5)                                                                                                                                                                                         |
| -- ordering|	Integer| 	Quick reply order(required  when quick reply exists)                                                                                                                                                             |
| -- type| String | 	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                     |
| -- name| String | 	Quick reply name(required  when quick reply exists, up to 14 characters)                                                                                                                                         |
| -- linkMo| String | 	Mobile web link(required for the WL type, up to 500 characters)                                                                                                                                                  |
| -- linkPc | String | PC web link(optional for the WL type, up to 500 characters)                                                                                                                                                       |
| -- schemeIos | String | 	iOS app link(required for the AL type, up to 500 characters) |
| -- schemeAndroid | String |	Android app link(required for the AL type, up to 500 characters) |
| -- bizFormId|	String| 	Plugin ID(up to 24 characters) |
| -- target|	String| 	In the case of a web link type, out link used when adding "target":"out" attribute<br>Send with the default in-app link |

## Templates

### List Template Categories
#### Request
[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|categories|	List|	List of categories |
|- name | String | Category name |
|- subCategories | List |	List of subcategories |
|-- code | String | Category code(Used when registering/modifying templates) |
|-- name | String |	Category name |
|-- groupName | String |	Category group name |
|-- inclusion | String |	Description of templates to which the category applies |
|-- exclusion| String| Description of templates to which the category does not apply |

### Register Templates
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |
|senderKey|	String|	Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "templateCode" : String,
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateHeader" : String,
  "templateItem" : {
    "list" : [{
      "title": String,
      "description": String
    }],
    "summary" : {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight" : {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink" : {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
      "bizFormId" : Integer,
      "pluginId" : String
    }
  ],
  "quickReplies" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
      "bizFormId" : Integer
    }
  ]
}
```

| Name                  |	Type|	Required|	Description|
|-----------------------|---|---|---|
| templateCode          |	String |	O | Template code(up to 20 characters) |
| templateName          |	String |	O | Template name(up to 150 characters) |
| templateContent       |	String |	O | Template body(up to 1000 characters) |
| templateMessageType   | String | X |Types of template message(BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
| templateEmphasizeType | String| X| Types of emphasized template(NONE: Basic, TEXT: Emphasized, IMAGE: Image type, ITEM_LIST: 아이템리스트형, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required<br>IMAGE: templateImageName and templateImageUrl fields are required <br>ITEM_LIST: 이미지, 헤더, 아이템 하이라이트, 아이템 리스트 중 1개 이상 필수|
| templateExtra         | String | X | Additional template information(Required, if template message type is[Ad Included/Mixed Purposes]) |
| tempalteTitle         | String | X| Template title(No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
| templateSubtitle      | String | X| Auxiliary template phrase(No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| templateHeader        | String| X| Template header(up to 16 characters) |
| templateItem          | Object | X| Item |
| - list                | List | X | Item list(at least 2, up to 10) |
| -- title              | String | X | Title(up to 6 characters) |
| -- description        | String | X | Description(up to 23 characters) |
| - summary             | Object | X | Item summary information |
| -- title              | String | X | Title(up to 6 characters) |
| -- description        | String | X | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
| templateItemHighlight | Object | X| Item highlight |
| - title               | String | X | Title(up to 30 characters, 21 characters with a thumbnail image) |
| - description         | String | X | Description(up to 19 characters, 13 with a thumbnail image) |
| - imageUrl            | String | X | Thumbnail image address |
| templateRepresentLink | Object | X| Main link |
| - linkMo              | String |	X |	Mobile web link(up to 500 characters)|
| - linkPc              | String |	X |PC web link(up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(up to 500 characters) |
| templateImageName     | String |	X | Image name(name of uploaded file) |
| templateImageUrl      | String |	X | Image URL |
| securityFlag          | Boolean | X| Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending(default: false) |
| categoryCode          | String | X | Template category code(Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
| buttons               |	List |	X | List of buttons(up to 5) |
| -ordering             |	Integer |	X | Button sequence(1~5) |
| - type                | String |	X |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
| - name                | String |	X |	Button name(required, if there's a button, up to 14 characters)|
| - linkMo              | String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
| - linkPc              | String |	X |PC web link(optional for the WL type, up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(required for the AL type, up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(required for the AL type, up to 500 characters) |
| - bizFormId           |	Integer|	X |	Business form ID(required for BF type) |
| - pluginId            |	String|	X |	Plugin ID(up to 24 characters) |
| quickReplies          |	List |	X | Quick reply list(up to 5) |
| - ordering            |	Integer|	X |	Quick reply order(required  when quick reply exists)|
| - type                | String |	X |	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form) |
| - name                | String |	X |	Quick reply name(required  when quick reply exists, up to 14 characters)|
| - linkMo              | String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
| - linkPc              | String |	X |PC web link(optional for the WL type, up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(required for the AL type, up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(required for the AL type, up to 500 characters) |
| - bizFormId           |	Integer|	X |	Business form ID(required for BF type) |

* The templateAd value is fixed when registering the AD included(AD) or Mixed Purposes(MI) message type template.
* The Add Chanel button must be in the first when registering the AD included(AD) or Mixed Purposes(MI) message type template.
* The Add Channel(AC) button must be registered with a fixed button name of "Add Channel".


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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Modify Templates
#### Request
[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateHeader" : String,
  "templateItem" : {
    "list" : [{
      "title": String,
      "description": String
    }],
    "summary" : {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight" : {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink" : {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
      "bizFormId" : Integer,
      "pluginId" : String
    }
  ],
  "quickReplies" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
      "bizFormId" : Integer
    }
  ]
}
```

| Name                  |	Type|	Required|	Description|
|-----------------------|---|---|---|
| templateName          |	String |	O | Template name(up to 150 characters) |
| templateContent       |	String |	O | Template body(up to 1000 characters) |
| templateMessageType   | String | X | Types of template message(BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic) |
| templateEmphasizeType | String| X| Types of emphasized template(NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required<br>IMAGE: templateImageName and templateImageUrl fields are required|
| templateExtra         | String | X | Additional template information(Required, if template message type is[Ad Included/Mixed Purposes]) |
| tempalteTitle         | String | X| Template title(No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
| templateSubtitle      | String | X| Auxiliary template phrase(No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| templateHeader        | String| X| Template header(up to 16 characters) |
| templateItem          | Object | X| Item |
| - list                | List | X | Item list(at least 2, up to 10) |
| -- title              | String | X | Title(up to 6 characters) |
| -- description        | String | X | Description(up to 23 characters) |
| - summary             | Object | X | Item summary information |
| -- title              | String | X | Title(up to 6 characters) |
| -- description        | String | X | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
| templateItemHighlight | Object | X| Item highlight |
| - title               | String | X | Title(up to 30 characters, 21 characters with a thumbnail image) |
| - description         | String | X | Description(up to 19 characters, 13 with a thumbnail image) |
| - imageUrl            | String | X | Thumbnail image address |
| templateRepresentLink | Object | X| Main link |
| - linkMo              | String |	X |	Mobile web link(up to 500 characters)|
| - linkPc              | String |	X |PC web link(up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(up to 500 characters) |
| templateImageName     | String |	X | Image name(name of uploaded file) |
| templateImageUrl      | String |	X | Image URL |
| securityFlag          | Boolean | X| Security template<br>Set for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending(default: false) |
| categoryCode          | String | X | Template category code(Refer to API to View Template Category, default: 999999)<br>For other categories, screened by the lowest priority. |
| buttons               |	List |	X | List of buttons(up to 5) |
| - ordering            |	Integer |	X | Button sequence(1~5) |
| - type                | String |	X |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
| - name                | String |	X |	Button name(required, if there's a button, up to 14 characters)|
| - linkMo              | String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
| - linkPc              | String |	X |PC web link(optional for the WL type, up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(required for the AL type, up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(required for the AL type, up to 500 characters) |
| - bizFormId           |	Integer|	X |	Business form ID(required for BF type) |
| - pluginId            |	String|	X |	Plugin ID(up to 24 characters) |
| quickReplies          |	List |	X | Quick reply list(up to 5) |
| - ordering            |	Integer|	X |	Quick reply order(required  when quick reply exists)|
| - type                | String |	X |	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form) |
| - name                | String |	X |	Quick reply name(required  when quick reply exists, up to 14 characters)|
| - linkMo              | String |	X |	Mobile web link(required for the WL type, up to 500 characters)|
| - linkPc              | String |	X |PC web link(optional for the WL type, up to 500 characters) |
| - schemeIos           | String | X |	iOS app link(required for the AL type, up to 500 characters) |
| - schemeAndroid       | String | X |	Android app link(required for the AL type, up to 500 characters) |
| - bizFormId           |	Integer|	X |	Business form ID(required for BF type) |

* The templateAd value is fixed when modifying the AD included(AD) or Mixed Purposes(MI) message type template.
* The Add Chanel button must be in the first when modifying the AD included(AD) or Mixed Purposes(MI) message type template.
* The Add Channel(AC) button must be modified with a fixed button name of "Add Channel".

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Delete Templates
#### Request
[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
* When an approved template is deleted, it is only deleted within NHN Cloud.(Only templates that have not been sent for 3 days can be deleted.)
* In the case of an approved template, Kakao's internal data cannot be deleted due to the restrictions of KakaoTalk BizMessage.
* A template remaining in Kakao becomes dormant if it is not used for 1 year, and gets deleted if it remains dormant for 1 year.(If a template becomes dormant or gets deleted on Kakao, the person in charge will be notified.)


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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Inquire of Templates
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "comment" : String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|comment|	String |	O | Inquiries |

* When commenting a template in the REJ status, it will be changed to the REQ status.

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Attach files to send inquiry on templates
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "comment" : String,
  "attachments" : File
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|comment|	String |	O | Inquiries |
|attachments| List<File> | X | List of Attachment(Up to 5) |

* When commenting a template in the REJ status, it will be changed to the REQ status.

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### List Templates

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|templateCode|	String|	X |	Template code|
|templateName|	String|	X |	Template name|
|templateStatus| String |	X | Template status code|
|pageNum|	Integer|	X|	Page number(default:1)|
|pageSize|	Integer|	X|	Number of queries(default: 15, Max: 1000)|

|Template status code| Description|
|---|---|
| TSC01 | Request |
| TSC02 | Inspecting |
| TSC03 | Approved |
| TSC04 | Rejected |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates?templateStatus={템플릿 상태 코드}"
```

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "templateListResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateMessageType" : String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra" : String,
              "templateAd" : String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateHeader" : String,
              "templateItem" : {
                "list" : [{
                  "title": String,
                  "description": String
                }],
                "summary" : {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight" : {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink" : {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName" : String,
              "templateImageUrl" : String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
                }
              ],
              "quickReplies" : [
                {
                  "ordering" : Integer,
                  "type" : String,
                  "name" : String,
                  "linkMo" : String,
                  "linkPc" : String,
                  "schemeIos" : String,
                  "schemeAndroid" : String
                  "bizFormId" : Integer
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

| Name                     |	Type| 	Description                                                                                                                                                                                                                                                                                           |
|--------------------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                   |	Object| 	Header area                                                                                                                                                                                                                                                                                           |
| - resultCode             |	Integer| 	Result code                                                                                                                                                                                                                                                                                           |
| - resultMessage          |	String| Result message                                                                                                                                                                                                                                                                                         |
| - isSuccessful           |	Boolean| Successful or not                                                                                                                                                                                                                                                                                      |
| templateListResponse     |	Object| 	Body area                                                                                                                                                                                                                                                                                             |
| - templates              | List | 	Template list                                                                                                                                                                                                                                                                                         |
| -- plusFriendId          | String | 	PlusFriend ID                                                                                                                                                                                                                                                                                         |
| -- senderKey             | String | Sender Key                                                                                                                                                                                                                                                                                             |
| -- plusFriendType        | String | PlusFriend type(NORMAL, GROUP)                                                                                                                                                                                                                                                                         |
| -- templateCode          | String | 	Template code                                                                                                                                                                                                                                                                                         |
| -- kakaoTemplateCode     | String | 	Kakao's origin template code                                                                                                                                                                                                                                                                          |
| -- templateName          | String | 	Template name                                                                                                                                                                                                                                                                                         |
| -- templateMessageType   | String | Types of template message(BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes)                                                                                                                                                                                                       |
| -- templateEmphasizeType | String| Types of emphasized template(NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE)                                                                                                                                                                                                           |
| -- templateContent       | String | 	Template body                                                                                                                                                                                                                                                                                         |
| -- templateExtra         | String | Additional template information                                                                                                                                                                                                                                                                        |
| -- templateAd            | String | Request for consent of receiving within template or simple ad phrases                                                                                                                                                                                                                                  |
| -- tempalteTitle         | String | Template title                                                                                                                                                                                                                                                                                         |
| -- templateSubtitle      | String | Auxiliary template phrase                                                                                                                                                                                                                                                                              |
| - templateHeader         | String| Template header(up to 16 characters)                                                                                                                                                                                                                                                                   |
| - templateItem           | Object | Item                                                                                                                                                                                                                                                                                                   |
| -- list                  | List | Item list(at least 2, up to 10)                                                                                                                                                                                                                                                                        |
| --- title                | String | Title(up to 6 characters)                                                                                                                                                                                                                                                                              |
| --- description          | String | Description(up to 23 characters)                                                                                                                                                                                                                                                                       |
| -- summary               | Object | Item summary information                                                                                                                                                                                                                                                                               |
| --- title                | String | Title(up to 6 characters)                                                                                                                                                                                                                                                                              |
| --- description          | String | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                                                                                                      |
| - templateItemHighlight  | Object | Item highlight                                                                                                                                                                                                                                                                                         |
| -- title                 | String | Title(up to 30 characters, 21 characters with a thumbnail image)                                                                                                                                                                                                                                       |
| -- description           | String | Description(up to 19 characters, 13 with a thumbnail image)                                                                                                                                                                                                                                            |
| -- imageUrl              | String | Thumbnail image address                                                                                                                                                                                                                                                                                |
| -templateRepresentLink   | Object | Main link                                                                                                                                                                                                                                                                                              |
| -- linkMo                | String | 	Mobile web link(up to 500 characters)                                                                                                                                                                                                                                                                 |
| -- linkPc                | String | PC web link(up to 500 characters)                                                                                                                                                                                                                                                                      |
| -- schemeIos             | String | 	iOS app link(up to 500 characters)                                                                                                                                                                                                                                                                    |
| -- schemeAndroid         | String | 	Android app link(up to 500 characters)                                                                                                                                                                                                                                                                |
| -- templateImageName     | String | Image name(name of uploaded file)                                                                                                                                                                                                                                                                      |
| -- templateImageUrl      | String | 	Image URL                                                                                                                                                                                                                                                                                             |
| -- buttons               | List | 	List of buttons                                                                                                                                                                                                                                                                                       |
| --- ordering             | Integer | 	Button sequence(1~5)                                                                                                                                                                                                                                                                                  |
| --- type                 | String | 	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
| --- name                 | String | 	Button name                                                                                                                                                                                                                                                                                           |
| --- linkMo               | String | 	Mobile web link(required for the WL type)                                                                                                                                                                                                                                                             |
| --- linkPc               | String | 	PC web link(optional for the WL type)                                                                                                                                                                                                                                                                 |
| --- schemeIos            | String | 	iOS app link(required for the AL type)                                                                                                                                                                                                                                                                |
| --- schemeAndroid        | String | 	Android app link(required for the AL type)                                                                                                                                                                                                                                                            |
| --- bizFormId            |	Integer| 	X                                                                                                                                                                                                                                                                                                     |	Business form ID(required for BF type) |
| --- pluginId             |	String| 	X                                                                                                                                                                                                                                                                                                     |	Plugin ID(up to 24 characters) |
| -- quickReplies          |	List | 	X                                                                                                                                                                                                                                                                                                     | Quick reply list(up to 5) |
| --- ordering             |	Integer| 	X                                                                                                                                                                                                                                                                                                     |	Quick reply order(required  when quick reply exists)|
| --- type                 | String | 	X                                                                                                                                                                                                                                                                                                     |	Qucik reply type(WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form) |
| --- name                 | String | 	X                                                                                                                                                                                                                                                                                                     |	Quick reply name(required  when quick reply exists, up to 14 characters)|
| --- linkMo               | String | 	X                                                                                                                                                                                                                                                                                                     |	Mobile web link(required for the WL type, up to 500 characters)|
| --- linkPc               | String | 	X                                                                                                                                                                                                                                                                                                     |PC web link(optional for the WL type, up to 500 characters) |
| --- schemeIos            | String | X                                                                                                                                                                                                                                                                                                      |	iOS app link(required for the AL type, up to 500 characters) |
| --- schemeAndroid        | String | X                                                                                                                                                                                                                                                                                                      |	Android app link(required for the AL type, up to 500 characters) |
| --- bizFormId            |	Integer| 	X                                                                                                                                                                                                                                                                                                     |	Business form ID(required for BF type) |
| -- comments              | List | Inspection result                                                                                                                                                                                                                                                                                      |
| --- id                   | Integer | Inquiry ID                                                                                                                                                                                                                                                                                             |
| --- content              |  String | Inquiries                                                                                                                                                                                                                                                                                              |
| --- userName             | String | Creator                                                                                                                                                                                                                                                                                                |
| --- createAt             | String | Date of registration                                                                                                                                                                                                                                                                                   |
| --- attachment           | List | Attachment                                                                                                                                                                                                                                                                                             |
| ---- originalFileName    | String | Attachment file name                                                                                                                                                                                                                                                                                   |
| ---- filePath            | String | Attachment file path                                                                                                                                                                                                                                                                                   |
| --- status               | String | Comment status(INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied)                                                                                                                                                                                                                              |
| -- status                | String | Template status                                                                                                                                                                                                                                                                                        |
| -- statusName            | String | Template status name                                                                                                                                                                                                                                                                                   |
| -- securityFlag          | Boolean | Whether it is a security template                                                                                                                                                                                                                                                                      |
| -- categoryCode          | String | Template category code                                                                                                                                                                                                                                                                                 |
| -- createDate            | String | Date of creation                                                                                                                                                                                                                                                                                       |
| -- updateDate            | String | Date of modification                                                                                                                                                                                                                                                                                   |
| - totalCount             | Integer | Total count                                                                                                                                                                                                                                                                                            |

### List Template modifications

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|templateCode|	String|	Template code |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

#### Response
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "templateModificationsResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateMessageType" : String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra" : String,
              "templateAd" : String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateHeader" : String,
              "templateItem" : {
                "list" : [{
                  "title": String,
                  "description": String
                }],
                "summary" : {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight" : {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink" : {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName" : String,
              "templateImageUrl" : String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|templateModificationsResponse|	Object|	Body area|
|- templates | List |	Template list |
|-- plusFriendId | String |	PlusFriend ID |
|-- senderKey    | String | Sender Key    |
|-- plusFriendType | String | PlusFriend type(NORMAL, GROUP) |
|-- templateCode | String |	Template code |
|-- templateName | String |	Template name |
|-- templateMessageType| String | Types of template message(BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes) |
|-- templateEmphasizeType| String| Types of emphasized template(NONE: Basic, TEXT: Emphasized, IMAGE: Image type, default:NONE) |
|-- templateContent | String |	Template body |
|-- templateExtra | String | Additional template information |
|-- templateAd | String | Request for consent of receiving within template or simple ad phrases |
|-- tempalteTitle| String | Template title |
|-- templateSubtitle| String | Auxiliary template phrase |
|- templateHeader| String| Template header(up to 16 characters) |
|- templateItem | Object | Item |
|-- list | List | Item list(at least 2, up to 10) |
|--- title | String | Title(up to 6 characters) |
|--- description | String | Description(up to 23 characters) |
|-- summary | Object | Item summary information |
|--- title | String | Title(up to 6 characters) |
|--- description | String | Description(Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
|- templateItemHighlight | Object | Item highlight |
|-- title | String | Title(up to 30 characters, 21 characters with a thumbnail image) |
|-- description | String | Description(up to 19 characters, 13 with a thumbnail image) |
|-- imageUrl | String | Thumbnail image address |
|-templateRepresentLink | Object | Main link |
|-- linkMo| String |	Mobile web link(up to 500 characters)|
|-- linkPc | String |PC web link(up to 500 characters) |
|-- schemeIos | String |	iOS app link(up to 500 characters) |
|-- schemeAndroid | String |	Android app link(up to 500 characters) |
|-- templateImageName | String | Image name(name of uploaded file) |
|-- templateImageUrl | String |	Image URL |
|-- buttons | List |	List of buttons |
|--- ordering | Integer |	Button sequence(1~5) |
|--- type| String |	Button type(WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID) |
|--- name | String |	Button name |
|--- linkMo | String |	Mobile web link(required for the WL type) |
|--- linkPc | String |	PC web link(optional for the WL type) |
|--- schemeIos | String |	iOS app link(required for the AL type) |
|--- schemeAndroid | String |	Android app link(required for the AL type) |
|-- comments | List | Inspection result |
|--- id | Integer | Inquiry ID |
|--- content |  String | Inquiries |
|--- userName | String | Creator |
|--- createAt | String | Date of registration |
|--- attachment | List | Attachment |
|---- originalFileName | String | Attachment file name |
|---- filePath | String | Attachment file path |
|--- status | String | Comment status(INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied) |
|-- status| String | Template status |
|-- statusName | String | Template status name |
|-- securityFlag| Boolean | Whether it is a security template |
|-- categoryCode| String | Template category code  |
|-- activated | Boolean | Activated or not |
|-- createDate | String | Date of creation |
|-- updateDate | String | Date of modification |
|- totalCount | Integer | Total count |

### Register Template Image
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|file|	File|	O |	Template image file |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

#### Response
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|templateImage|	Object|	Body area|
|- templateImageName | String |	Image name(name of uploaded file) |
|- templateImageUrl | String |	Image URL |

### Register Template Item Highlight Images
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|file|	File|	O |	Template image file |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight" -F "file=@alimtalk-template-image.jpeg"
```

#### Response
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|templateImage|	Object|	Body area|
|- templateImageName | String |	Image name(name of uploaded file) |
|- templateImageUrl | String |	Image URL |

### Register Template Plugin
#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "pluginType" : String,
  "pluginId" : String,
  "callbackUrl" : String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|pluginType|	String |	O | Plugin type(SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
|pluginId|	String |	O | Plugin ID |
|callbackUrl|	String |	O | The callback URL to receive when the plugin button is clicked |

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Modify Template Plugin
#### Request
[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|pluginId|	String|	Plugin ID |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

[Request Body]

```
{
  "pluginType" : String,
  "callbackUrl" : String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|pluginType|	String |	O | Plugin type(SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
|callbackUrl|	String |	O | The callback URL to receive when the plugin button is clicked |

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Modify Template Plugin
#### Request
[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |
|pluginId|	String|	Plugin ID |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

### Retrieve Template Plugin
#### Request
[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |

#### Response
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "plugins" : [
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

| Name |	Type|	Description|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|plugins|	List |	Plugin list |
|- pluginId|	String|	Plugin ID|
|- pluginType|	String| Plugin type(SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
|- pluginTypeName|	String| Plugin name |
|- callbackUrl|	String|	The callback URL to receive when the plugin button is clicked |
|- modifiable|	Boolean| Modifiable |
|- deletable|	Boolean| Deletable |

## Manage Alternative Delivery
### Register an SMS AppKey

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)]  |


[Request body]

```
{
    "resendAppKey": String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|resendAppKey|	String|	O | SMS service appkey to set for alternative delivery |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

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

### Register Alternative Delivery Settings

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)  |


[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|senderKey|	String|	O | Sender Key |
|isResend|	Boolean|	O | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console. |
|resendSendNo|	String|	O | Sender number for alternative delivery<br><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

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
