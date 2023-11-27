## Notification > KakaoTalk Bizmessage > 친구톡 > API v2.3 가이드

## 친구톡

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

## v2.3 API 소개
1. 친구톡 와이드 아이템리스트, 케러셀 피드형, 쿠폰, 비즈니스폼 버튼 기능이 추가되었습니다.
2. 와이드 아이템리스트 이미지 등록, 캐러셀 이미지 등록 API가 추가되었습니다.
3. 이미지 조회 시, imageType 필드가 추가되었습니다.
4. 발송 시, imageSeq -> imageUrl 필드를 사용하도록 변경되었습니다.

## 메시지 발송
#### 발송 요청

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/messages
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
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
    "recipientList": [{
        "recipientNo": String,
        "content": String,
        "imageUrl": String,
        "imageLink": String,
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
            "bizFormKey": String,
            "target": String
          }
        ],
        "header": String,
        "item": {
          "list": [
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            }
          ]
        },
        "carousel": {
          "list": [
            {
              "header": String,
              "message": String,
              "attachment": {
                "buttons": [
                  {
                    "name": String,
                    "type": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeAndroid": String,
                    "schemeIos": String,
                  }
                ],
                "image":{
                  "imageUrl": String,
                  "imageLink": String
                }
              }
            }
          ],
          "tail": {
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          }
        },
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "resendParameter": {
            "isResend" : boolean,
            "resendType" : String,
            "resendTitle" : String,
            "resendContent" : String,
            "resendSendNo" : String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키(40자) |
|requestDate|	String|	X | 요청 일시(yyyy-MM-dd HH:mm), 필드를 보내지 않을 경우, 즉시 발송 |
|senderGroupingKey| String | X| 발신 그룹핑 키(최대 100자) |
| createUser | String | X | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|recipientList|	List|	O|	수신자 목록(최대 1000명) |
|- recipientNo|	String|	O|	수신 번호 |
|- content|	String|	O| 내용(최대 1000자)<br>이미지 발송 시, 최대 400자<br>와이드 이미지 발송 시, 최대 76자 |
|- imageUrl|	String|	X|	이미지 URL |
|- imageLink|	String|	X|	이미지 링크   |
|- buttons|	List|	X|	버튼<br>와이드 이미지 발송 시, 링크 버튼 최대 2개 |
|-- ordering|	Integer|	X |	버튼 순서(버튼이 있는 경우 필수)|
|-- type| String |	X |	버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달, BF:비즈니스폼) |
|-- name| String |	X |	버튼 이름(버튼이 있는 경우 필수)|
|-- linkMo| String |	X |	모바일 웹 링크(WL 타입일 경우 필수 필드)|
|-- linkPc | String |	X |PC 웹 링크(WL 타입일 경우 선택 필드) |
|-- schemeIos | String | X |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String | X |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|-- chatExtra|	String|	X| BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
|-- chatEvent|	String|	X| BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
|-- bizFormKey|	String|	X| BF(비즈니스 폼) 타입 버튼 시, 비즈폼 키 |
|-- target|	String|	X |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
|- header | String | X | 헤더(와이드 아이템리스트 메시지 타입 사용 시, 필수, 최대 25자) |
|- item | Object | X | 와이드 아이템 |
|-- list | List | X | 와이드 아이템리스트(최소 3개/최대 4개) |
|--- title | String | X | 아이템 제목(최대 25자) |
|--- imageUrl | String | X | 아이템 이미지 URL |
|--- linkMo | String | X | 모바일 웹 링크 |
|--- linkPc | String | X | PC 웹 링크 |
|--- schemeIos | String | X | iOS 앱 링크 |
|--- schemeAndroid | String | X | 안드로이드 앱 링크 |
|- carousel | Object | X | 캐러셀 | 
|-- list | List | X |  캐러셀 리스트(최소 2개/최대 6개) | 
|--- header | String | X | 캐러셀 아이템 제목(최대 20자) | 
|--- message | String | X | 캐러셀 아이템 메시지(최대 180자) | 
|--- attachment | Object | X | 캐러셀 아이템 이미지, 버튼 정보 | 
|---- buttons | List | X | 버튼 리스트(최대 2개) | 
|----- name| String |	X |	버튼 이름(버튼이 있는 경우 필수, 최대 28자)|
|----- type| String |	X |	버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달, BF:비즈니스폼) |
|----- linkMo| String |	X |	모바일 웹 링크(WL 타입일 경우 필수 필드)|
|----- linkPc | String |	X |PC 웹 링크(WL 타입일 경우 선택 필드) |
|----- schemeIos | String | X |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
|----- schemeAndroid | String | X |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|---- image | Object | X |  | 
|----- imageUrl|	String|	X|	이미지 URL   |
|----- imageLink|	String|	X|	이미지 링크   |
|-- tail | Object | X | 더보기 버튼 정보 | 
|--- linkMo| String |	X |	모바일 웹 링크|
|--- linkPc | String |	X |PC 웹 링크 |
|--- schemeIos | String | X |	iOS 앱 링크 |
|--- schemeAndroid | String | X |	안드로이드 앱 링크 |
|- coupon | Object | X | 쿠폰 | 
|-- title| String |	X |	title의 경우 5가지 형식으로 제한 됨<br>"${숫자}원 할인 쿠폰" 숫자는 1이상 99,999,999 이하<br>"${숫자}% 할인 쿠폰" 숫자는 1이상 100 이하<br>"배송비 할인 쿠폰"<br><br>"${7자 이내} 무료 쿠폰"<br>"${7자 이내} UP 쿠폰"|
|-- description| String |	X |	쿠폰 상세 설명 (일반 텍스트, 이미지형 최대 12자 / 와이드 이미지형, 와이드 아이템리스트형 최대 18자)|
|-- linkMo| String |	X |	모바일 웹 링크 (하단 필수 조건 확인) |
|-- linkPc | String |	X |PC 웹 링크 |
|-- schemeIos | String | X |	iOS 앱 링크 |
|-- schemeAndroid | String | X |	안드로이드 앱 링크 |
|- resendParameter|	Object|	X| 대체 발송 정보 |
|-- isResend|	boolean|	X|	발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 재발송됩니다. |
|-- resendType|	String|	X|	대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다. |
|-- resendTitle|	String|	X|	LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 재발송됩니다.) |
|-- resendContent|	String|	X|	대체 발송 내용<br>(값이 없을 경우, [메시지 본문과 웹링크 버튼명 - 웹링크 Mobile 링크]으로 재발송됩니다.) |
|-- resendSendNo | String| X| 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|-- resendUnsubscribeNo | String| X| 대체 발송 080 수신 거부 번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신 거부 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |
|- isAd | Boolean | X |	광고 여부(기본값 true) |
|- recipientGroupingKey|	String|	X|	수신자 그룹핑 키(최대 100자) |
| statsId | String |	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다, 최대 8자) |

* <b>요청 일시는 호출하는 시점부터 90일 후까지 설정 가능합니다.</b>
* <b>야간 발송 제한(20:50~다음 날 08:00)</b>
* <b>SMS 서비스로 대체 발송되므로, SMS 서비스의 발송 API 명세에 따라 필드를 입력해야 합니다.(SMS 서비스에 등록된 발신 번호, 080 수신 거부 번호, 각종 필드 길이 제한 등)</b>
* <b>지정한 대체 발송 타입의 바이트 제한을 초과하는 대체 발송 제목이나 내용은 잘려서 대체 발송될 수 있습니다.([[SMS 주의 사항](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 참고)</b>
* <b>친구톡 광고 메시지는 광고 SMS API로 대체 발송되므로, 반드시 080 수신 거부 번호를 등록해야 대체 발송됩니다.</b>
* <b>친구톡 광고 메시지의 resendContent 필드를 입력할 경우, SMS 광고 API의 <span style="color:red">광고 문구</span>를 필수로 입력해야 정상 대체 발송됩니다. `(광고)내용[무료 수신거부]080XXXXXXX`</b>
* <b>친구톡 광고 메시지의 resendContent 필드가 없을 경우, 등록된 080 수신 거부 번호로 <span style="color:red">광고 문구</span>를 자동 생성해서 대체 발송됩니다.</b>
* <b>와이드 아이템리스트형, 캐러셀 피드형은 광고 발송만 가능합니다.</b>
* <b>쿠폰은 일반 텍스트형, 이미지형, 와이드 이미지형, 와이드 아이템리스트형만 사용 가능합니다.</b>
* <b>쿠폰의 linkMo 필수값 나머지 옵션값 또는 채널 쿠폰 URL(포멧: alimtalk=coupon://) 사용 - scheme_android 혹은 scheme_ios 둘 중 하나 필수값 나머지 옵션값</b>


[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"9e0afe2c12aaaaaaaaaa7520052880b555f1a60a","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"내용","buttons":[{"ordering":1,"type":"WL","name":"버튼1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
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
|- requestId | String |	요청 ID |
|- senderGroupingKey | String |	발신 그룹핑 키 |
|- sendResults | Object | 발송 요청 결과 |
|-- recipientSeq | Integer | 수신자 시퀀스 번호 |
|-- recipientNo | String | 수신 번호 |
|-- resultCode | Integer | 발송 요청 결과 코드 |
|-- resultMessage | String | 발송 요청 결과 메시지 |
|-- recipientGroupingKey | String | 수신자 그룹핑 키 |

## 발송 목록 조회

#### 요청

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/messages
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
|requestId|	String|	조건 필수(1번) | 요청 ID |
|startRequestDate|	String|	조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 조건 필수(2번) |	발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm) |
|startCreateDate| String| 조건 필수(3번) | 등록 날짜 시작값(yyyy-MM-dd HH:mm)|
|endCreateDate|  String| 조건 필수(3번) |  등록 날짜 끝값(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	수신 번호 |
|senderKey|	String|	X |	발신키 |
|senderGroupingKey| String | X| 발신 그룹핑 키 |
|recipientGroupingKey|	String|	X|	수신자 그룹핑 키 |
|messageStatus| String |	X | 요청 상태(COMPLETED: 성공, FAILED: 실패 )	|
|resultCode| String |	X | 발송 결과(MRC01: 성공 MRC02: 실패 )	|
|createUser| String | X | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|pageNum|	Integer|	X|	페이지 번호(Default: 1)|
|pageSize|	Integer|	X|	조회 건수(Default: 15, Max: 1000)|

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
          "recipientNo" :  String,
          "requestDate" : String,
          "createDate" : String,
          "receiveDate" : String,
          "content" :  String,
          "messageStatus" :  String,
          "resendStatus" :  String,
          "resendStatusName" :  String,
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
|-- requestId | String |	요청 ID |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- senderKey   | String | 발신 키   |
|-- recipientNo | String |	수신 번호 |
|-- requestDate | String |	요청 일시 |
|-- createDate | String | 등록 일시 |
|-- receiveDate | String |	수신 일시 |
|-- content | String |	본문 |
|-- messageStatus | String |	요청 상태(COMPLETED: 성공, FAILED: 실패 ) |
|-- resendStatus | String |	재발송 상태 코드 |
|-- resendStatusName | String |	재발송 상태 코드명 |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### 재발송 상태
| 이름 |	설명|
|---|---|
|RSC01|	재발송 미대상|
|RSC02|	재발송 대상(발송 결과 실패 시, 재발송이 진행됩니다.)|
|RSC03|	재발송 중|
|RSC04|	재발송 성공|
|RSC05|	재발송 실패|

## 발송 단건 조회

#### 요청

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	O | 요청 ID |
|recipientSeq|	Integer |	O | 수신자 시퀀스 번호 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "senderKey" :  String,
      "recipientNo" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" : String,
      "resendRequestId" : String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
      "imageSeq" : Integer,
      "imageName" : String,
      "imageUrl" : String,
      "imageLink" : String,
      "wide" : Boolean,
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
          "bizFormKey": String,
          "target": String
        }
      ],
      "header": String,
       "item": {
        "list": [
           {
            "title": String,
            "imageUrl": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
          }
         ]
       },
      "carousel": {
        "list": [
          {
            "header": String,
            "message": String,
            "attachment": {
               "buttons": [
                {
                  "name": String,
                  "type": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String,
                 }
              ],
              "image":{
                "imageUrl": String,
                "imageLink": String
               }
            }
           }
        ],
        "tail": {
           "linkMo": String,
          "linkPc": String,
           "schemeAndroid": String,
          "schemeIos": String
         }
      },
      "coupon": {
        "title": String,
        "description": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      "isAd" : Boolean,
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
|- requestId | String |	요청 ID |
|- recipientSeq | Integer |	수신자 시퀀스 번호 |
|- plusFriendId | String |	플러스친구 ID |
|- senderKey   | String | 발신 키   |
|- recipientNo | String |	수신 번호 |
|- requestDate | String |	요청 일시 |
|- createDate | String | 등록 일시 |
|- receiveDate | String |	수신 일시 |
|- content | String |	본문 |
|- messageStatus | String |	요청 상태(COMPLETED: 성공, FAILED: 실패 ) |
|- resendStatus | String |	재발송 상태 코드 |
|- resendStatusName | String |	재발송 상태 코드명 |
|- resendResultCode | String | 재발송 결과 코드 SMS 결과 코드 |
|- resendRequestId | String | 재발송 SMS 요청 ID |
|- resultCode | String |	수신 결과 코드 |
|- resultCodeName | String |	수신 결과 코드명 |
|- createUser | String | 등록자(콘솔에서 발송 시 사용자 UUID로 저장) |
|- imageSeq|	Integer|  이미지 번호 |
|- imageName|	String|  이미지명(업로드한 파일명) |
|- imageUrl|	String|  이미지 URL |
|- imageLink|	String| 이미지 링크   |
|- wide     | boolean | 와이드 이미지 여부 |
|- buttons | List |	버튼 리스트 |
|-- ordering | Integer |	버튼 순서 |
|-- type | String |	버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달) |
|-- name | String |	버튼 이름 |
|-- linkMo | String |	모바일 웹 링크(WL 타입일 경우 필수 필드) |
|-- linkPc | String |	PC 웹 링크(WL 타입일 경우 선택 필드) |
|-- schemeIos | String |	iOS 앱 링크(AL 타입일 경우 필수 필드) |
|-- schemeAndroid | String |	안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|-- chatExtra|	String|	BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
|-- chatEvent|	String| BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
|-- bizFormKey|	String|	BF(비즈니스 폼) 타입 버튼 시, 비즈폼 키 |
|-- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
|- header | String | 헤더(와이드 아이템리스트 메시지 타입 사용 시, 필수, 최대 25자) |
|- item | Object | 와이드 아이템 |
|-- list | List | 와이드 아이템리스트(최소 3개/최대 4개) |
|--- title | String | 아이템 제목(최대 25자) |
|--- imageUrl | String | 아이템 이미지 URL |
|--- linkMo | String | 모바일 웹 링크 |
|--- linkPc | String | PC 웹 링크 |
|--- schemeIos | String | iOS 앱 링크 |
|--- schemeAndroid | String | 안드로이드 앱 링크 |
|- carousel | Object | 캐러셀 | 
|-- list | List | 캐러셀 리스트(최소 2개/최대 6개) | 
|--- header | String | 캐러셀 아이템 제목(최대 20자) | 
|--- message | String | 캐러셀 아이템 메시지(최대 180자) | 
|--- attachment | Object | 캐러셀 아이템 이미지, 버튼 정보 | 
|---- buttons | List | 버튼 리스트(최대 2개) | 
|----- name| String |	버튼 이름(버튼이 있는 경우 필수, 최대 28자)|
|----- type| String |	버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달, BF:비즈니스폼) |
|----- linkMo| String |	모바일 웹 링크(WL 타입일 경우 필수 필드)|
|----- linkPc | String | PC 웹 링크(WL 타입일 경우 선택 필드) |
|----- schemeIos | String | iOS 앱 링크(AL 타입일 경우 필수 필드) |
|----- schemeAndroid | String | 안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
|---- image | Object | 이미지  | 
|----- imageUrl|	String|	이미지 URL   |
|----- imageLink|	String|	이미지 링크   |
|-- tail | Object | 더보기 버튼 정보 | 
|--- linkMo| String |	모바일 웹 링크|
|--- linkPc | String |	PC 웹 링크 |
|--- schemeIos | String | iOS 앱 링크 |
|--- schemeAndroid | String | X안드로이드 앱 링크 |
|- coupon | Object | 쿠폰 | 
|-- title| String |	쿠폰 title |
|-- description| String |	쿠폰 상세 설명 |
|-- linkMo| String | 모바일 웹 링크 |
|-- linkPc | String |	PC 웹 링크 |
|-- schemeIos | String | iOS 앱 링크 |
|-- schemeAndroid | String | 안드로이드 앱 링크 |
|- isAd | Boolean |	광고 여부 |
|- senderGroupingKey | String | 발신 그룹핑 키 |
|- recipientGroupingKey | String |	수신자 그룹핑 키 |

## 메시지
### 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}
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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### 메시지 결과 업데이트 조회

#### 요청

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/message-results
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

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|startUpdateDate|	String|	O | 결과 업데이트 조회 시작 시간(yyyy-MM-dd HH:mm)|
|endUpdateDate|	String| O |	결과 업데이트 조회 종료 시간(yyyy-MM-dd HH:mm) |
|pageNum|	Integer|	X|	페이지 번호(기본: 1)|
|pageSize|	Integer|	X|	조회 건수(기본: 15)|

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
      "senderKey"    :  String,
      "recipientNo" :  String,
      "requestDate" :  String,
      "receiveDate" : String,
      "content" :  String,
      "messageStatus" :  String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
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
|-- requestId | String |	요청 ID |
|-- recipientSeq | Integer |	수신자 시퀀스 번호 |
|-- plusFriendId | String |	플러스친구 ID |
|-- senderKey | String |	발신 키 |
|-- recipientNo | String |	수신 번호 |
|-- requestDate | String |	요청 일시 |
|-- receiveDate | String |	수신 일시 |
|-- content | String |	본문 |
|-- messageStatus | String |	요청 상태(COMPLETED -> 성공, FAILED -> 실패, CANCEL -> 취소 ) |
|-- resendStatus | String |	재발송 상태 코드 |
|-- resendStatusName | String |	재발송 상태 코드명 |
|-- resultCode | String |	수신 결과 코드 |
|-- resultCodeName | String |	수신 결과 코드명 |
|-- senderGroupingKey | String | 발신 그룹핑 키 |
|-- recipientGroupingKey | String |	수신자 그룹핑 키 |
|- totalCount | Integer | 총개수 |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### 대량 발송 요청 목록 조회

#### 요청
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
| X-Secret-Key | String | 고유의 비밀 키 |

[Query parameter]
* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.

| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | O | 발송 날짜 시작 |
| endRequestDate | String | - | O | 발송 날짜 종료 |
| startCreateDate |	String| - |	O |	등록 날짜 시작 |
| endCreateDate |	String| - |	O |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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
                "masterStatusCode": String,
                "content": String,
                "isAd": Boolean,
                "imageSeq": Integer,
                "imageLink": String,
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

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean | 성공 여부 |
| body | Object | 본문 영역 |
| - messages | Object | 메시지 리스트 |
| -- requestId | String | 요청 ID |
| -- requestDate | String | 요청 날짜 |
| -- plusFriendId | String | 플러스 친구 ID |
| -- senderKey | String | 전송자 ID |
| -- masterStatusCode | String | 대량 발송 상태 코드(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 내용 |
| -- isAd | Boolean | 광고 여부 |
| -- imageSeq | Integer | 이미지 순서 |
| -- imageLink | Boolean | 이미지 URL |
| -- fileId | String | 첨부 파일 ID |
| -- autoSendYn | String | 자동 발송 여부 |
| -- statsId | String | 통계 ID |
| -- createDate | String | 생성 날짜 |
| -- createUser | String | 생성 사용자(콘솔에서 발송 시 사용자 UUID로 저장) |
| - totalCount | Integer | 총개수 |


### 대량 발송 수신자 목록 조회

#### 요청
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey |	String |	고유의 앱키 |
| requestId |	String |	요청 ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
| X-Secret-Key | String | 고유의 비밀 키 |


| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |
| pageNum | optional, Integer | - | X | 페이지 번호 |
| pageSize | optional, Integer | 1000 | X | 검색 수 |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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

| 이름 | 타입| 설명 |
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean| 성공 여부 |
| body | Object | 본문 영역 |
| - recipients | List | 수신자 리스트 |
| -- requestId | String | 요청 ID |
| -- recipientSeq | Integer | 수신자 시퀀스 번호 |
| -- recipientNo | String | 수신 번호 |
| -- requestDate | String | 요청 날짜 |
| -- receiveDate | String | 수신 날짜 |
| -- messageStatus | String | 대량 수신자 발송 상태 코드(READY, COMPLETED, FAILED, CANCEL) |
| -- resultCode | String | 수신 결과 코드 |
| -- resultCodeName | String | 수신 결과 코드명 |
| - totalCount | Integer | 총개수 |

### 대량 발송 수신자 조회

#### 요청
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey |	String | 고유의 앱키 |
| requestId |	String | 요청 ID |
| recipientSeq | String | 수신자 순서 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
|X-Secret-Key|	String|	고유의 비밀 키 |


| 이름 |	타입| 최대 길이 |	필수|	설명|
|---|---|---|---|---|
| requestId | String | - | O | 요청 ID |
| startRequestDate | String | - | X | 발송 날짜 시작 |
| endRequestDate | String | - | X | 발송 날짜 종료 |
| startCreateDate |	String| - |	X |	등록 날짜 시작 |
| endCreateDate |	String| - |	X |	등록 날짜 종료 |


#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### 응답
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
        "recipientNo": String,
        "requestDate": String,
        "receiveDate": String,
        "content": String,
        "messageStatus": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendRequestId": String,
        "resendResultCode": String,
        "resultCode": String,
        "resultCodeName": String,
        "imageSeq": Integer,
        "imageLink": String,
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
                "bizFormKey": String,
                "target": String
            }
        ],
        "header": String,
        "item": {
          "list": [
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            }
          ]
        },
        "carousel": {
          "list": [
            {
              "header": String,
              "message": String,
              "attachment": {
                "buttons": [
                  {
                    "name": String,
                    "type": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeAndroid": String,
                    "schemeIos": String,
                  }
                ],
                "image":{
                  "imageUrl": String,
                  "imageLink": String
                }
              }
            }
          ],
          "tail": {
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          }
        },
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "isAd": Boolean,
        "createDate": String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
| header | Object |	헤더 영역 |
| - resultCode |	Integer |	결과 코드 |
| - resultMessage |	String | 결과 메시지 |
| - isSuccessful |	Boolean| 성공 여부 |
| body | Object | 본문 영역 |
| - requestId | String | 요청 ID |
| - recipientSeq | Integer | 수신자 시퀀스 번호 |
| - plusFriendId | String | 플러스 친구 ID |
| - senderKey | String | 발신 키(40자)|
| - recipientNo | String | 수신 번호 |
| - requestDate | String | 요청 날짜 |
| - receiveDate | String | 수신 날짜 |
| - content | String | 본문 |
| - messageStatus | String | 대량 수신자 발송 상태 코드(READY, COMPLETED, FAILED, CANCEL) |
| - resendStatus | String |	대체 발송 상태 코드(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[아래 대체 발송 상태 표](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 참고) |
| - resendStatusName | String |	대체 발송 상태 코드명 |
| - resendRequestId | String | 대체 발송 SMS 요청 ID |
| - resendResultCode | String | 대체 발송 결과 코드 [SMS 결과 코드](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resultCode | String |	수신 결과 코드 |
| - resultCodeName | String |	수신 결과 코드명 |
| - imageSeq | Integer | 이미지 순서 |
| - imageLink | Integer | 이미지 URL |
| - buttons | List | 버튼 순서 |
| -- ordering | String | 버튼 순서 |
| -- type | String | 버튼 종류<br/> - WL: 웹링크<br/> - AL: 앱링크<br/> - DS: 배송 조회<br/> - BK: 봇 키워드<br/> - MD: 메시지 전달<br/> - BC: 상담톡 전환<br/> - BT: 봇 전환<br/> - AC: 채널 추가[광고 추가/복합형만] |
| -- name | String | 버튼 이름 |
| -- linkMo | String | 모바일 웹 링크(WL 타입일 경우 필수 필드) |
| -- linkPc | String | PC 웹 링크(WL 타입일 경우 선택 필드)|
| -- schemeIos | String | iOS 앱 링크(AL 타입일 경우 필수 필드) |
| -- schemeAndroid | String | 안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
| -- chatExtra | String | BC: 상담톡 전환시 전달할 메타 정보<br/> BT: 봇 전환 시 전달할 메타 정보 |
| -- chatEvent | String | BT: 봇 전환 시 연결할 봇 이벤트명 |
| -- bizFormKey|	String|	BF(비즈니스 폼) 타입 버튼 시, 비즈폼 키 |
| -- target|	String|	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - header | String | 헤더(와이드 아이템리스트 메시지 타입 사용 시, 필수, 최대 25자) |
| - item | Object | 와이드 아이템 |
| -- list | List | 와이드 아이템리스트(최소 3개/최대 4개) |
| --- title | String | 아이템 제목(최대 25자) |
| --- imageUrl | String | 아이템 이미지 URL |
| --- linkMo | String | 모바일 웹 링크 |
| --- linkPc | String | PC 웹 링크 |
| --- schemeIos | String | iOS 앱 링크 |
| --- schemeAndroid | String | 안드로이드 앱 링크 |
| - carousel | Object | 캐러셀 | 
| -- list | List | 캐러셀 리스트(최소 2개/최대 6개) | 
| --- header | String | 캐러셀 아이템 제목(최대 20자) | 
| --- message | String | 캐러셀 아이템 메시지(최대 180자) | 
| --- attachment | Object | 캐러셀 아이템 이미지, 버튼 정보 | 
| ---- buttons | List | 버튼 리스트(최대 2개) | 
| ----- name| String |	버튼 이름(버튼이 있는 경우 필수, 최대 28자)|
| ----- type| String |	버튼 타입(WL:웹 링크, AL:앱 링크, BK:봇 키워드, MD:메시지 전달, BF:비즈니스폼) |
| ----- linkMo| String |	모바일 웹 링크(WL 타입일 경우 필수 필드)|
| ----- linkPc | String |	PC 웹 링크(WL 타입일 경우 선택 필드) |
| ----- schemeIos | String | iOS 앱 링크(AL 타입일 경우 필수 필드) |
| ----- schemeAndroid | String | 안드로이드 앱 링크(AL 타입일 경우 필수 필드) |
| ---- image | Object | 이미지  | 
| ----- imageUrl|	String|	이미지 URL   |
| ----- imageLink|	String|	이미지 링크   |
| -- tail | Object | 더보기 버튼 정보 | 
| --- linkMo| String |	모바일 웹 링크|
| --- linkPc | String |	PC 웹 링크 |
| --- schemeIos | String | iOS 앱 링크 |
| --- schemeAndroid | String | 안드로이드 앱 링크 |
|- coupon | Object | 쿠폰 | 
|-- title| String |	쿠폰 title |
|-- description| String |	쿠폰 상세 설명 |
|-- linkMo| String | 모바일 웹 링크 |
|-- linkPc | String |	PC 웹 링크 |
|-- schemeIos | String | iOS 앱 링크 |
|-- schemeAndroid | String | 안드로이드 앱 링크 |
| - isAd | Boolean | 광고 여부 |
| - createDate | String | 생성 날짜 |


## 이미지 관리

### 이미지 등록
#### 요청

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/images
Content-Type: multipart/form-data
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

[Request parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|image|	File|	O |	이미지 |
|wide| boolean | X | 와이드 이미지 여부(Default: false) |

[예시]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "image": {
      "imageSeq" : Integer,
      "imageUrl" : String,
      "imageName" : String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|image|	Object|	본문 영역|
|- imageSeq | Integer |	이미지 번호(친구톡 메시지 발송 시 사용)|
|- imageUrl | String |	이미지 URL |
|- imageName | String |	이미지명(업로드한 파일명) |

### 와이드 아이템리스트 이미지 등록
#### 요청

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/wide-itemlist/images
Content-Type: multipart/form-data
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

[Request parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|image|	File|	O |	이미지 |

[예시]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/wide-itemlist/images" -F "image=@friend-ricecake02.jpeg"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "image": {
      "imageSeq" : Integer,
      "imageUrl" : String,
      "imageName" : String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|image|	Object|	본문 영역|
|- imageSeq | Integer |	이미지 번호(친구톡 메시지 발송 시 사용)|
|- imageUrl | String |	이미지 URL |
|- imageName | String |	이미지명(업로드한 파일명) |

### 캐러셀 이미지 등록
#### 요청

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/carousel/images
Content-Type: multipart/form-data
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

[Request parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|image|	File|	O |	이미지 |

[예시]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/carousel/images" -F "image=@friend-ricecake02.jpeg"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "image": {
      "imageSeq" : Integer,
      "imageUrl" : String,
      "imageName" : String
    }
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|image|	Object|	본문 영역|
|- imageSeq | Integer |	이미지 번호(친구톡 메시지 발송 시 사용)|
|- imageUrl | String |	이미지 URL |
|- imageName | String |	이미지명(업로드한 파일명) |

### 이미지 조회
#### 요청

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/images
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

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|imageTypes|	String|	X| - IMAGE: 일반 이미지<br/> - WIDE_IMAGE: 와이드 이미지<br/> - WIDE_ITEMLIST_IMAGE: 와이드 아이템리스트 이미지<br/> - CAROUSEL_IMAGE: 캐러셀 이미지<br/> IMAGE, WIDE_IMAGE(default) |
|pageNum|	Integer|	X|	페이지 번호(기본: 1)|
|pageSize|	Integer|	X|	조회 건수(기본: 15)|

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "imagesResponse": {
    "images" : [
        {
            "imageSeq" : Integer,
            "imageUrl" : String,
            "imageName" : String,
            "imageType": String,
            "createUser" : String
        }
    ],
    "totalCount" : Integer
  }

}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|imagesResponse| Object| 본문 영역|
|- image|	Object|	본문 영역|
|-- imageSeq | Integer |	이미지 번호(친구톡 메시지 발송 시 사용)|
|-- imageUrl | String |	이미지 URL |
|-- imageName | String |	이미지명(업로드한 파일명) |
|-- createUser | String |	생성자 |
|-- imageType | String |	- IMAGE: 일반 이미지<br/> - WIDE_IMAGE: 와이드 이미지<br/> - WIDE_ITEMLIST_IMAGE: 와이드 아이템리스트 이미지<br/> - CAROUSEL_IMAGE: 캐러셀 이미지<br/> |
|- totalCount | Integer | 총개수 |

* 이미지는 최근 등록한 순대로 정렬되어 응답합니다.

### 이미지 삭제
#### 요청

[URL]

```
DELETE  /friendtalk/v2.3/appkeys/{appkey}/images
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

[Query parameter]

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|imageSeq|	String|	O|	이미지 번호 |

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images?imageSeq=1,2,3"
```

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


## 업로드
### 비즈니스폼 등록
[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
|appkey|	String|	고유의 앱키|
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


[Request body]

```
{
    "bizFormId": Integer
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|bizFormId|	Integer|	O | 비즈니스폼 아이디 |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/biz-form -d '{"bizFormId": 1}
```

#### 응답
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "bizFormKey": String
}
```

| 이름 |	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- resultCode|	Integer|	결과 코드|
|- resultMessage|	String| 결과 메시지|
|- isSuccessful|	Boolean| 성공 여부|
|bizFormKey | String | 비즈니스폼 키 |


## 대체 발송 관리
### SMS AppKey 등록

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/failback/appkey
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
    "resendAppKey": String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|resendAppKey|	String|	O | 대체 발송으로 설정할 SMS 서비스 앱키 |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

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

### 대체 발송 설정 등록

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/failback
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
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 이름 |	타입|	필수|	설명|
|---|---|---|---|
|senderKey|	String|	O | 발신 키 |
|isResend|	Boolean|	O | 발송 실패 시, 문자 대체발송 여부<br>Console에서 대체 발송 설정 시, default로 재발송 됩니다. |
|resendSendNo|	String|	O | 대체 발송 발신번호<br><span style="color:red">(SMS 상품에 등록된 발신번호가 아닐 경우, 대체발송이 실패할 수 있습니다.)</span> |
|resendUnsubscribeNo|	String|	X | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 상품에 등록된 080수신거부번호가 아닐 경우, 대체발송이 실패할 수 있습니다.)</span> |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"senderKey": "9e0afe2c12aaaaaaaaaa7520052880b555f1a60a","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
```

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
