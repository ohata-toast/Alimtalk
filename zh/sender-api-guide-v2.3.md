## Notification > KakaoTalk Bizmessage > Sender > API v2.3 Guide

## Overview of v2.3 API
#### What's the diffrence
1. 발신프로필 조회 API에 최초 사용자 제한 상태 필드가 추가되었습니다.
2. 발신프로필 조회 API에 카카오톡 채널 스팸 상태, 카카오톡 메시지 스팸 상태 필드가 추가되었습니다. 

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

## Senders

### Query Sender by Category

#### Request
[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/sender/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

#### Response
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "categories" : [
  {
      "parentCode" : String,
      "depth" : Integer,
      "code" : String,
      "name" : String,
      "subCategories" : [
        {
        "parentCode" : String,
        "depth" : Integer,
        "code" : String,
        "name" : String,
        "subCategories" : [
          {
            "parentCode" : String,
            "depth" : Integer,
            "code" : String,
            "name" : String
          }
          ]
        }
      ]
    }
  ]
}
```

| Value            | Type    | Description       |
| ---------------- | ------- | ----------------- |
| header           | Object  | Header area       |
| - resultCode     | Integer | Result code       |
| - resultMessage  | String  | Result message    |
| - isSuccessful   | Boolean | Successful or not |
| categories       | Object  | Category          |
| - parentCode     | String  | Parent code       |
| - depth          | Integer | Depth of category |
| - code           | String  | Category code     |
| - name           | String  | Category name     |
| - subCategories  | Object  | Sub-category      |
| -- parentCode    | String  | Parent code       |
| -- depth         | Integer | Depth of category |
| -- code          | String  | Category code     |
| -- name          | String  | Category name     |
| -- subCategories | Object  | Sub-category      |
| --- parentCode   | String  | Parent code       |
| --- depth        | Integer | Depth of category |
| --- code         | String  | Category code     |
| --- name         | String  | Category name     |

### Register Senders

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "categoryCode" : String
}
```

| Value        | Type    | Required | Description                                                  |
| ------------ | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId | String  | O        | PlusFriend ID(up to 30 characters)                          |
| phoneNo      | String  | O        | Mobile number of administrator(up to 15 characters)         |
| categoryCode | String  | O        | Category code(11 characters) See response for Search Category API  e.g.) 00100010001 Health(001) - Hospital(0001) - General Hospital(0001) |

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

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |

### Authenticate Tokens for Senders

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/sender/token
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Request Body]

```
{
  "plusFriendId" : String,
  "token" : "Integer"
}
```

| Value | Type    | Required | Description                                                  |
| ----- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId | String  | O | PlusFriend ID |
| token | Integer | O        | Authentication token(received on KakaoTalk app, after Register PlusFriend API call) |

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

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |

### Delete Sender
#### Request

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appkey       | String | Original appkey |
| senderKey    | String | Sender key      |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

* 발신 프로필 삭제 시, 등록한 템플릿 데이터가 함께 삭제 됩니다.
* 발신 프로필 삭제 시, 복구가 불가능합니다.

#### Response
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |

### Get Sender
#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| senderKey    | String | Sender key      |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

#### Response
```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "sender":{  
         "plusFriendId" : String,
         "senderKey" : String,
         "categoryCode" : String,
         "status" : String,
         "statusName" : String,
         "kakaoStatus" : String,
         "kakaoStatusName" : String,
         "kakaoProfileStatus" : String,
         "kakaoProfileStatusName" : String,
         "profileSpamLevel" : String,
         "profileMessageSpamLevel" : String,
         "alimtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         },
         "dormant": Boolean,
         "block": Boolean,
         "createDate": String,
         "initialUserRestriction" : Boolean
    }
}
```

| Value                     | Type    | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| header                    | Object  | Header area                                                  |
| - resultCode              | Integer | Result code                                                  |
| - resultMessage           | String  | Result message                                               |
| - isSuccessful            | Boolean | Successful or not                                            |
| sender                | Object  | Sender                                                   |
| - plusFriendId            | String  | PlusFriend ID                                                |
| - senderKey               | String  | Sender key                                                   |
| - categoryCode            | String  | Category code                                                |
| - status                  | String  | Status code of NHN Cloud PlusFriend(YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of NHN Cloud PlusFriend(ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend(A: Normal, S: Blocked) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend(normal, blocked) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile(A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile(Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
| - profileSpamLevel        | String | 카카오톡 채널 스팸 상태명(영구제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                            |
| - profileMessageSpamLevel | String | 카카오톡 메시지 스팸 상태명(활동제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                           |
|- alimtalk                 |	Object  |	AlimTalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily AlimTalk delivery count(no limits for 0)      |
|-- sentCount               | Integer |	Daily AlimTalk delivery count(no limits for 0)              |
|- friendtalk               |	Object  |	FriendTalk information                                       |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery           |
|-- dailyMaxCount           | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음)              |
|-- sentCount               | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음)                  |
| - dormant                 | Boolean |	Sender dormant or not                                        |
| - block                   | Boolean |	Sender block or not                                          |
| - createDate              | String  | Date and time of registration                                |
| - initialUserRestriction  | Boolean | 	최초 사용자 제한 여부                                         |
| totalCount                | Integer | Total count                                                  |

### List Sender

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

[Query parameter] No.1 or 2 is conditionally required

| Value               | Type    | Required | Description                                                  |
| ------------------- | ------- | -------- | ------------------------------------------------------------ |
| plusFriendId        | String  | X        | PlusFriend ID                                                |
| senderKey | String | X | Sender key |
| status              | String  | X        | Status code of PlusFriend(YSC02: Ready for token authenticated, YSC03: Normally registered) |
|pageNum|	Integer|	X|	page number(Default: 1)|
|pageSize|	Integer|	X|	page size(Default: 15, Max: 1000)|

#### Response

```
{  
   "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
   },
   "senders":[  
      {  
         "plusFriendId" : String,
         "senderKey" : String,
         "categoryCode" : String,
         "status" : String,
         "statusName" : String,
         "kakaoStatus" : String,
         "kakaoStatusName" : String,
         "kakaoProfileStatus" : String,
         "kakaoProfileStatusName" : String,
         "profileSpamLevel" : String,
         "profileMessageSpamLevel" : String,
         "alimtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
          },
         "friendtalk": {  
                "resendAppKey": String,
                "isResend": Boolean,
                "resendSendNo": String,
                "resendUnsubscribeNo": String,
                "dailyMaxCount" : Integer,
                "sentCount" : Integer
         },
         "dormant": Boolean,
         "block": Boolean,
         "createDate": String,
         "initialUserRestriction" : Boolean
      }
   ],
   "totalCount": Integer
}
```

| Value                     | Type    | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| header                    | Object  | Header area                                                  |
| - resultCode              | Integer | Result code                                                  |
| - resultMessage           | String  | Result message                                               |
| - isSuccessful            | Boolean | Successful or not                                            |
| senders               | List  | Sender                                                   |
| - plusFriendId            | String  | PlusFriend ID                                                |
| - senderKey               | String  | Sender key                                                   |
| - categoryCode            | String  | Category code                                                |
| - status                  | String  | Status code of NHN Cloud PlusFriend(YSC02: Ready for registeration, YSC03: Normally registered) |
| - statusName              | String  | Status name of NHN Cloud PlusFriend(ready for registration, normally registered) |
| - kakaoStatus             | String  | Status code of Kakao PlusFriend(A: Normal, S: Blocked) kakaoStatus is null if the status is YSC02. |
| - kakaoStatusName         | String  | Status name of Kakao PlusFriend(normal, blocked) kakaoStatusName is null if the status is YSC02. |
| - kakaoProfileStatus      | String  | Status code of Kakao PlusFriend profile(A: Activated, B: Blocked, C: Deactivated, D:Deleted, E: Deleting) kakaoProfileStatus is null if the status is YSC02. |
| - kakaoProfileStatusName  | String  | Status name of Kakao PlusFriend profile(Activated, Deactivated, Blocked, Deleted, or Deleting) kakaoProfileStatusName is null if the status is YSC02. |
| - profileSpamLevel        | String | 카카오톡 채널 스팸 상태명(영구제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                            |
| - profileMessageSpamLevel | String | 카카오톡 메시지 스팸 상태명(활동제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                           |
|- alimtalk                 |	Object  |	AlimTalk information                                         |
|-- resendAppKey            | String  | Alternative sms appkey                                       |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails       |
|-- resendSendNo            | String  |	Sender number for alternative delivery                       |
|-- dailyMaxCount           | Integer |	Maximum daily AlimTalk delivery count(no limits for 0)      |
|-- sentCount               | Integer |	Daily AlimTalk delivery count(no limits for 0)              |
|- friendtalk               |	Object  |	FriendTalk information                                        |
|-- resendAppKey            | String  | Alternative sms appkey                                        |
|-- isResend                | String  | Whether to send text as alternative, if delivery fails        |
|-- resendSendNo            | String  |	Sender number for alternative delivery                        |
|-- resendUnsubscribeNo     | String  |	080 unsubscription number for alternative delivery            |
|-- dailyMaxCount           | Integer |	친구톡 일별 최대 발송 건수<br>(값이 0일 경우 건수 제한없음)              |
|-- sentCount               | Integer |	친구톡 일별 발송 건수<br>(값이 0일 경우 건수 제한없음)                  |
| - dormant                 | Boolean |	Sender dormant or not                                        |
| - block                   | Boolean |	Sender block or not                                          |
| - createDate              | String  | Date and time of registration                                |
| - initialUserRestriction  | Boolean | 	최초 사용자 제한 여부                                         |
| totalCount                | Integer | Total count                                                  |

## Sender group

### Get Sender group

#### Request
[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/sender-groups/{groupSenderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| groupSenderKey    | String | Sender key of Sender group      |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

#### Response
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    },
    "senderGroup": {
        "groupName": String,
        "senderKey": String,
        "status": String,
        "senders": [
            {
                "plusFriendId": String,
                "senderKey": String,
                "createDate": String
            }
        ],
        "createDate": String,
        "updateDate": String
    }
}
```

| Value                     | Type    | Description                                                  |
|---|---|---|
| header                    | Object  | Header area                                                  |
| - resultCode              | Integer | Result code                                                  |
| - resultMessage           | String  | Result message                                               |
| - isSuccessful            | Boolean | Successful or not                                            |
|senderGroup|	Object|	Sender group |
|- groupName | String |	group name |
|- senderKey | String |	Sender key |
| - status                  | String  | Status code of NHN Cloud PlusFriend(YSC02: Ready for registeration, YSC03: Normally registered) |
|- senders | List |	Sender List |
|-- plusFriendId | String |	PlusFriend ID |
|-- senderKey | String |	Sender key |
|-- createDate | String | Date and time of registration |
|- createDate | String | Date and time of registration |
|- updateDate |	String|	Date and time of modification |

### Add sender to group

#### Request
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| groupSenderKey    | String | Sender key of Sender group      |
| senderKey    | String | Sender key   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

* The maximum number of members in a group is 5000.

#### Response
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |

### Delete sender from group

#### Request
[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/sender-groups/{groupSenderKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Value  | Type   | Description     |
| ------ | ------ | --------------- |
| appkey | String | Original appkey |
| groupSenderKey    | String | Sender key of Sender group      |
| senderKey    | String | Sender key   |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Value        | Type   | Required | Description                                                  |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| X-Secret-Key | String | O        | Can be created on console. [[Reference](./sender-console-guide/#x-secret-key)] |

#### Response
```
{
    "header":{  
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - resultCode    | Integer | Result code       |
| - resultMessage | String  | Result message    |
| - isSuccessful  | Boolean | Successful or not |
