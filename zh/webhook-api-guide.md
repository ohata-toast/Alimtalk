## Notification > KakaoTalk Bizmessage > Webhook > API Guide

<span id="webhook"></span>
## WebHook
When a specific event occurs within the KakaoTalk Bizmessage service, it generates a POST request to the URL defined in the webhook settings.<br>
API documentation for the generated POST request.

### Send Webhook

[URL]

|Http method|	URI|
|---|---|
| POST | The destination URL defined in the webhook settings |

[Header]

|Value|	Type|	Descriptions|
|---|---|---|
|X-Toast-Webhook-Signature|	String| The signature entered when webhook is configured |

[Request body]

|Value|	Type|	Descriptions|
|---|---|---|
|hooksId|	String| A unique ID created every time a POST request is sent to the URL specified by webhook settings |
|webhookConfigId|	String|Webhook setup ID|
|productName|	String|	The name of the service where a webhook event occurred |
|appKey|	String| The service appkey where the webhook event occurred |
|event|	String| Webhook event name |
|hooks|	List<Map> | Data when a webhook event occurs<br>\* For more information, see  [Hook definitions by event type](./webhook-api-guide/#event-hooks). |

#### cURL
```
curl -X POST \
    '{TargetUrl}' \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'X-Toast-Webhook-Signature: application/json;charset=UTF-8' \
    -H 'X-Secret-Key: '"${SECRET_KEY}"'' \
    -d '{
        "hooksId":"202007271010101010sadasdavas",
        "webhookConfigId":"String",
        "productName":"KakaoTalk Bizmessage",
        "appKey":"akb3dukdmdjsdSvgk",
        "event":"MESSAGE_RESULT_UPDATE",
        "hooks":[
            {
                ...
            }
        ]
    }'
```

<span id="event-hooks"></span>

### Definitions of hooks by event type
Hook data per event type when generating a POST request to the URL defined in the webhook settings.
#### Update template status/questions
|Value|	Type|	Descriptions|
|---|---|---|
|hooks|	List<Map> | Data when a webhook event occurs |
|- hookId|	String| A unique ID created when an event occurs in a service |
|- senderKey|	String|	Sender Key |
|- templateCode|	String| Template code |
|- kakaoTemplateCode|	String| Original template code |
|- status|	String| Template status (TSC01: Requested, TSC02: Reviewing, TSC03: Approved, TSC04: Rejected) |
|- comments|	List| Inspection result |
|-- id|	String| Inquiry ID|
|-- content|	String|Inquiries |
|-- userName|	String|Creator |
|-- createdAt|	String|Date of registration |
|-- attachment|	List|Attachment |
|--- originalFileName|	String|Attachment file name |
|--- filePath|	String|Attachment file path |
|-- status|	String| Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied) |
|- updateDate|	String| Modification date |

```json
"hooks": [
    {
      "senderKey": "String",
      "templateCode": "String",
      "kakaoTemplateCode": "String",
      "status": "String",
      "comments": [
        {
          "id": "Integer",
          "content": "String",
          "userName": "String",
          "createdAt": "2022-05-01 00:00:00",
          "attachment": [
            {
              "originalFileName": "String",
              "filePath": "String"
            }
          ],
          "status": "String"
        }
      ],
      "updateDate": "2022-05-01 00:00:00",
      "hookId": "String"
    }
]
```

#### Update the message sending result code
|Value|	Type|	Descriptions|
|---|---|---|
|hooks|	List<Map> | Data when a webhook event occurs |
|- kakaoMessageType|	String| Kakao Message Types<br>ALIMTALK_NORMAL<br>ALIMTALK_AUTH<br>ALIMTALK_MASS<br>FRIENDTALK_NORMAL<br>FRIENDTALK_MASS  |
|- requestId|	String| Request ID |
|- recipientSeq|	Integer| Recipient sequence number |
|- requestDate|	String| Date and time of request |
|- createDate|	String| Date and time of creation |
|- receiveDate|	String| Date and time of receiving |
|- recipientNo|	String| Recipient number |
|- resultCode|	String| Result code of receiving |
|- senderGroupingKey|	String| Sender's grouping key |
|- recipientGroupingKey|	String| Recipient's grouping key |
|- _links|	Object|	Link |
|- self|	Object|	- |
|- href|	String|	Query Message API link |
|- hookId|	String| A unique ID created when an event occurs in a service |

```json
"hooks": [
  {
     "kakaoMessageType": "String(ALIMTALK_NORMAL / ALIMTALK_AUTH / ALIMTALK_MASS / FRIENDTALK_NORMAL / FRIENDTALK_MASS)",
    "requestId": "String",
     "recipientSeq": "Integer",
     "requestDate": "String",
     "createDate": "String",
     "receiveDate": "2023-06-01",
     "recipientNo": "String",
     "resultCode": "String",
     "senderGroupingKey": "String",
     "recipientGroupingKey": "String",
     "_links": {
       "self": {
         "href": "String"
       }
     },
    "hookId": "String"
  }
]
```