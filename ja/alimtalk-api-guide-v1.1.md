## Notification > KakaoTalk Bizmessage > お知らせトーク > API v1.1 Guide

## お知らせトーク

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

## メッセージ

### メッセージ置換送信リクエスト

[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "isResend" : boolean,
        "resendType" : String,
        "resendTitle" : String,
        "resendContent" : String,
        "resendSendNo" : String
    }]
}
```

| 値             | タイプ | 必須 | 説明                                 |
| ------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId        | String  | X    | プラスフレンドID(最大30文字)                         |
| templateCode        | String  | O    | 登録した送信テンプレートコード(最大20文字)                    |
| requestDate         | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信) |
| recipientList       | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo       | String  | O    | 受信番号(最大15桁)                            |
| - templateParameter | Object  | X    | テンプレートパラメータ<br>(テンプレートに置換する変数が含まれる時は必須)       |
| -- key              | String  | X    | 置換キー(#{key})                             |
| -- value            | String  | X    | 置換キーにマッピングされるvalue値                 |
| - isResend          | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| - resendType        | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決められます。 |
| - resendTitle       | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| - resendContent     | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| - resendSendNo      | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |

* <b>プラスフレンドIDフィールドを送信しない場合、最初に登録したプラスフレンドに送信されます。</b>
* <b>リクエスト日時は呼び出す時点から90日後まで設定可能です。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages -d '{"plusFriendId":"{プラスフレンドID}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","templateParameter":{"{日本語識別子フィールド}":"{置換データ}"}}]}'
```

### メッセージ全文送信リクエスト

[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/raw-messages
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
    "plusFriendId": String,
    "templateCode": String,
    "requestDate": String,
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
            ],
            "isResend" : boolean,
            "resendType" : String,
            "resendTitle" : String,
            "resendContent" : String,
            "resendSendNo" : String
        }
    ]
}
```

| 値          | タイプ | 必須 | 説明                                 |
| ---------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId     | String  | X    | プラスフレンドID(最大30文字)                         |
| templateCode     | String  | O    | 登録した送信テンプレートコード(最大20文字)                    |
| requestDate      | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信) |
| recipientList    | List    | O    | 受信者リスト(最大1,000人)                        |
| - recipientNo    | String  | O    | 受信番号(最大15桁)                            |
| - content        | String  | O    | 内容(最大1000文字)                             |
| - buttons        | List    | X    | ボタンリスト(最大5個)                             |
| -- ordering      | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type          | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| -- name          | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -- linkMo        | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大200文字)       |
| -- linkPc        | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大200文字)         |
| -- schemeIos     | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大200文字)       |
| -- schemeAndroid | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大200文字)   |
| - isResend       | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| - resendType     | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが決められます。 |
| - resendTitle    | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合、プラスフレンドIDで再送信されます。) |
| - resendContent  | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合、テンプレートの内容で再送信されます。) |
| - resendSendNo   | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |


* <b>プラスフレンドIDフィールドを送信しない場合、最初に登録したプラスフレンドに送信されます。</b>
* <b>本文とボタンに置換が完了したデータを入れてください。</b>
* <b>リクエスト日時は呼び出す時点から90日後まで設定可能です。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/raw-messages -d '{"plusFriendId":"{プラスフレンドID}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","content":"{内容}","buttons":[{"ordering":"{ボタン順序}","type":"{ボタンタイプ}","name":"{ボタン名}","linkMo":"{モバイルWebリンク}"}]}]}'
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
    "requestId": String
  }
}
```

| 値         | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
| message         | Object  | 本文領域 |
| - requestId     | String  | リクエストID  |


### メッセージ送信取消

#### リクエスト

[URL]

```
DELETE  /alimtalk/v1.1/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値   | タイプ | 説明 |
| --------- | ------ | ------ |
| appkey    | String | 固有のアプリケーションキー |
| requestId | String | リクエストID  |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| recipientSeq | String | X    | 受信者シーケンス番号<br>(入力しない場合、リクエストIDのすべての送信件をキャンセル) |

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

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### メッセージリストの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1番or 2番の条件は必須

| 値          | タイプ | 必須  | 説明                                 |
| ---------------- | ------- | --------- | ---------------------------------------- |
| requestId        | String  | 条件必須(1番) | リクエストID                                    |
| startRequestDate | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)           |
| endRequestDate   | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)            |
| recipientNo      | String  | X         | 受信番号                              |
| plusFriendId     | String  | X         | プラスフレンドID                                 |
| templateCode     | String  | X         | テンプレートコード                             |
| messageStatus    | String  | X         | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| resultCode       | String  | X         | 送信結果(MRC01 -> 成功MRC02 -> 失敗)          |
| pageNum          | Integer | X         | ページ番号(基本：1)                            |
| pageSize         | Integer | X         | 照会件数(基本：15, 最大 : 1000)                |

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

| 値                     | タイプ | 説明                                 |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                              |
| - resultCode                | Integer | 結果コード                              |
| - resultMessage             | String  | 結果メッセージ                             |
| - isSuccessful              | Boolean | 成否                               |
| messageSearchResultResponse | Object  | 本文領域                              |
| - messages                  | List    | メッセージリスト                             |
| -- requestId                | String  | リクエストID                                    |
| -- recipientSeq             | Integer | 受信者シーケンス番号                         |
| -- plusFriendId             | String  | プラスフレンドID                                 |
| -- templateCode             | String  | テンプレートコード                             |
| -- recipientNo              | String  | 受信番号                              |
| -- content                  | String  | 本文                                 |
| -- requestDate              | String  | リクエスト日時                              |
| -- receiveDate              | String  | 受信日時                              |
| -- resendStatus             | String  | 再送信ステータスコード                          |
| -- resendStatusName         | String  | 再送信ステータスコード名                          |
| -- messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| -- resultCode               | String  | 受信結果コード                           |
| -- resultCodeName           | String  | 受信結果コード名                           |
| -- buttons                  | List    | ボタンリスト                              |
| --- ordering                | Integer | ボタン順序                              |
| --- type                    | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| --- name                    | String  | ボタン名                              |
| --- linkMo                  | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| --- linkPc                  | String  | PC Webリンク(WLタイプの場合は任意フィールド)                  |
| --- schemeIos               | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| --- schemeAndroid           | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| - totalCount                | Integer | 総個数                                |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS再送信ステータス
| 値 | 説明                        |
| ----- | ------------------------------- |
| RSC01 | 再送信の対象ではない                   |
| RSC02 | 再送信の対象(送信結果が失敗の時、再送信が行われます。) |
| RSC03 | 再送信中                      |
| RSC04 | 再送信成功                    |
| RSC05 | 再送信失敗                    |

### メッセージ単件照会

#### リクエスト

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/message/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明   |
| ------------ | ------- | ---------- |
| appkey       | String  | 固有のアプリケーションキー |
| requestId    | String  | リクエストID      |
| recipientSeq | Integer | 受信者シーケンス番号 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/message/{requestId}/{recipientSeq}"
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
}
```

| 値            | タイプ | 説明                                 |
| ------------------ | ------- | ---------------------------------------- |
| header             | Object  | ヘッダ領域                              |
| - resultCode       | Integer | 結果コード                              |
| - resultMessage    | String  | 結果メッセージ                             |
| - isSuccessful     | Boolean | 成否                               |
| message            | Object  | メッセージ                                |
| - requestId        | String  | リクエストID                                    |
| - recipientSeq     | Integer | 受信者シーケンス番号                         |
| - plusFriendId     | String  | プラスフレンドID                                 |
| - templateCode     | String  | テンプレートコード                             |
| - recipientNo      | String  | 受信番号                              |
| - content          | String  | 本文                                 |
| - requestDate      | String  | リクエスト日時                              |
| - receiveDate      | String  | 受信日時                              |
| - resendStatus     | String  | 再送信ステータスコード                          |
| - resendStatusName | String  | 再送信ステータスコード名                           |
| - messageStatus    | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| - resultCode       | String  | 受信結果コード                           |
| - resultCodeName   | String  | 受信結果コード名                            |
| - buttons          | List    | ボタンリスト                              |
| -- ordering        | Integer | ボタン順序                              |
| -- type            | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| -- name            | String  | ボタン名                              |
| -- linkMo          | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc          | String  | PC Webリンク(WLタイプの場合は任意フィールド)                  |
| -- schemeIos       | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid   | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |

## プラスフレンド

### プラスフレンドカテゴリーの照会

#### リクエスト
[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/categories
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

#### レスポンス
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

| 値          | タイプ | 説明 |
| ---------------- | ------- | ------- |
| header           | Object  | ヘッダ領域 |
| - resultCode     | Integer | 結果コード |
| - resultMessage  | String  | 結果メッセージ |
| - isSuccessful   | Boolean | 成否 |
| categories       | Object  | カテゴリー |
| - parentCode     | String  | 親コード |
| - depth          | Integer | カテゴリーの深さ |
| - code           | String  | カテゴリーコード |
| - name           | String  | カテゴリー名 |
| - subCategories  | Object  | サブカテゴリー |
| -- parentCode    | String  | 親コード |
| -- depth         | Integer | カテゴリーの深さ |
| -- code          | String  | カテゴリーコード |
| -- name          | String  | カテゴリー名 |
| -- subCategories | Object  | サブカテゴリー |
| --- parentCode   | String  | 親コード |
| --- depth        | Integer | カテゴリーの深さ |
| --- code         | String  | カテゴリーコード |
| --- name         | String  | カテゴリー名 |

### プラスフレンド事業者登録証のアップロード
#### リクエスト
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/business-licenses
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "fileName" : String,
  "fileBody" : "{byte[] -> Base64エンコードした値}"
}
```

| 値  | タイプ | 必須 | 説明                                 |
| -------- | ------ | ---- | ---------------------------------------- |
| fileName | String | O    | ファイル名                              |
| fileBody | Byte[] | O    | ファイルbyte[]をBase64にエンコードした値(最大500KB)<br>またはbyte配列値 |

#### レスポンス
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "attachFile" : {
    "fileSeq" : Integer
  }
}
```

| 値         | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
| attachFile      | Object  | 添付ファイル |
| - fileSeq       | Integer | ファイルシーケンス |

### プラスフレンドの登録
#### リクエスト
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "plusFriendId" : String,
  "phoneNo" : String,
  "categoryCode" : String
}
```

| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------- | ---- | ---------------------------------------- |
| plusFriendId | String  | O    | プラスフレンドID(最大30文字)                         |
| phoneNo      | String  | O    | 管理者の携帯電話番号(最大15桁)                       |
| categoryCode | String  | O    | カテゴリーコード(11桁)<br>カテゴリー照会APIのレスポンス参考<br>ex) 00100010001健康(001) - 病院(0001) - 総合病院(0001) |

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

### プラスフレンドトークン認証
#### リクエスト
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/tokens
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |
| plusFriendId | String | プラスフレンドID |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "token" : "Integer"
}
```

| 値 | タイプ | 必須 | 説明                                 |
| ----- | ------- | ---- | ---------------------------------------- |
| token | Integer | O    | 認証トークン(プラスフレンド登録API呼び出し後、カカオトークアプリで受け取った認証トークン) |

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

### プラスフレンドリストの照会
#### リクエスト

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/plus-friends
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter] 1番or 2番の条件は必須

| 値             | タイプ | 必須 | 説明                                 |
| ------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId        | String  | X    | プラスフレンドID                                 |
| status              | String  | X    | プラスフレンドステータスコード <br>(YSC02：トークン認証待機中、YSC03：正常登録) |
| isSearchKakaoStatus | boolean | X    | カカオステータスを照会するかどうか(falseの場合、カカオステータス関連フィールド(kakaoStatus、kakaoProfileStatusなど) null値)<br>default値：true |

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "plusFriends" : [
    {
      "plusFriendId" : String,
      "plusFriendType" : String,
      "senderKey" : String,
      "categoryCode" : String,
      "alimtalkDailyMaxCount" : Integer,
      "friendtalkDailyMaxCount" : Integer,
      "alimtalkSentCount" : Integer,
      "friendtalkSentCount" : Integer,
      "status" : String,
      "statusName" : String,
      "kakaoStatus" : String,
      "kakaoStatusName" : String,
      "kakaoProfileStatus" : String,
      "kakaoProfileStatusName" : String,
      "resendYn" : String,
      "smsSendNo" : Integer,
      "createDate" : String
    }
  ],
  "totalCount" : Integer
  }
}
```

| 値                   | タイプ | 説明                                 |
| ------------------------- | ------- | ---------------------------------------- |
| header                    | Object  | ヘッダ領域                              |
| - resultCode              | Integer | 結果コード                              |
| - resultMessage           | String  | 結果メッセージ                             |
| - isSuccessful            | Boolean | 成否                               |
| plusFriends               | Object  | プラスフレンド                              |
| - plusFriendId            | String  | プラスフレンドID                                 |
| - plusFriendType          | String  | プラスフレンドタイプ(NORMAL、GROUP)                  |
| - senderKey               | String  | 発信キー                                  |
| - categoryCode            | String  | カテゴリーコード                            |
| - alimtalkDailyMaxCount   | Integer | お知らせトークの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
| - friendtalkDailyMaxCount | Integer | カカともへのメッセージの一日最大送信件数<br>(値が0の場合、件数制限なし)    |
| - alimtalkSentCount       | Integer | お知らせトークの一日送信件数<br>(値が0の場合、件数制限なし)       |
| - friendtalkSentCount     | Integer | カカともへのメッセージの一日送信件数<br>(値が0の場合、件数制限なし)       |
| - status                  | String  | TOASTプラスフレンドステータスコード <br>(YSC02：登録待機中、YSC03：正常登録) |
| - statusName              | String  | TOASTプラスフレンドステータス名(登録待機中、正常登録)           |
| - kakaoStatus             | String  | カカオプラスフレンドステータスコード<br>(A：正常、S：遮断、D：削除)<br>statusがYSC02の場合、kakaoStatus null値を持ちます。 |
| - kakaoStatusName         | String  | カカオプラスフレンドステータス名(正常、遮断、削除)<br>statusがYSC02の場合、kakaoStatusName null値を持ちます。 |
| - kakaoProfileStatus      | String  | カカオプラスフレンドプロフィールステータスコード<br>(A：有効化、B：遮断、C：無効化、D：削除E：削除処理中)<br>statusがYSC02の場合、kakaoProfileStatus null値を持ちます。 |
| - kakaoProfileStatusName  | String  | カカオプラスフレンドプロフィールステータス名(有効化、無効化、遮断、削除処理中、削除)<br>statusがYSC02の場合、kakaoProfileStatusName null値を持ちます。 |
| - resendYn                | String  | 送信失敗設定(再送信)するかどうか                     |
| - smsSendNo               | String  | 再送信時、tc-sms発信番号                |
| - createDate              | String  | 登録日時                              |
| totalCount                | Integer | 総個数                                 |

## テンプレート

### テンプレートの登録
#### リクエスト
[URL]

```
POST  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |
| plusFriendId | String | プラスフレンドID |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "templateCode" : String,
  "templateName" : String,
  "templateContent" : String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
    }
  ]
}
```

| 値         | タイプ | 必須 | 説明                                 |
| --------------- | ------- | ---- | ---------------------------------------- |
| templateCode    | String  | O    | テンプレートコード(最大20文字)                           |
| templateName    | String  | O    | テンプレート名(最大20文字)                             |
| templateContent | String  | O    | テンプレート本文(最大1000文字)                         |
| buttons         | List    | X    | ボタンリスト(最大5個)                             |
| -ordering       | Integer | X    | ボタン順序(1～5)                               |
| -type           | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| -name           | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -linkMo         | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大200文字)       |
| -linkPc         | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大200文字)         |
| -schemeIos      | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大200文字)       |
| -schemeAndroid  | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大200文字)   |

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

### テンプレートの修正
#### リクエスト
[URL]

```
PUT  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |
| plusFriendId | String | プラスフレンドID |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "templateName" : String,
  "templateContent" : String,
  "buttons" : [
    {
      "ordering" : Integer,
      "type" : String,
      "name" : String,
      "linkMo" : String,
      "linkPc" : String,
      "schemeIos" : String,
      "schemeAndroid" : String
    }
  ]
}
```

| 値         | タイプ | 必須 | 説明                                 |
| --------------- | ------- | ---- | ---------------------------------------- |
| templateName    | String  | O    | テンプレート名(最大20文字)                             |
| templateContent | String  | O    | テンプレート本文(最大1000文字)                         |
| buttons         | List    | X    | ボタンリスト(最大5個)                             |
| -ordering       | Integer | X    | ボタン順序(1～5)                               |
| -type           | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| -name           | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -linkMo         | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大200文字)       |
| -linkPc         | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大200文字)         |
| -schemeIos      | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大200文字)       |
| -schemeAndroid  | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大200文字)   |

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

### テンプレートの削除
#### リクエスト
[URL]

```
DELETE  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |
| plusFriendId | String | プラスフレンドID |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
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

### テンプレートの問い合わせをする
#### リクエスト
[URL]

```
PUT  /alimtalk/v1.1/appkeys/{appkey}/plus-friends/{plusFriendId}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリケーションキー |
| plusFriendId | String | プラスフレンドID |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Request body]

```
{
  "comment" : String
}
```

| 値 | タイプ | 必須 | 説明 |
| ------- | ------ | ---- | ----- |
| comment | String | O    | お問い合わせ内容 |

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

### テンプレートリストの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v1.1/appkeys/{appkey}/templates
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
| X-Secret-Key | String | O    | コンソールで作成できる。[[参考](./plus-friend-console-guide/#x-secret-key)] |

[Query parameter]

| 値        | タイプ | 必須 | 説明      |
| -------------- | ------- | ---- | ------------- |
| plusFriendId   | String  | X    | プラスフレンドID      |
| templateCode   | String  | X    | テンプレートコード  |
| templateName   | String  | X    | テンプレート名  |
| templateStatus | String  | X    | テンプレートステータスコード |
| pageNum        | Integer | X    | ページ番号(基本：1) |
| pageSize       | Integer | X    | 照会件数(基本：15, 最大 : 1000) |

| テンプレートステータスコード | 説明 |
| --------- | ---- |
| TSC01     | リクエスト |
| TSC02     | 検収中 |
| TSC03     | 承認 |
| TSC04     | 差し戻し |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v1.1/appkeys/{appkey}/templates?plusFriendId={プラスフレンドID}&templateStatus={テンプレートステータスコード}"
```

#### レスポンス
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
              "plusFriendType": String,
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

| 値              | タイプ | 説明                                 |
| -------------------- | ------- | ---------------------------------------- |
| header               | Object  | ヘッダ領域                              |
| - resultCode         | Integer | 結果コード                              |
| - resultMessage      | String  | 結果メッセージ                             |
| - isSuccessful       | Boolean | 成否                               |
| templateListResponse | Object  | 本文領域                              |
| - templates          | List    | テンプレートリスト                            |
| -- plusFriendId      | String  | プラスフレンドID                                 |
| -- plusFriendType    | String  | プラスフレンドタイプ(NORMAL、GROUP)                  |
| -- templateCode      | String  | テンプレートコード                             |
| -- templateName      | String  | テンプレート名                               |
| -- templateContent   | String  | テンプレート本文                             |
| -- buttons           | List    | ボタンリスト                              |
| --- ordering         | Integer | ボタン順序(1～5)                               |
| --- type             | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達) |
| --- name             | String  | ボタン名                              |
| --- linkMo           | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| --- linkPc           | String  | PC Webリンク(WLタイプの場合は任意フィールド)                  |
| --- schemeIos        | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| --- schemeAndroid    | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- comments          | List    | 検収結果                              |
| --- id               | Integer | お問い合わせID                                    |
| --- content          | String  | お問い合わせ内容                              |
| ---userName          | String  | 作成者                                 |
| ---createAt          | String  | 登録日                              |
| ---status            | String  | 応答状態(INQ：お問い合わせ、APR：承認、REJ：差し戻し、REP：返信) |
| -- status            | String  | テンプレートのステータス                             |
| -- statusName        | String  | テンプレートのステータス名                             |
| -- createDate        | String  | 作成日時                              |
| - totalCount         | Integer | 総個数                                |
