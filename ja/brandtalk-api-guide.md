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

## 一般メッセージ

### メッセージ置換送信リクエスト

[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

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

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|senderKey|	String|	O | 発信キー(40文字) |
|templateCode|	String|	O | 登録した送信テンプレートコード(最大20文字) |
|requestDate| String | X| リクエスト日時(yyyy-MM-dd HH:mm:ss)<br>(入力しない場合は即時送信)<br>最大30日後まで予約可能 |
|senderGroupingKey| String | X| 発信グルーピングキー(最大100文字) |
|createUser| String | X| 登録者(コンソールから送信時にユーザーUUIDで保存)|
|recipientList|	List|	O|	受信者リスト(最大1000人) |
|- recipientNo|	String|	O|	受信番号(最大15文字) |
|- templateParameter|	Object|	X|	テンプレートパラメータ<br>(テンプレートに置換する変数を含む時は、必須) |
|-- key|	String|	X |	置換キー(#{key})|
|-- value| String |	X |	置換キーにマッピングされるValue値|
|- recipientGroupingKey|	String|	X|	受信者グルーピングキー(最大100文字) |

* <b>リクエスト日時は呼び出す時点から90日後まで設定可能です。</b>
* <b>夜間送信制限(20:50～翌日08:00)</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","templateParameter":{"{日本語識別子フィールド}":"{置換データ}"}}]}'
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|message|	Object|	本文領域|
|- requestId | String |	リクエストID |
|- senderGroupingKey | String |	発信グルーピングキー |
|- sendResults | Object | 送信リクエスト結果 |
|-- recipientSeq | Integer | 受信者シーケンス番号 |
|-- recipientNo | String | 受信番号 |
|-- resultCode | Integer | 送信リクエスト結果コード |
|-- resultMessage | String | 送信リクエスト結果メッセージ |
|-- recipientGroupingKey | String | 受信者グルーピングキー |

### メッセージリスト照会

#### リクエスト

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter] 1番or (2番、3番)条件必須

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	条件必須(1番) | リクエストID |
|startRequestDate|	String|	条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)|
|endRequestDate|	String| 条件必須(2番) |	送信リクエスト日終了値(yyyy-MM-dd HH:mm) |
|startCreateDate|  String| 条件必須(3番) | 登録日付開始値(yyyy-MM-dd HH:mm)|
|endCreateDate|  String| 条件必須(3番) | 登録日の最終値(yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	受信番号 |
|senderKey|	String|	X |	発信キー |
|templateCode|	String|	X |	テンプレートコード|
|senderGroupingKey| String | X| 発信グルーピングキー |
|recipientGroupingKey|	String|	X|	受信者グルーピングキー |
|messageStatus| String |	X | リクエスト状態( COMPLETED -> 成功、 FAILED -> 失敗、 CANCEL -> キャンセル)	|
|resultCode| String |	X | 送信結果( BRC01 -> 成功、 BRC02 -> 失敗)	|
|pageNum|	Integer|	X|	ページ番号(Default : 1)|
|pageSize|	Integer|	X|	照会件数(Default : 15、Max : 1000)|

* 90日以前の送信リクエストデータは照会されません。
* 送信リクエスト日時の範囲は最大30日です。

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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|messageSearchResultResponse|	Object|	本文領域|
|- messages | List |	メッセージリスト |
|-- requestId | String |	リクエストID |
|-- recipientSeq | Integer |	受信者シーケンス番号 |
|-- plusFriendId | String |	プラスフレンドID |
|-- senderKey    | String | 発信キー   |
|-- templateCode | String |	テンプレートコード |
|-- recipientNo | String |	受信番号 |
|-- requestDate | String |	リクエスト日時 |
|-- createDate | String | 登録日時 |
|-- receiveDate | String |	受信日時 |
|-- content | String |	本文 |
|-- messageStatus | String |	リクエスト状態( COMPLETED -> 成功、 FAILED -> 失敗、 CANCEL -> キャンセル) |
|-- resultCode | String |	受信結果コード |
|-- resultCodeName | String |	受信結果コード名 |
|-- createUser | String | 登録者(コンソールから送信する時、ユーザーUUIDで保存) |
|-- senderGroupingKey | String | 発信グルーピングキー |
|-- recipientGroupingKey | String |	受信者グルーピングキー |
|- totalCount | Integer | 総数 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

### メッセージ単件照会

#### リクエスト

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー |
|requestId|	String|	リクエストID |
|recipientSeq|	Integer|	受信者シーケンス番号 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|message|	Object|	メッセージ|
|- requestId | String |	リクエストID |
|- recipientSeq | Integer |	受信者シーケンス番号 |
|- plusFriendId | String |	プラスフレンドID |
|- senderKey    | String | 発信キー   |
|- templateCode | String |	テンプレートコード |
|- recipientNo | String |	受信番号 |
|- requestDate | String |	リクエスト日時 |
|- createDate | String | 登録日時 |
|- receiveDate | String |	受信日時 |
|- content | String |	本文 |
|- buttons | List |	ボタンリスト |
|-- name | String |	ボタン名 |
|-- type | String |	ボタンタイプ(WL:Webリンク、 AL:アプリリンク、 BK:Botキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BF:ビジネスフォーム) |
|-- linkMo | String |	モバイルWebリンク(WLタイプの場合は必須フィールド) |
|-- linkPc | String |	PC Webリンク(WLタイプの場合は任意フィールド) |
|-- schemeAndroid | String |	Androidアプリリンク(ALタイプの場合は必須フィールド) |
|-- schemeIos | String |	iOSアプリリンク(ALタイプの場合は必須フィールド) |
|-- bizFormId|	Integer|	ボタンクリック時に実行するビジネスフォームID |
|- messageStatus | String |	リクエスト状態( COMPLETED -> 成功、 FAILED -> 失敗、 CANCEL -> キャンセル) |
|- resultCode | String |	受信結果コード |
|- resultCodeName | String |	受信結果コード名 |
|- createUser | String | 登録者(コンソールから送信する時、ユーザーUUIDで保存) |
|- senderGroupingKey | String | 発信グルーピングキー |
|- recipientGroupingKey | String |	受信者グルーピングキー |

## メッセージ
### メッセージ送信キャンセル

#### リクエスト

[URL]

```
DELETE  /brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー|
|requestId| String| リクエストID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|recipientSeq|	String|	X | 受信者シーケンス番号<br>(入力しない場合はリクエストIDのすべての送信をキャンセル) |

* 一般/認証メッセージともに同じAPIでキャンセルできます。

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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

## テンプレート

### テンプレート登録
#### リクエスト
[URL]

```
POST  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー |
|senderKey|	String|	発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Request parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|templateCode|	String |	O | テンプレートコード(最大20文字) |
|templateName|	String |	O | テンプレート名(最大150文字) |
|messageType| String | X |テンプレートメッセージタイプ(I:イメージ型、W:ワイド型) |
|contentType| String | X |テキストタイプ(F:固定型、V:変数型) |
|unsubscribeContent|	String |	O | 無料受信拒否電話番号/認証番号 |
|templateContent|	String |	O | テンプレート本文(最大1000文字) |
|templateImageLink | String |	X | テンプレートイメージリンク |
|image | String |	X | イメージURL |
| buttons[i].type|	String |	X | ボタンタイプ(WL:Webリンク、 AL:アプリリンク、 BK:Botキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BF:ビジネスフォーム) |
| buttons[i].name| String |	X |	ボタン名(ボタンがある場合は必須、最大14文字)|
| buttons[i].linkMo| String |	X |	モバイルWebリンク(WLタイプの場合は必須フィールド、最大500文字)|
| buttons[i].linkPc | String |	X |PC Webリンク(WLタイプの場合は任意フィールド、最大500文字) |
| buttons[i].schemeAndroid | String | X |	Androidアプリリンク(ALタイプの場合は必須フィールド、最大500文字) |
| buttons[i].schemeIos | String | X |	iOSアプリリンク(ALタイプの場合は必須フィールド、最大500文字) |
| buttons[i].bizFormId | Integer | X |	ボタンクリック時に実行するビジネスフォームID |

* contentTypeが変数型(V)の場合、テンプレート内容(templateContent)に変数入力可能
* 変数名は最大20文字以内、韓国語/英字/数字/許可された特殊記号('-', '_')のみ入力可能
* 最大20個の変数名を入力可能(重複除外)
* 無料受信拒否電話番号に認証番号がない場合(例：080-1111-2222)
* 無料受信拒否電話番号に認証番号がある場合は /で区切って入力(例：080-1111-2222/12345)
* 認証番号は数字1～10桁です。

* イメージ型はボタン数最大5個、ワイド型はボタン数最大2個まで可能
* ACボタンが含まれる場合、イメージ型は最初のボタン / ワイド型は最後のボタンの順序通りに入力
* ACボタンはボタン名が""チャンネル追加""に固定
* contentTypeが変数型(V)の場合、リンク(linkMo, linkPc, schemeAndroid, schemeIos)に変数入力可能。
* BFボタンのみ含まれる場合、イメージ型は最初のボタン / ワイド型は最後のボタンの順序通りに入力
* ACボタンとBFボタンが同時に使われる場合のBFボタンは、イメージ型は2番目のボタン / ワイド型は最初のボタンの順序通りに入力
* BFボタンはボタン名が""トークで予約する"", ""トークでアンケートする"", ""トークで応募する""のいずれか1つを使用しなければならない
* BFボタンリンクはカカオforビジネスで作成したビジネスフォームIDで登録が可能で、以下の条件が有効でなければならない
  1)ビジネスフォーム登録管理者とチャンネル管理者が一致していること(管理者権限は無関係)
  2)ビジネスフォーム状態が作成完了 / 実行であり、終了日が登録日より先でなければならない
  3)ビジネスフォーム内チャンネル追加オプションがある場合、メッセージ送信チャンネルと一致しなければならない

* image詳細
  * 基本イメージポリシー
    * 推奨サイズ： 800px * 400px
    * 制限サイズ：横500px以上。縦：縦比率2：1以上3：4以下。
    * サポートファイル： jpg.png / 最大2MB

  * ワイド吹き出しイメージポリシー
    * 推奨サイズ： 800px * 600px
    * 制限サイズ：横500px以上。横：縦比率2：1以上1：1以下。
    * ファイル形式およびサイズ： jpg, png / 最大2MB"

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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|

### テンプレート照会

#### リクエスト

[URL]

```
GET  /brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリケーションキー|
|senderKey|	String|	発信キー |
|templateCode|	String|	テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できる。[[参考](./sender-console-guide/#x-secret-key)] |

[Query parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|templateCode|	String|	X |	テンプレートコード|
|templateName|	String|	X |	テンプレート名|
|kakaoStatus| String |	X | テンプレートステータスコード|
|pageNum|	Integer|	X|	ページ番号(Default : 1)|
|pageSize|	Integer|	X|	照会件数(Default : 15、Max : 1000)|

|テンプレートステータスコード| 説明|
|---|---|
| TSC01 | リクエスト |
| TSC02 | 検収中 |
| TSC03 | 承認 |
| TSC04 | 差し戻し |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brandtalk/v2.2/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

#### レスポンス
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|template|	Object|	本文領域|
|- plusFriendId | String |	カカオトークチャンネル検索用IDまたは発信プロフィールグループ名 |
|- senderKey    | String | 発信キー |
|- templateCode | String |	テンプレートコード |
|- templateName | String |	テンプレート名 |
|- messageType| String | テンプレートメッセージタイプ(I:イメージ型、 W:ワイド型) |
|- contentType| String | テキストタイプ(F:固定型、 V:変数型) |
|- templateContent | String |	テンプレート本文 |
|- unsubscribeContent | String | 無料受信拒否電話番号/認証番号 |
|- templateImageLink | String | テンプレートイメージリンク |
|- templateImageUrl | String |	イメージURL |
|- buttons | List |	ボタンリスト |
|-- name | String |	ボタン名 |
|-- type | String |	ボタンタイプ(WL:Webリンク、 AL:アプリリンク、 BK:Botキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BF:ビジネスフォーム) |
|-- linkMo | String |	モバイルWebリンク(WLタイプの場合は必須フィールド) |
|-- linkPc | String |	PC Webリンク(WLタイプの場合は任意フィールド) |
|-- schemeAndroid | String |	Androidアプリリンク(ALタイプの場合は必須フィールド) |
|-- schemeIos | String |	iOSアプリリンク(ALタイプの場合は必須フィールド) |
|-- bizFormId | Integer |	ボタンクリック時に実行するビジネスフォームID |
|- kakaoStatus | String | テンプレート状態(A:正常、 R:待機(送信前)、S:中断) |
|- createDate | String | 作成日時 |
|- updateDate | String | 修正日時 |
