## Notification > KakaoTalk Bizmessage > 알림톡 > API v1.0 Guide

## 알림톡

#### [API 도메인]

<table>
<thead>
<tr>
<th>도메인</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## 메시지 발송

#### 치환 발송 요청

[URL]

```
POST  /alimtalk/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        }
    }]
}
```

| 값                   | 타입     | 필수   | 설명                                 |
| ------------------- | ------ | ---- | ---------------------------------- |
| plusFriendId        | String | X    | 플러스친구 ID                           |
| templateCode        | String | O    | 등록한 발송 템플릿 코드                      |
| recipientList       | List   | O    | 수신자 목록(최대 1000명)                   |
| - recipientNo       | String | O    | 수신 번호                              |
| - templateParameter | Object | X    | 템플릿 파라미터<br>(템플릿에 치환할 변수 포함 시, 필수) |
| -- key              | String | X    | 치환 키(#{key})                       |
| -- value            | String | X    | 치환 키에 매핑되는 Value값                  |

* <b>플러스친구 ID 필드를 보내지 않을 경우, 첫 번째 등록한 플러스친구로 발송됩니다.</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.0/appkeys/{appkey}/messages -d '{"plusFriendId": "{플러스친구 ID}","templateCode": "{템플릿 코드}","recipientList":[{"recipientNo": "{수신 번호}","templateParameter": { "{치환자 필드}": "{치환 데이터}" }}]}'
```

#### 전문 발송 요청

[URL]

```
POST  /alimtalk/v1.0/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Request Body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String
                }
            ]
        }
    ]
}
```

| 값                | 타입      | 필수   | 설명                                       |
| ---------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId     | String  | X    | 플러스친구 ID                                 |
| templateCode     | String  | O    | 등록한 발송 템플릿 코드                            |
| recipientList    | List    | O    | 수신자 목록(최대 1,000명)                        |
| - recipientNo    | String  | O    | 수신 번호                                    |
| - content        | String  | O    | 내용                                       |
| - buttons        | List    | X    | 버튼                                       |
| -- ordering      | Integer | X    | 버튼 순서(버튼이 있는 경우 필수)                      |
| -- type          | String  | X    | 버튼 타입(WL:웹 링크, AL:앱 링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
| -- name          | String  | X    | 버튼 이름(버튼이 있는 경우 필수)                      |
| -- linkMo        | String  | X    | 모바일 웹 링크(WL 타입일 경우 필수 필드)                |
| -- linkPc        | String  | X    | PC 웹 링크(WL 타입일 경우 선택 필드)                 |
| -- schemeIos     | String  | X    | iOS 앱 링크(AL 타입일 경우 필수 필드)                |
| -- schemeAndroid | String  | X    | Android 앱 링크(AL 타입일 경우 필수 필드)            |


* <b>플러스친구 ID 필드를 보내지 않을 경우, 첫 번째 등록한 플러스친구로 발송됩니다.</b>
* <b>본문과 버튼에 치환이 완성된 데이터를 넣어주세요.</b>

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.0/appkeys/{appkey}/raw-messages -d '{"plusFriendId": "{플러스친구 ID}","templateCode": "{템플릿 코드}","recipientList":[{"recipientNo": "{수신 번호}", "content": "{내용}", "buttons": [{ "ordering": "{버튼 순서}", "type": "{버튼 타입}", "name": "{버튼 이름}", "linkMo": "{모바일 웹 링크}" }]}]}'
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
    "requestId": String
  }
}
```

| 값               | 타입      | 설명     |
| --------------- | ------- | ------ |
| header          | Object  | 헤더 영역  |
| - resultCode    | Integer | 결과 코드  |
| - resultMessage | String  | 결과 메시지 |
| - isSuccessful  | Boolean | 성공 여부  |
| message         | Object  | 본문 영역  |
| - requestId     | String  | 요청 ID  |

## 발송 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1번 or 2번 조건 필수

| 값                | 타입      | 필수        | 설명                                    |
| ---------------- | ------- | --------- | ------------------------------------- |
| requestId        | String  | 조건 필수(1번) | 요청 ID                                 |
| startRequestDate | String  | 조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)       |
| endRequestDate   | String  | 조건 필수(2번) | 발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm)        |
| recipientNo      | String  | X         | 수신 번호                                 |
| plusFriendId     | String  | X         | 플러스친구 ID                              |
| templateCode     | String  | X         | 템플릿 코드                                |
| messageStatus    | String  | X         | 요청 상태(COMPLETED -> 성공, FAILED -> 실패 ) |
| resultCode       | String  | X         | 발송 결과(MRC01 -> 성공 MRC02 -> 실패 )       |
| pageNum          | Integer | X         | 페이지 번호(기본: 1)                         |
| pageSize         | Integer | X         | 조회 건수(Default : 15, Max : 1000)        |

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
      "plusFriendId" :  String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "buttons" : [
        {
          "ordering" :  Integer,
          "type" :  String,
          "name" :  String,
          "linkMo" :  String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String
        }
      ]
    }
    ],
    "totalCount" :  Integer
  }
}
```

| 값                           | 타입      | 설명                                       |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | 헤더 영역                                    |
| - resultCode                | Integer | 결과 코드                                    |
| - resultMessage             | String  | 결과 메시지                                   |
| - isSuccessful              | Boolean | 성공 여부                                    |
| messageSearchResultResponse | Object  | 본문 영역                                    |
| - messages                  | List    | 메시지 리스트                                  |
| -- requestId                | String  | 요청 ID                                    |
| -- plusFriendId             | String  | 플러스친구 ID                                 |
| -- templateCode             | String  | 템플릿 코드                                   |
| -- recipientNo              | String  | 수신 번호                                    |
| -- content                  | String  | 본문                                       |
| -- requestDate              | String  | 요청 일시                                    |
| -- receiveDate              | String  | 수신 일시                                    |
| -- resendStatus             | String  | 재발송 상태 코드                                |
| -- resendStatusName         | String  | 재발송 상태 코드명                               |
| -- messageStatus            | String  | 요청  상태(COMPLETED -> 성공, FAILED -> 실패 )   |
| -- resultCode               | String  | 수신 결과 코드                                 |
| -- resultCodeName           | String  | 수신 결과 코드명                                |
| -- buttons                  | List    | 버튼 목록                                    |
| --- ordering                | Integer | 버튼 순서                                    |
| --- type                    | String  | 버튼 타입(WL:웹 링크, AL:앱 링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
| --- name                    | String  | 버튼 이름                                    |
| --- linkMo                  | String  | 모바일 웹 링크(WL 타입일 경우 필수 필드)                |
| --- linkPc                  | String  | PC 웹 링크(WL 타입일 경우 선택 필드)                 |
| --- schemeIos               | String  | iOS 앱 링크(AL 타입일 경우 필수 필드)                |
| --- schemeAndroid           | String  | Android 앱 링크(AL 타입일 경우 필수 필드)            |
| - totalCount                | Integer | 총 개수                                     |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.0/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS 재발송 상태
| 값     | 설명                              |
| ----- | ------------------------------- |
| RSC01 | 재발송 미대상                         |
| RSC02 | 재발송 대상(발송 결과 실패 시, 재발송이 진행됩니다.) |
| RSC03 | 재발송 중                           |
| RSC04 | 재발송 성공                          |
| RSC05 | 재발송 실패                          |

## 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /alimtalk/v1.0/appkeys/{appkey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 타입     | 설명     |
| ------ | ------ | ------ |
| appkey | String | 고유의 앱키 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 값            | 타입     | 필수   | 설명                                       |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | 콘솔에서 생성할 수 있다. [[참고](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 값              | 타입      | 필수   | 설명            |
| -------------- | ------- | ---- | ------------- |
| plusFriendId   | String  | X    | 플러스친구 ID      |
| templateCode   | String  | X    | 템플릿 코드        |
| templateName   | String  | X    | 템플릿 이름        |
| templateStatus | String  | X    | 템플릿 상태 코드     |
| pageNum        | Integer | X    | 페이지 번호(기본: 1) |
| pageSize       | Integer | X    | 조회 건수(Default : 15, Max : 1000) |

| 템플릿 상태 코드 | 설명   |
| --------- | ---- |
| TSC01     | 요청   |
| TSC02     | 검수중  |
| TSC03     | 승인   |
| TSC04     | 반려   |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.0/appkeys/{appkey}/templates?plusFriendId={플러스친구 ID}&templateStatus={템플릿 상태 코드}"
```

#### 응답
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
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
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
                      "status": String
                    }  
                ],
                "status": String,
                "statusName": String,
                "createDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| 값                    | 타입      | 설명                                       |
| -------------------- | ------- | ---------------------------------------- |
| header               | Object  | 헤더 영역                                    |
| - resultCode         | Integer | 결과 코드                                    |
| - resultMessage      | String  | 결과 메시지                                   |
| - isSuccessful       | Boolean | 성공 여부                                    |
| templateListResponse | Object  | 본문 영역                                    |
| - templates          | List    | 템플릿 리스트                                  |
| -- plusFriendId      | String  | 플러스친구 ID                                 |
| -- templateCode      | String  | 템플릿 코드                                   |
| -- templateName      | String  | 템플릿명                                     |
| -- templateContent   | String  | 템플릿 본문                                   |
| -- buttons           | List    | 버튼 목록                                    |
| --- ordering         | Integer | 버튼 순서(1~5)                               |
| --- type             | String  | 버튼 타입(WL:웹 링크, AL:앱 링크, DS:배송 조회, BK:봇 키워드, MD:메시지 전달) |
| --- name             | String  | 버튼 이름                                    |
| --- linkMo           | String  | 모바일 웹 링크(WL 타입일 경우 필수 필드)                |
| --- linkPc           | String  | PC 웹 링크(WL 타입일 경우 선택 필드)                 |
| --- schemeIos        | String  | iOS 앱 링크(AL 타입일 경우 필수 필드)                |
| --- schemeAndroid    | String  | Android 앱 링크(AL 타입일 경우 필수 필드)            |
| -- comments          | List    | 검수 결과                                    |
| --- id               | Integer | 문의 아이디                                   |
| --- content          | String  | 문의 내용                                    |
| ---userName          | String  | 작성자                                      |
| ---createAt          | String  | 등록 날짜                                    |
| ---status            | String  | 댓글 상태(INQ: 문의, APR: 승인, REJ: 반려, REP: 답변) |
| -- status            | String  | 템플릿 상태                                   |
| -- statusName        | String  | 템플릿 상태명                                  |
| -- createDate        | String  | 생성 일자                                    |
| - totalCount         | Integer | 총 개수                                     |
