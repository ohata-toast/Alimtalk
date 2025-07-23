## Notification > KakaoTalk Bizmessage > Webhook > API Guide

<span id="webhook"></span>
## 웹훅
KakaoTalk Bizmessage 서비스 내 특정 이벤트가 발생하면 웹훅 설정에 정의된 URL로 POST 요청을 생성합니다.<br>
생성된 POST 요청에 대한 API 문서입니다.

### 웹훅 발송

[URL]

|Http method|	URI|
|---|---|
| POST | 웹훅 설정에 정의한 대상 URL |

[Header]

|값|	타입|	설명|
|---|---|---|
|X-Toast-Webhook-Signature|	String| 웹훅 설정 시 입력한 서명 |

[Request body]

|값|	타입|	설명|
|---|---|---|
|hooksId|	String| 웹훅 설정에 정의된 URL로 POST 요청을 할 때마다 고유하게 생성되는 ID |
|webhookConfigId|	String|웹훅 설정 ID|
|productName|	String|	웹훅 이벤트가 발생한 서비스명 |
|appKey|	String| 웹훅 이벤트가 발생한 서비스 앱키 |
|event|	String| 웹훅 이벤트명 |
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터<br>* 상세한 내용은 [이벤트 유형별 훅(hook) 정의](./webhook-api-guide/#event-hooks)를 참고해 주세요. |

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

### 이벤트 유형별 hooks 정의
웹훅 설정에 정의된 URL로 POST 요청을 생성할 때 이벤트 타입별 훅(hook) 데이터입니다.
#### 템플릿 상태/문의 업데이트
|값|	타입|	설명|
|---|---|---|
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터 |
|- hookId|	String| 서비스에서 이벤트가 발생할 때 생성되는 고유 ID |
|- senderKey|	String|	발신 키 |
|- templateCode|	String| 템플릿 코드 |
|- kakaoTemplateCode|	String| 원본 템플릿 코드 |
|- status|	String| 템플릿 상태(TSC01: 요청, TSC02: 검수 중, TSC03: 승인, TSC04: 반려) |
|- comments|	List| 검수 결과 |
|-- id|	String| 문의 아이디|
|-- content|	String|문의 내용 |
|-- userName|	String|작성자 |
|-- createdAt|	String|등록 날짜 |
|-- attachment|	List|첨부 파일 |
|--- originalFileName|	String|첨부 파일명 |
|--- filePath|	String|첨부 파일 경로 |
|-- status|	String| 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
|- updateDate|	String| 수정 일자 |

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

#### 메시지 발송 결과 코드 업데이트
|값|	타입| 	설명                                                                                                                                                             |
|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터                                                                                                                                                 |
|- kakaoMessageType|	String| 카카오 메시지 타입<br>ALIMTALK_NORMAL<br>ALIMTALK_AUTH<br>ALIMTALK_MASS<br>FRIENDTALK_NORMAL<br>FRIENDTALK_MASS<br>BRAND_MESSAGE_NORMAL<br>BRAND_MESSAGE_MASS |
|- requestId|	String| 요청 ID                                                                                                                                                           |
|- recipientSeq|	Integer| 수신자 시퀀스 번호                                                                                                                                                      |
|- requestDate|	String| 요청 일시                                                                                                                                                           |
|- createDate|	String| 생성 일시                                                                                                                                                           |
|- receiveDate|	String| 수신 일시                                                                                                                                                           |
|- recipientNo|	String| 수신번호                                                                                                                                                            |
|- resultCode|	String| 수신 결과 코드                                                                                                                                                        |
|- senderGroupingKey|	String| 발신 그룹핑 키                                                                                                                                                        |
|- recipientGroupingKey|	String| 수신자 그룹핑 키                                                                                                                                                       |
|- _links|	Object| 	링크                                                                                                                                                             |
|- self|	Object| 	-                                                                                                                                                              |
|- href|	String| 	메시지 조회 API 링크                                                                                                                                                  |
|- hookId|	String| 서비스에서 이벤트가 발생할 때 생성되는 고유 ID                                                                                                                                     |

```json
"hooks": [
  {
     "kakaoMessageType": "String(ALIMTALK_NORMAL / ALIMTALK_AUTH / ALIMTALK_MASS / FRIENDTALK_NORMAL / FRIENDTALK_MASS / BRAND_MESSAGE_NORMAL / BRAND_MESSAGE_MASS)",
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
