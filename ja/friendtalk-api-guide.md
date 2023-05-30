## Notification > KakaoTalk Bizmessage > カカともへのメッセージ > API v2.3 Guide

## カカともへのメッセージ

#### [APIドメイン]

<table>
<thead>
<tr>
<th>ドメイン</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://api-alimtalk.cloud.toast.com</td>
</tr>
</tbody>
</table>

## v2.3 API紹介
1. 친구톡 와이드 아이템리스트, 케러셀 피드형, 쿠폰, 비즈니스폼 버튼 기능이 추가되었습니다.
2. 와이드 아이템리스트 이미지 등록, 캐러셀 이미지 등록 API가 추가되었습니다.
3. 이미지 조회 시, imageType 필드가 추가되었습니다.
4. 발송 시, imageSeq -> imageUrl 필드를 사용하도록 변경되었습니다.

## メッセージの送信
#### 送信リクエスト

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

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

| 値                | タイプ | 必須 | 説明                                 |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信 |
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| createUser             | String  | X    | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| recipientList          | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo          | String  | O    | 受信番号                              |
| - content              | String  | O    | 内容(最大1000文字)<br>イメージを含む時は最大400文字  |
| - imageUrl             | String  | X    |	이미지 URL |
| - imageLink            | String  | X    | イメージリンク                                |
| - buttons              | List    | X    | ボタン                                 |
| -- ordering            | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type                | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name                | String  | X    | ボタン名(ボタンがある場合は必須)                      |
| -- linkMo              | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos           | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | X    | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | X    | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- bizFormKey          |	String | X    | Biz from key for BF(Business from) type button |
| -- target              | String  | X    |	웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
| - header               | String  | X    | Header(required when using the wide item list message type, up to 25 characters) |
| - item                 | Object  | X    | Wide item |
| -- list                | List    | X    | Wide item list(at lease 3, up to 4) |
| --- title              | String  | X    | Item title(up to 25 characters) |
| --- imageUrl           | String  | X    | Item image URL |
| --- linkMo | String | X | Mobile web link |
| --- linkPc | String | X | PC web link |
| --- schemeIos | String | X | iOS app link |
| --- schemeAndroid | String | X | Android app link |
| - carousel | Object | X | Carousel | 
| -- list | List | X |  Carousel list(at least 2, up to 6) | 
| --- header | String | X | Carousel item title(up to 20 characters) | 
| --- message | String | X | Carousel item message(up to 180 characters) | 
| --- attachment | Object | X | Carousel item images, button information | 
| ---- buttons | List | X | Button list(up to 2) | 
| ----- name| String |	X |	Button name(required, if there's a button, up to 28 characters)|
| ----- type| String |	X |	Button type(WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form) |
| ----- linkMo| String |	X |	Mobile web link(required for the WL type)|
| ----- linkPc | String |	X |PC web link(optional for the WL type) |
| ----- schemeIos | String | X |	iOS app link(required for the AL type) |
| ----- schemeAndroid | String | X |	Android app link(required for the AL type) |
| ---- image | Object | X | Image | 
| ----- imageUrl|	String|	X|	Image URL   |
| ----- imageLink|	String|	X|	Image link   |
| -- tail | Object | X | Learn more button information | 
| --- linkMo| String |	X |	Mobile web link|
| --- linkPc | String |	X |PC web link |
| --- schemeIos | String | X |	iOS app link |
| --- schemeAndroid | String | X |	Android app link |
| - coupon | Object | X | 쿠폰 | 
| -- title| String |	X |	title의 경우 5가지 형식으로 제한 됨<br>"${숫자}원 할인 쿠폰" 숫자는 1이상 99,999,999 이하<br>"${숫자}% 할인 쿠폰" 숫자는 1이상 100 이하<br>"배송비 할인 쿠폰"<br><br>"${7자 이내} 무료 쿠폰"<br>"${7자 이내} UP 쿠폰"|
| -- description| String |	X |	쿠폰 상세 설명 (일반 텍스트, 이미지형 최대 12자 / 와이드 이미지형, 와이드 아이템리스트형 최대 18자)|
| -- linkMo| String |	X |	모바일 웹 링크 (하단 필수 조건 확인) |
| -- linkPc | String |	X |PC 웹 링크 |
| -- schemeIos | String | X |	iOS 앱 링크 |
| -- schemeAndroid | String | X |	안드로이드 앱 링크 |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String  | X    | 代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span> |
| - isAd                 | Boolean | X    | 広告かどうか(デフォルト値true)                          |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |
| statsId                 | String  | X    |	統計ID(発信検索条件には含まれません, 最大8文字) |

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

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages -d '{"plusFriendId":"@プラスフレンド","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"内容","buttons":[{"ordering":1,"type":"WL","name":"ボタン1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
```

#### レスポンス

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

| 値                 | タイプ | 説明     |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域  |
| - resultCode            | Integer | 結果コード  |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否   |
| message                 | Object  | 本文領域  |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー   |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号  |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー  |

## 送信リスト照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter] 1番or(2番, 3番)の条件必須

| 値              | タイプ | 必須  | 説明                          |
| -------------------- | ------- | --------- | --------------------------------- |
| requestId            | String  | 条件必須(1番) | リクエストID                             |
| startRequestDate     | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)   |
| endRequestDate       | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)    |
| startCreateDate      | String  | 条件必須(3番) | 登録日 開始値(yyy-MM-dd HH:mm)                   |
| endCreateDate        | String  | 条件必須(3番) | 登録日 終値(yyy-MM-dd HH:mm)                    |
| recipientNo          | String  | X         | 受信番号                       |
| plusFriendId         | String  | X         | プラスフレンドID                          |
| senderGroupingKey    | String  | X         | 発信グルーピングキー                        |
| recipientGroupingKey | String  | X         | 受信者グルーピングキー                       |
| messageStatus        | String  | X         | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| resultCode           | String  | X         | 送信結果(MRC01：成功、MRC02：失敗)       |
| createUser           | String  | X         | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| pageNum              | Integer | X         | ページ番号(基本：1)                     |
| pageSize             | Integer | X         | 照会件数(基本：15, 最大 : 1000)                     |

#### レスポンス
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
          "recipientNo" :  String,
          "requestDate" : String,
          "createDate" : String,
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

| 値                     | タイプ | 説明                          |
| --------------------------- | ------- | --------------------------------- |
| header                      | Object  | ヘッダ領域                       |
| - resultCode                | Integer | 結果コード                       |
| - resultMessage             | String  | 結果メッセージ                      |
| - isSuccessful              | Boolean | 成否                        |
| messageSearchResultResponse | Object  | 本文領域                       |
| - messages                  | List    | メッセージリスト                     |
| -- requestId                | String  | リクエストID                             |
| -- recipientSeq             | Integer | 受信者シーケンス番号                  |
| -- plusFriendId             | String  | プラスフレンドID                          |
| -- recipientNo              | String  | 受信番号                       |
| -- requestDate              | String  | リクエスト日時                       |
| -- createDate               | String  | 登録日時                             |
| -- content                  | String  | 本文                          |
| -- messageStatus            | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| -- resendStatus             | String  | 再送信ステータスコード                   |
| -- resendStatusName         | String  | 再送信ステータスコード名                      |
| -- resultCode               | String  | 受信結果コード                    |
| -- resultCodeName           | String  | 受信結果コード名                       |
| -- createUser               | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| -- senderGroupingKey        | String  | 発信グルーピングキー                        |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                       |
| - totalCount                | Integer | 総個数                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### 再送信ステータス
| 値 | 説明                        |
| ----- | ------------------------------- |
| RSC01 | 再送信の対象外                   |
| RSC02 | 再送信対象(送信結果が失敗の時、再送信が行われます。) |
| RSC03 | 再送信中                      |
| RSC04 | 再送信成功                    |
| RSC05 | 再送信失敗                    |

## 送信単件照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値      | タイプ | 必須 | 説明   |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | リクエストID      |
| recipientSeq | Integer | O    | 受信者シーケンス番号 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

#### レスポンス
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
                  "bizFormKey": String,
                  "target": String
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

| 値                | タイプ | 説明                                 |
| ---------------------- | ------- | ---------------------------------------- |
| header                 | Object  | ヘッダ領域                              |
| - resultCode           | Integer | 結果コード                              |
| - resultMessage        | String  | 結果メッセージ                             |
| - isSuccessful         | Boolean | 成否                               |
| message                | Object  | メッセージ                                |
| - requestId            | String  | リクエストID                                    |
| - recipientSeq         | Integer | 受信者シーケンス番号                         |
| - plusFriendId         | String  | プラスフレンドID                                 |
| - recipientNo          | String  | 受信番号                              |
| - requestDate          | String  | リクエスト日時                              |
| - createDate           | String  | 登録日時                             |
| - receiveDate          | String  | 受信日時                              |
| - content              | String  | 本文                                 |
| - messageStatus        | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗)      |
| - resendStatus         | String  | 再送信ステータスコード                          |
| - resendStatusName     | String  | 再送信ステータスコード名                             |
| - resultCode           | String  | 受信結果コード                           |
| - resultCodeName       | String  | 受信結果コード名                              |
| - createUser           | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| - imageSeq             | Integer | イメージ番号                             |
| - imageName            | String  | イメージ名(アップロードしたファイル名)                           |
| - imageUrl             | String  | イメージURL                                  |
| - imageLink            | String  | イメージリンク(イメージ番号を入力した場合は必須)                |
| - wide                 | Boolean | ワイドイメージの可否                        |
| - buttons              | List    | ボタンリスト                              |
| -- ordering            | Integer | ボタン順序                              |
| -- type                | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name                | String  | ボタン名                              |
| -- linkMo              | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos           | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | BC(상담톡 전환) / BT(봇 전환) 타입 버튼 시, 전달할 메타정보 |
| -- chatEvent           | String  | BT(봇 전환) 타입 버튼 시, 연결할 봇 이벤트명 |
| -- bizFormKey|	String|	BF(비즈니스 폼) 타입 버튼 시, 비즈폼 키 |
| -- target              | String  | 웹 링크 버튼일 경우, "target":"out" 속성 추가 시 아웃 링크<br>기본 인앱 링크로 발송 |
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
| - isAd                 | Boolean | 広告かどうか                                   |
| - senderGroupingKey    | String  | 発信グルーピングキー                               |
| - recipientGroupingKey | String  | 受信者グルーピングキー                              |

## メッセージ
### メッセージ送信取消

#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値  | タイプ | 説明 |
| --------- | ------ | ------ |
| appkey    | String | 固有のアプリケーションキー |
| requestId | String | リクエストID  |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| recipientSeq | String | X    | 受信者シーケンス番号<br>(入力しない場合、リクエストIDのすべての送信件をキャンセル) |

* 一般/認証メッセージは同じAPIでキャンセルできます。

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| 値        | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### メッセージ結果アップデート照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値         | タイプ | 必須 | 説明                           |
| --------------- | ------- | ---- | ---------------------------------- |
| startUpdateDate | String  | O    | 結果アップデート照会の開始時間(yyyy-MM-dd HH:mm) |
| endUpdateDate   | String  | O    | 結果アップデート照会の終了時間(yyyy-MM-dd HH:mm) |
| pageNum         | Integer | X    | ページ番号(基本：1)                      |
| pageSize        | Integer | X    | 照会件数(基本：15, 最大 : 1000)          |

#### レスポンス
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

| 値                     | タイプ | 説明                                 |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                              |
| - resultCode                | Integer | 結果コード                              |
| - resultMessage             | String  | 結果メッセージ                             |
| - isSuccessful              | Boolean | 成否                               |
| messageSearchResultResponse | Object  | 本文領域                              |
| - messages                  | List    | メッセージリスト                            |
| -- requestId                | String  | リクエストID                                    |
| -- recipientSeq             | Integer | 受信者シーケンス番号                         |
| -- plusFriendId             | String  | プラスフレンドID                                 |
| -- recipientNo              | String  | 受信番号                              |
| -- requestDate              | String  | リクエスト日時                              |
| -- receiveDate              | String  | 受信日時                              |
| -- content                  | String  | 本文                                 |
| -- messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| -- resendStatus             | String  | 再送信ステータスコード                          |
| -- resendStatusName         | String  | 再送信ステータスコード名                             |
| -- resultCode               | String  | 受信結果コード                           |
| -- resultCodeName           | String  | 受信結果コード名                              |
| -- senderGroupingKey        | String  | 発信グルーピングキー                               |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                              |
| - totalCount                | Integer | 総個数                                    |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 大量送信
### 大量送信リクエストリスト照会

#### リクエスト
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey | String | 固有のアプリケーションキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
| X-Secret-Key | String | 固有のシークレットキー |

[Query parameter]
* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | O | 送信日の開始 |
| endRequestDate | String | - | O | 送信日の終了 |
| startCreateDate |	String| - |	O |	登録日の開始 |
| endCreateDate |	String| - |	O |	登録日の終了 |
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean | 成否 |
| body | Object | 本文領域 |
| - messages | Object | メッセージリスト |
| -- requestId | String | リクエストID |
| -- requestDate | String | リクエスト日 |
| -- plusFriendId | String | プラスフレンドID |
| -- senderKey | String | 送信者ID |
| -- masterStatusCode | String | 大量送信ステータスコード(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 内容 |
| -- isAd | Boolean | 広告かどうか |
| -- imageSeq | Integer | 画像の順序 |
| -- imageLink | Boolean | 画像のURL |
| -- fileId | String | 添付ファイルID |
| -- autoSendYn | String | 自動送信を行うかどうか |
| -- statsId | String | 統計ID |
| -- createDate | String | 作成日 |
| -- createUser | String | 登録者(コンソールから送信時、ユーザーUUIDに保存) |
| - totalCount | Integer | 総数


### 大量送信大量送信受信者リスト照会

#### リクエスト
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String |	固有のアプリケーションキー |
| requestId |	String |	リクエストID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
| X-Secret-Key | String | 固有のシークレットキー |


|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | X | 送信日の開始 |
| endRequestDate | String | - | X | 送信日の終了 |
| startCreateDate |	String| - |	X |	登録日の開始 |
| endCreateDate |	String| - |	X |	登録日の終了 |
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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

| 値 | タイプ| 説明 |
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean| 成否 |
| body | Object | 本文領域 |
| - recipients | List | 受信者リスト |
| -- requestId | String | リクエストID |
| -- recipientSeq | Integer | 受信者シーケンス番号 |
| -- recipientNo | String | 受信番号 |
| -- requestDate | String | リクエスト日 |
| -- receiveDate | String | 受信日 |
| -- messageStatus | String | 大量受信者送信ステータスコード(READY, COMPLETED, FAILED, CANCEL) |
| -- resultCode | String | 受信結果コード |
| -- resultCodeName | String | 受信結果コード名 |
| - totalCount | Integer | 総数

### 大量送信大量送信受信者照会

#### リクエスト
[URL]
```
GET /friendtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String | 固有のアプリケーションキー |
| requestId |	String | リクエストID |
| recipientSeq | String | 受信者の順序 |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
|X-Secret-Key|	String|	固有のシークレットキー |


|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| requestId | String | - | O | リクエストID |
| startRequestDate | String | - | X | 送信日の開始 |
| endRequestDate | String | - | X | 送信日の終了 |
| startCreateDate |	String| - |	X |	登録日の開始 |
| endCreateDate |	String| - |	X |	登録日の終了 |


#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/${RECIPIENT_SEQ}?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### レスポンス
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

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean| 成否 |
| body | Object | 本文領域 |
| - requestId | String | リクエストID |
| - recipientSeq | Integer | 受信者シーケンス番号 |
| - plusFriendId | String | プラスフレンドID |
| - senderKey | String | 発信キー(40文字)|
| - recipientNo | String | 受信番号 |
| - requestDate | String | リクエスト日 |
| - receiveDate | String | 受信日 |
| - content | String | 本文 |
| - messageStatus | String | 大量受信者送信ステータスコード(READY, COMPLETED, FAILED, CANCEL) |
| - resendStatus | String |	代替送信ステータスコード(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[以下の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)]参考) |
| - resendStatusName | String |	代替送信ステータスコード名 |
| - resendRequestId | String | 代替送信SMSリクエストID |
| - resendResultCode | String | 代替送信結果コード[SMS結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resultCode | String |	受信結果コード |
| - resultCodeName | String |	受信結果コード名 |
| - imageSeq | Integer | 画像の順序 |
| - imageLink | Integer | 画像のURL |
| - buttons | List | ボタンの順序 |
| -- ordering | String | ボタンの順序 |
| -- type | String | ボタンの種類<br/> - WL：Webリンク<br/> - AL：アプリリンク<br/> - DS：配送照会<br/> - BK：Botキーワード<br/> - MD：メッセージ伝達<br/> - BC：相談トーク切り替え<br/> - BT：Bot切り替え<br/> - AC：チャンネル追加[広告追加/複合型のみ] |
| -- name | String | ボタン名 |
| -- linkMo | String | モバイルWebリンク(WLタイプの場合は必須フィールド) |
| -- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)|
| -- schemeIos | String | IOSアプリリンク(ALタイプの場合は必須フィールド) |
| -- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| -- chatExtra | String | BC：相談トークに切り替える時に伝達するメタ情報<br/> BT：Botに切り替える時に伝達するメタ情報 |
| -- chatEvent | String | BT：Botに切り替える時に接続するBotイベント名 |
| -- bizFormKey|	String|	BF(비즈니스 폼) 타입 버튼 시, 비즈폼 키 |
| -- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
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
| - isAd | Boolean | 広告かどうか |
| - createDate | String | 作成日 |


## イメージの管理

### イメージの登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| 値 | タイプ | 必須 | 説明 |
| ----- | ---- | ---- | ---- |
| image | File | O    | イメージ |
| wide  | boolean | X | ワイドイメージの可否(基本: false) |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

#### レスポンス
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

| 値         | タイプ | 説明               |
| --------------- | ------- | ---------------------- |
| header          | Object  | ヘッダ領域            |
| - resultCode    | Integer | 結果コード            |
| - resultMessage | String  | 結果メッセージ           |
| - isSuccessful  | Boolean | 成否             |
| image           | Object  | 本文領域            |
| - imageSeq      | Integer | イメージ番号(カカともへのメッセージの送信時に使用) |
| - imageUrl      | String  | イメージURL                |
| - imageName     | String  | イメージ名(アップロードしたファイル名)         |

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


### イメージの照会
#### リクエスト

[URL]

```
GET  /friendtalk/v2.3/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明      |
| -------- | ------- | ---- | ------------- |
|imageTypes|	String|	X| - IMAGE: 일반 이미지<br/> - WIDE_IMAGE: 와이드 이미지<br/> - WIDE_ITEMLIST_IMAGE: 와이드 아이템리스트 이미지<br/> - CAROUSEL_IMAGE: 캐러셀 이미지<br/> IMAGE, WIDE_IMAGE(default) |
| pageNum  | Integer | X    | ページ番号(基本：1) |
| pageSize | Integer | X    | 照会件数(基本：15) |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### レスポンス
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
            "createDate" : String
        }
    ],
    "totalCount" : Integer
  }

}
```

| 値         | タイプ | 説明               |
| --------------- | ------- | ---------------------- |
| header          | Object  | ヘッダ領域            |
| - resultCode    | Integer | 結果コード            |
| - resultMessage | String  | 結果メッセージ           |
| - isSuccessful  | Boolean | 成否             |
| imagesResponse  | Object  | 本文領域            |
| - image         | Object  | 本文領域            |
| -- imageSeq     | Integer | イメージ番号(カカともへのメッセージの送信時に使用) |
| -- imageUrl     | String  | イメージURL                |
| -- imageName    | String  | イメージ名(アップロードしたファイル名)         |
| -- imageType     | String |	- IMAGE: 일반 이미지<br/> - WIDE_IMAGE: 와이드 이미지<br/> - WIDE_ITEMLIST_IMAGE: 와이드 아이템리스트 이미지<br/> - CAROUSEL_IMAGE: 캐러셀 이미지<br/> |
| -- createDate   | String  | 作成日時            |
| - totalCount    | Integer | 総個数                  |

* イメージは、最近登録した順にソートされてレスポンスを返します。


### イメージの削除
#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.3/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。 [[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 値  | タイプ | 必須 | 説明 |
| -------- | ------ | ---- | ------ |
| imageSeq | String | O    | イメージ番号 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/images?imageSeq=1,2,3"
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

| 値         | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

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

## 代替送信管理
### SMS AppKey 登録

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{
    "resendAppKey": String
}
```

| 値     | タイプ | 必須 | 説明                                |
|---|---|---|---|
|resendAppKey|	String|	O | 代替発送に設定するSMS AppKey |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```

### 代替送信設定登録

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```

| 値     | タイプ | 必須 | 説明                                |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |


[Request body]

```
{  
   "plusFriendId": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 値                | タイプ | 必須 | 説明                                 |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| isResend             | boolean | O    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| resendSendNo         | String  | O    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@플러스친구","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  }
}
```
