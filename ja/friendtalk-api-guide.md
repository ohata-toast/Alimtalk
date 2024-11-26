## Notification > KakaoTalk Bizmessage > カカともへのメッセージ > API v2.4 Guide

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

## v2.4 API紹介
1. カカともへのメッセージコマース型、カルーセルコマース型、プレミアムビデオ型、成人向けメッセージ設定機能が追加されました。
2. カルーセルコマース画像登録APIが追加されました。



## メッセージの送信
#### 送信リクエスト

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同じkeyで要求すると、その要求を失敗処理します。 |

* <b>リクエスト日時は呼び出した時点から60日後まで設定可能です。</b>
* <b>夜間送信制限(20:50~翌日08:00)</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信API仕様に従ってフィールドを入力する必要があります。(SMSサービスに登録された発信番号、 080受信拒否番号、各種フィールドの長さ制限など)</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は切り捨てられ、代替送信される場合があります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)</b>
* <b>カカともへのメッセージ広告メッセージは広告SMS APIで代替送信されるため、必ず080受信拒否番号を登録する必要があります。</b>
* <b>カカともへのメッセージ広告メッセージのresendContentフィールドを入力する場合、 SMS広告APIの <span style="color:red">広告文言</span>を必ず入力すると正常に代替送信されます。 `(広告)内容[無料受信拒否]080XXXXXXX`</b>
* <b>カカともへのメッセージ広告メッセージのresendContentフィールドがない場合、登録された080受信拒否番号に<span style="color:red">広告文言</span>を自動作成して代替送信されます。</b>
* <b>ワイドアイテムリスト型、カルーセルフィード型は広告送信のみ可能です。</b>
* <b>クーポンのlinkMo必須値残りオプション値またはチャンネルクーポンURL(フォーマット: alimtalk=coupon://)使用 - scheme_androidまたはscheme_iosのいずれか必須値残りオプション値</b>

#### テキスト型送信リクエスト

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
| recipientList          | List    | O    | 受信者リスト(最大1,000人)                         |
| - recipientNo          | String  | O    | 受信番号                              |
| - content              | String  | O    | 内容(最大1,000文字)<br>イメージを含む時は最大400文字  |
| - buttons              | List    | X    | ボタン                                 |
| -- ordering            | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type                | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達) |
| -- name                | String  | X    | ボタン名(ボタンがある場合は必須, 最大28文字、ワイドアイテムリストタイプの場合9文字）           |
| -- linkMo              | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド)                |
| -- schemeIos           | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | X    | BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報 |
| -- chatEvent           | String  | X    | BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名 |
|-- bizFormKey|	String|	X| BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                                                                                                                               |
| -- target              | String  | X    |	Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信 |
| - coupon | Object | X | クーポン | 
| -- title| String |	X |	titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン"|
| -- description| String |	X |	クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)|
| -- linkMo| String |	X |	モバイルWebリンク(下部の必須条件確認) |
| -- linkPc | String |	X |PC Webリンク |
| -- schemeIos | String | X |	iOSアプリリンク |
| -- schemeAndroid | String | X |	Androidアプリリンク |
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

#### 画像型 / ワイド画像型送信リクエスト

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
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前               |	タイプ|	必須| 	説明                                                                                                                                                    |
|------------------------|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              |	String|	O | 発信キー(40文字)                                                                                                                                                    |
| requestDate            |	String|	X | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                                          |
| senderGroupingKey      | String | X| 発信グルーピングキー(最大100文字)                                                                                                                                            |
| createUser             | String | X | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                                  |
| recipientList          |	List|	O| 	受信者リスト(最大1,000人)                                                                                                                                            |
| - recipientNo          |	String|	O| 	受信番号                                                                                                                                                 |
| - content              |	String|	O| 内容(最大1,000文字)<br>画像送信時、最大400文字<br>ワイド画像送信時、最大76文字                                                                                              |
| - imageUrl             |	String|	O| 	画像URL                                                                                                                                                     |
| - imageLink            |	String|	X| 	画像リンク                                                                                                                                                |
| - buttons              |	List|	X| 	ボタン(最大5個、クーポンが含まれる場合は最大4個)<br>ワイド画像送信時はリンクボタン最大2個                                                                                                              |
| -- ordering            |	Integer|	X | 	ボタン順序(ボタンがある場合は必須)                                                                                                                                         |
| -- type                | String |	X | 	ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF：ビジネスフォーム)
| -- name                | String |	X | 	ボタン名(ボタンがある場合は必須）                                                                                                                                         |
| -- linkMo              | String |	X | 	モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                                   |
| -- linkPc              | String |	X | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                                     |
| -- schemeIos           | String | X | 	iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                   |
| -- schemeAndroid       | String | X | 	Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                 |
| -- chatExtra           |	String|	X| BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報 
| -- chatEvent           |	String|	X| BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名                                                                                                                           |
| -- bizFormKey          |	String|	X| BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                                                                                                                               |
| -- target              |	String|	X | 	Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信                                                                                             |
| - coupon               | Object | X | クーポン                                                                                                                                                     | 
| -- title               | String |	X | 	titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| -- description         | String |	X | 	クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                                      |
| -- linkMo              | String |	X | 	モバイルWebリンク(下部の必須条件確認)                                                                                                                                      |
| -- linkPc              | String |	X | PC Webリンク                                                                                                                                                |
| -- schemeIos           | String | X | 	iOSアプリリンク                                                                                                                                              |
| -- schemeAndroid       | String | X | 	Androidアプリリンク                                                                                                                                            |
| - resendParameter      |	Object|	X| 代替送信情報                                                                                                                                               |
| -- isResend            |	boolean|	X| 	送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                                       |
| -- resendType          |	String|	X| 	代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                                     |
| -- resendTitle         |	String|	X| 	LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                               |
| -- resendContent       |	String|	X| 	代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                                         |
| -- resendSendNo        | String| X| 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                                |
| -- resendUnsubscribeNo | String| X| 代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                                  |
| - isAd                 | Boolean | X | 	広告かどうか(デフォルト値true)                                                                                                                                             |
| - adult                | Boolean | X | 成人向けメッセージかどうか(デフォルト値false)                                                                                                                           |
| - recipientGroupingKey |	String|	X| 	受信者グルーピングキー(最大100文字)                                                                                                                                          |
| statsId                | String |	X | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                                           |

#### ワイドアイテムリスト型送信リクエスト

[Request Body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
    "recipientList": [{
        "recipientNo": String,
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
            },
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            },
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
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前               | 	タイプ | 	必須 | 	説明                                                                                                                                       |
|------------------------|------|---|-------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | 発信キー(40文字)                                                                                                                                       |
| requestDate            | String | X | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                             |
| senderGroupingKey      | String | X | 発信グルーピングキー(最大100文字)                                                                                                                               |
| createUser             | String | X | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                     |
| recipientList          | List | 	O | 受信者リスト(最大1,000人)                                                                                                                                |
| - recipientNo          | String | O | 受信番号                                                                                                                                     |
| - buttons              | List | 	X | ボタン<br>ワイド画像送信時、リンクボタン最大2個                                                                                                           |
| -- ordering            | Integer | X | ボタン順序(ボタンがある場合は必須)                                                                                                                             |
| -- type                | String | X | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
| -- name                | String | X | ボタン名(ボタンがある場合は必須）                                                                                                                             |
| -- linkMo              | String | X | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                       |
| -- linkPc              | String | X | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                        |
| -- schemeIos           | String | X | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                       |
| -- schemeAndroid       | String | X | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                     |
| -- chatExtra           | String | X | BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報                                                                                                   |
| -- chatEvent           | String | X | BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名                                                                                                              |
| -- bizFormKey          | String | X | BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                                                                                                                  |
| -- target              | String | X | Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信                                                                                 |
| - header               | String | O | ヘッダ(ワイドアイテムリストメッセージタイプ使用時、必須、最大25文字)                                                                                                          |
| - item                 | Object | O | ワイドアイテム                                                                                                                                    |
| -- list                | List | O | ワイドアイテムリスト(最小3個、最大4個)                                                                                                                         |
| --- title              | String | O | アイテムタイトル(最初のアイテムの場合は最大25文字、 2～4番目のアイテムの場合は最大30文字)                                                                                                |
| --- imageUrl           | String | O | アイテム画像URL                                                                                                                                     |
| --- linkMo             | String | O | モバイルWebリンク                                                                                                                                   |
| ---------------------------------------- linkPc             | String | X | PC Webリンク                                                                                                                                   |
| ---------------------------------------- schemeIos          | String | X | iOSアプリリンク                                                                                                                                  |
| ---------------------------------------- schemeAndroid      | String | X | Androidアプリリンク                                                                                                                                |
| - coupon               | Object | X | クーポン                                                                                                                                        | 
| -- title               | String | X | titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| -- description         | String | X | クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                          |
| -- linkMo              | String | X | モバイルWebリンク(下部の必須条件確認)                                                                                                                          |
| -- linkPc              | String | X | PC Webリンク                                                                                                                                   |
| -- schemeIos           | String | X | iOSアプリリンク                                                                                                                                  |
| -- schemeAndroid       | String | X | Androidアプリリンク                                                                                                                                |
| - resendParameter      | Object | X | 代替送信情報                                                                                                                                  |
| -- isResend            | boolean | X | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                           |
| -- resendType          | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                         |
| -- resendTitle         | String | X | LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                   |
| -- resendContent       | String | X | 代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                             |
| -- resendSendNo        | String | X | 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                   |
| -- resendUnsubscribeNo | String | X | 代代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                     |
| - isAd                 | Boolean | X | 広告かどうか(デフォルト値true)                                                                                                                                 |
| - adult                | Boolean | X | 成人向けのメッセージかどうか(デフォルト値false)                                                                                                                           |
| - recipientGroupingKey | String | 	X | 受信者グルーピングキー(最大100文字)                                                                                                                              |
| statsId                | String | 	X | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                              |

#### カルーセルフィード型送信リクエスト

[Request Body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
    "recipientList": [{
        "recipientNo": String,
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
                },
                "coupon": {
                  "title": String,
                  "description": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String
                }
              }
            },
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
                },
                "coupon": {
                  "title": String,
                  "description": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String
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
        "resendParameter": {
            "isResend" : boolean,
            "resendType" : String,
            "resendTitle" : String,
            "resendContent" : String,
            "resendSendNo" : String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前               | 	タイプ | 	必須 | 	説明                                                                                                                                                   |
|------------------------|-------|--|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | 発信キー(40文字)                                                                                                                                                   |
| requestDate            | String | X | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                                         |
| senderGroupingKey      | String | X | 発信グルーピングキー(最大100文字)                                                                                                                                           |
| createUser             | String | X | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                                 |
| recipientList          | List  | O | 受信者リスト(最大1,000人)                                                                                                                                            |
| - recipientNo          | String | O | 受信番号                                                                                                                                                 |
| - carousel             | Object | O | カルーセル                                                                                                                                                   
| -- list                | List  | O | カルーセルリスト(最小2個、最大10個)                                                                                                                                       | 
| ---------------------------------------- header             | String | O | カルーセルアイテムタイトル(最大20文字)、カルーセルフィード型でのみ使用可能                                                                                                                  | 
| ---------------------------------------- message            | String | O | カルーセルアイテムメッセージ(最大180文字)、カルーセルフィード型でのみ使用可能                                                                                                                | 
| ---------------------------------------- attachment         | Object | O | カルーセルアイテム画像、ボタン情報                                                                                                                                    | 
| ----------------------------------------- buttons           | List  | X | ボタンリスト(最大2個)                                                                                                                                               | 
| ----- name             | String | X | ボタン名(ボタンがある場合は必須、最大8文字)                                                                                                                                  |
| ----- type             | String | X | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
| ------------------------------------------ linkMo           | String | X | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                                   |
| ------------------------------------------ linkPc           | String | X | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                                    |
| ------------------------------------------ schemeIos        | String | X | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                   |
| ------------------------------------------ schemeAndroid    | String | X | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                 |
| ---- image             | Object | O |                                                                                                                                                             | 
| ------------------------------------------ imageUrl         | String | O | 画像URL (カルーセルフィード型画像のみ使用可能です。カルーセルコマース画像使用不可)                                                                                                          |
| ------------------------------------------ imageLink        | String | X | 画像リンク                                                                                                                                                |
| ----------------------------------------- coupon            | Object | X | クーポン                                                                                                                                                    | 
| ------------------------------------------ title            | String | X | titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| ------------------------------------------ description      | String | X | クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                                      |
| ------------------------------------------ linkMo           | String | X | モバイルWebリンク(下部の必須条件確認)                                                                                                                                      |
| ------------------------------------------ linkPc           | String | X | PC Webリンク                                                                                                                                               |
| ------------------------------------------ schemeIos        | String | X | iOSアプリリンク                                                                                                                                              |
| ------------------------------------------ schemeAndroid    | String | X | Androidアプリリンク                                                                                                                                            |
| -- tail                | Object | X | さらに表示ボタン情報                                                                                                                                              | 
| --- linkMo             | String | O | モバイルWebリンク                                                                                                                                               |
| ---------------------------------------- linkPc             | String | X | PC Webリンク                                                                                                                                               |
| ---------------------------------------- schemeIos          | String | X | iOSアプリリンク                                                                                                                                              |
| ---------------------------------------- schemeAndroid      | String | X | Androidアプリリンク                                                                                                                                            |
| - resendParameter      | Object | X | 代替送信情報                                                                                                                                              |
| -- isResend            | boolean |X | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                                       |
| -- resendType          | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                                     |
| -- resendTitle         | String | X | LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                               |
| -- resendContent       | String | X | 代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                                         |
| -- resendSendNo        | String | X | 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                               |
| -- resendUnsubscribeNo | String | X | 代代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                                 |
| - isAd                 | Boolean | O | 広告かどうか(カルーセルフィード型は広告タイプのみ送信可能)                                                                                                                                |
| - adult                | Boolean | X | 成人向けのメッセージかどうか(デフォルト値false)                                                                                                                                       |
| - recipientGroupingKey | String | X | 受信者グルーピングキー(最大100文字)                                                                                                                                          |
| statsId                | String | X | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                                          |

#### カルーセルコマース型送信リクエスト

[Request Body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "carousel": {
          "head": {
            "header": String,
            "content": String,
            "imageUrl": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          },
          "list": [
            {
              "additionalContent": String,
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
                },
                "coupon": {
                  "title": String,
                  "description": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String
                },
                "commerce": {
                  "title": String,
                  "regularPrice": String,
                  "discountPrice": String,
                  "discountRate": String,
                  "discountFixed": String
                }
              }
            },
            {
              "additionalContent": String,
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
                },
                "coupon": {
                  "title": String,
                  "description": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String
                },
                "commerce": {
                  "title": String,
                  "regularPrice": Integer,
                  "discountPrice": Integer,
                  "discountRate": Integer,
                  "discountFixed": Integer
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
        "resendParameter": {
            "isResend": boolean,
            "resendType": String,
            "resendTitle": String,
            "resendContent": String,
            "resendSendNo": String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前 | 	タイプ | 	必須 | 	説明                                                                                                                                                   |
|--------|-------|--|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | 発信キー(40文字)                                                                                                                                                   |
| requestDate | String | X | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                                         |
| senderGroupingKey | String | X | 発信グルーピングキー(最大100文字)                                                                                                                                           |
| createUser | String | X | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                                 |
| recipientList | List  | O | 受信者リスト(最大1,000人)                                                                                                                                            |
| - recipientNo | String | 	O | 受信番号                                                                                                                                                 |
| - carousel | Object | O | カルーセル                                                                                                                                                   
| -- head | String | X | カルーセルイントロ情報(カルーセルコマース型でのみ使用可能)                                                                                                                              | 
| ---------------------------------------- header | String | O | カルーセルイントロヘッダ(最大20文字)                                                                                                                                          |  
| ---------------------------------------- content | String | O | カルーセルイントロ内容(最大50文字)                                                                                                                                          |
| ---------------------------------------- imageUrl | String | O | カルーセルイントロ画像アドレス(使用される画像はカルーセルの画像と比率が同じである必要があります。)                                                                                                           |
| ---------------------------------------- linkMo | String | X | モバイル環境でイントロクリック時に移動するWebリンク                                                                                                                             |
| ---------------------------------------- linkPc | String | X | PC環境でイントロクリック時に移動するWebリンク                                                                                                                              |
| ---------------------------------------- schemeIos | String | X | 	iOS環境でイントロクリック時に移動するアプリリンク                                                                                                                            |
| ---------------------------------------- schemeAndroid | String | X | 	Android環境でイントロクリック時に移動するアプリリンク                                                                                                                          |
| -- list | List  | O | カルーセルリスト(最小2個、最大10個)                                                                                                                                       | 
| ---------------------------------------- additionalContent | String | X | 付加情報(最大34文字)、カルーセルコマース型でのみ使用可能                                                                                                                      |  
| ---------------------------------------- attachment | Object | O | カルーセルアイテム画像、ボタン情報                                                                                                                                    | 
| ----------------------------------------- buttons | List  | X | ボタンリスト(最大2個)                                                                                                                                               | 
| ----- name | String | X | ボタン名(ボタンがある場合は必須、最大8文字)                                                                                                                                  |
| ----- type | String | X | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
| ------------------------------------------ linkMo | String |X | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                                   |
| ------------------------------------------ linkPc | String | X | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                                    |
| ------------------------------------------ schemeIos | String | X | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                   |
| ------------------------------------------ schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                 |
| ---- image | Object | O |                                                                                                                                                             | 
| ------------------------------------------ imageUrl | String | O | 	画像URL（カルーセルコマースの画像のみ使用可能です。使用される画像は、他のカルーセルコマースの画像やイントロの画像と同じ比率である必要があります。)                                                                        |
| ------------------------------------------ imageLink | String | X | 	画像リンク                                                                                                                                               |
| ----------------------------------------- coupon | Object | X | クーポン                                                                                                                                                    | 
| ------------------------------------------ title | String | X | titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| ------------------------------------------ description | String |X | クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                                      |
| ------------------------------------------ linkMo | String | X | モバイルWebリンク(下部の必須条件確認)                                                                                                                                      |
| ------------------------------------------ linkPc | String | X | PC Webリンク                                                                                                                                               |
| ------------------------------------------ schemeIos | String | X | iOSアプリリンク                                                                                                                                              |
| ------------------------------------------ schemeAndroid | String | X | Androidアプリリンク                                                                                                                                            |
| ----------------------------------------- commerce | Object | O | コマース(カルーセルコマース型でのみ使用可能)                                                                                                                                      | 
| ------------------------------------------ title | String | O | 商品タイトル(最大30文字)                                                                                                                                               |
| ------------------------------------------ regularPrice | Integer | O | 正常価格(0 ～ 99,999,999)                                                                                                                                        |
| ------------------------------------------ discountPrice | Integer | X | 割引価格(0 ～ 99,999,999)                                                                                                                                        |
| ------------------------------------------ discountRate | Integer | X | 割引率(0 ～ 100)、割引価格が存在する時は割引率、定額割引価格のいずれかが必須                                                                                                          |
| ------------------------------------------ discountFixed | Integer | X | 	定額割引価格(0 ～ 999,999)、割引価格が存在する時は割引率、定額割引価格のいずれかが必須                                                                                                  |
| -- tail | Object | X | さらに表示ボタン情報                                                                                                                                             | 
| --- linkMo | String | O | モバイルWebリンク                                                                                                                                               |
| ---------------------------------------- linkPc | String | X | PC Webリンク                                                                                                                                               |
| ---------------------------------------- schemeIos | String | X | iOSアプリリンク                                                                                                                                              |
| ---------------------------------------- schemeAndroid | String | X | Androidアプリリンク                                                                                                                                            |
| - resendParameter | Object | X | 代替送信情報                                                                                                                                              |
| -- isResend | boolean | X | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                                       |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                                     |
| -- resendTitle | String | X | LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                               |
| -- resendContent | String | X | 代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                                         |
| -- resendSendNo | String | X | 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                               |
| -- resendUnsubscribeNo | String | X | 代代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                                 |
| - isAd | Boolean | O | 広告かどうか(カルーセルコマース型は広告タイプのみ送信可能)                                                                                                                               |
| - adult | Boolean | X | 成人向けのメッセージかどうか(デフォルト値false)                                                                                                                                       |
| - recipientGroupingKey | 	String | X | 	受信者グルーピングキー(最大100文字)                                                                                                                                         |
| statsId | String | X | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                                          |

* カルーセルコマースに使用される全ての画像は同じ比率である必要があります。

#### プレミアム動画型送信リクエスト

[Request body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "content": String,
        "header": String,
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
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "video": {
          "videoUrl": String,
          "videoLink": String
        },
        "resendParameter": {
            "isResend": boolean,
            "resendType": String,
            "resendTitle": String,
            "resendContent": String,
            "resendSendNo": String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前               | 	タイプ | 	必須 | 	説明                                                                                                                                                    |
|------------------------|------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | 発信キー(40文字)                                                                                                                                                    |
| requestDate            | String | X | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                                           |
| senderGroupingKey      | String | X | 発信グルーピングキー(最大100文字)                                                                                                                                            |
| createUser             | String | X | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                                  |
| recipientList          | List | O | 受信者リスト(最大1,000人)                                                                                                                                            |
| - recipientNo          | String | O | 受信番号                                                                                                                                                  |
| - content              | String | O | 内容(最大1,000文字)<br>画像送信時、最大400文字<br>ワイド画像送信時、最大76文字                                                                                             |
| - header               | String | O | ヘッダ(プレミアムビデオタイプ使用時、選択、最大25文字)                                                                                                                             |
| - buttons              | List | X | ボタン(最大5個、クーポンが含まれる場合は最大4個)<br>ワイド画像送信時はリンクボタン最大2個                                                                                                              |
| -- ordering            | Integer | X | ボタン順序(ボタンがある場合は必須)                                                                                                                                          |
| -- type                | String | X | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |                                                                                    
| -- name                | String | X | ボタン名(ボタンがある場合は必須）                                                                                                                                          |
| -- linkMo              | String | X | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                                    |
| -- linkPc              | String | X | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                                     |
| -- schemeIos           | String | X | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                    |
| -- schemeAndroid       | String | X | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                  |
| -- chatExtra           | String | X | BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報                                                                                                                 |
| -- chatEvent           | String | X | BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名                                                                                                                            |
| -- bizFormKey          | String | X | | BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                                                                                                                                |
| -- target              | String | X | Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信                                                                                               |
| - coupon               | Object | X | クーポン                                                                                                                                                     | 
| -- title               | String | X | titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| -- description         | String | X | クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                                      |
| -- linkMo              | String | X | モバイルWebリンク(下部の必須条件確認)                                                                                                                                        |
| -- linkPc              | String | X | PC Webリンク                                                                                                                                                |
| -- schemeIos           | String | X | iOSアプリリンク                                                                                                                                               |
| -- schemeAndroid       | String | X | Androidアプリリンク                                                                                                                                             |
| - video                | Object | O | ビデオ                                                                                                                                                    | 
| -- videoUrl            | String | O | カカオTV動画URL (カカオTVにアップロードされた動画アドレスのみ使用可能)                                                                                                                    |
| -- thumbnailUrl        | String | X | 動画のサムネイル用画像URL、一般画像でアップロードされたURLのみ使用可能、ない場合はカカオTV動画の基本サムネイルを使用                                                                                   |
| - resendParameter      | Object | X | 代替送信情報                                                                                                                                               |
| -- isResend            | boolean | X | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                                          |
| -- resendType          | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                                      |
| -- resendTitle         | String | X | LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                                 |
| -- resendContent       | String | X | 代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                                           |
| -- resendSendNo        | String | X | 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                                 |
| -- resendUnsubscribeNo | String | X | 代代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                                   |
| - isAd                 | Boolean | X | 広告かどうか(デフォルト値true)                                                                                                                                              |
| - adult                | Boolean | X | 成人向けのメッセージかどうか(デフォルト値false)                                                                                                                                        |
| - recipientGroupingKey | 	String | X | 受信者グルーピングキー(最大100文字)                                                                                                                                           |
| statsId                | String | X | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                                           |

#### コマース型送信リクエスト

[Request body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "additionalContent": String,
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
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer
          "discountRate": Integer
          "discountFixed": Integer
        },
        "resendParameter": {
            "isResend": boolean,
            "resendType": String,
            "resendTitle": String,
            "resendContent": String,
            "resendSendNo": String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "adult": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| 名前             | 	タイプ | 	必須 | 	説明                                                                                                                                                   |
|----------------------|------|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey            | String | O   | 発信キー(40文字)                                                                                                                                                   |
| requestDate          | String | X   | リクエスト日時(yyyy-MM-dd HH:mm)、フィールドを送信しない場合、即時送信                                                                                                         |
| senderGroupingKey    | String | X   | 発信グルーピングキー(最大100文字)                                                                                                                                           |
| createUser           | String | X   | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                                                                                                                                 |
| recipientList        | List | 	O  | 受信者リスト(最大1,000人)                                                                                                                                            |
| - recipientNo        | String | O   | 受信番号                                                                                                                                                 |
| - additionalContent | String | X   | 付加情報(最大34文字)、コマース型でのみ使用可能                                                                                                                      |
| - buttons            | List | X   | 	ボタン(最大5個、クーポンが含まれる場合は最大4個)<br>ワイド画像送信時はリンクボタン最大2個                                                                                                              |
| -- ordering          | Integer | X   | ボタン順序(ボタンがある場合は必須)                                                                                                                                         |
| -- type              | String | X   | ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
| -- name              | String | X   | ボタン名(ボタンがある場合は必須）                                                                                                                                         |
| -- linkMo            | String | X   | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                                                                                   |
| -- linkPc            | String | X   | PC Webリンク(WLタイプの場合は任意フィールド)                                                                                                                                    |
| -- schemeIos         | String | X   | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                   |
| -- schemeAndroid     | String | X   | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                                                                                 |
| -- chatExtra         | String | X   | BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報                                                                                                               |
| -- chatEvent         | String | X   | BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名                                                                                                                          |
| -- bizFormKey        | String | X   | | BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                                                                                                                              |
| -- target            | String | X   | Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信                                                                                             |
| - coupon             | Object | X   | クーポン                                                                                                                                                    | 
| -- title             | String | X   | titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン" |
| -- description       | String | X   | クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)                                                                                      |
| -- linkMo            | String | X   | モバイルWebリンク(下部の必須条件確認)                                                                                                                                      |
| -- linkPc            | String | X   | PC Webリンク                                                                                                                                               |
| -- schemeIos         | String | X   | iOSアプリリンク                                                                                                                                              |
| -- schemeAndroid     | String | X   | Androidアプリリンク                                                                                                                                            |
| - commerce           | Object | O   | コマース(コマース型でのみ使用可能)                                                                                                                                          | 
| -- title             | String | O   | 商品タイトル(最大30文字)                                                                                                                                                |
| -- regularPrice      | Integer | O   | 正常価格(0 ～ 99,999,999)                                                                                                                                        |
| -- discountPrice     | Integer | X   | 割引価格(0 ～ 99,999,999)                                                                                                                                        |
| -- discountRate      | Integer | X   | 割引率(0 ～ 100)、割引価格が存在する時は割引率、定額割引価格のいずれかが必須                                                                                                           |
| -- discountFixed     | Integer | X   | 定額割引価格(0 ～ 999,999)、割引価格が存在する時は割引率、定額割引価格のいずれかが必須                                                                                                    |
| - resendParameter    | Object | X   | 代替送信情報                                                                                                                                              |
| -- isResend          | boolean | X   | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。                                                                                                       |
| -- resendType        | String | X   | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。                                                                                                     |
| -- resendTitle       | String | X   | LMS代替送信タイトル<br>(値がない場合は、プラスフレンドIDで再送信されます。)                                                                                                               |
| -- resendContent     | String | X   | 代替送信内容<br>(値がない場合は、テンプレートの内容で再送信されます。)                                                                                         |
| -- resendSendNo      | String | X   | 代替送信発信番号<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span>                                                               |
| -- resendUnsubscribeNo | String | X   | 代代替080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080の受信拒否番号がない場合、代替の転送が失敗することがあります。)</span>                                                 |
| - isAd               | Boolean | X   | 	広告かどうか(デフォルト値true)                                                                                                                                            |
| - adult              | Boolean | X   | 成人向けのメッセージかどうか(デフォルト値false)                                                                                                                                       |
| - recipientGroupingKey | String | 	X  | 	受信者グルーピングキー(最大100文字)                                                                                                                                         |
| statsId              | String | 	X  | 統計ID(発信検索条件には含まれません、最大8文字)                                                                                                                          |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages -d '{"plusFriendId":"@プラスフレンド","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"内容","buttons":[{"ordering":1,"type":"WL","name":"ボタン1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
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
GET  /friendtalk/v2.4/appkeys/{appkey}/messages
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
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
GET  /friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値      | タイプ | 必須 | 説明   |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | リクエストID      |
| recipientSeq | Integer | O    | 受信者シーケンス番号 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
        "head": {
            "header": String,
            "content": String,
            "imageUrl": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
        },
        "list": [
          {
            "header": String,
            "message": String,
            "addtionalContent": String,
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
              },
              "coupon": {
                "title": String,
                "description": String,
                "linkMo": String,
                "linkPc": String,
                "schemeAndroid": String,
                "schemeIos": String
              },
              "commerce": {
                "title": String,
                "regularPrice": Integer,
                "discountPrice": Integer,
                "discountRate": Integer,
                "discountFixed": Integer
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
      "video": {
        "videoUrl": String,
        "thumbnailUrl": String
      },
      "commerce": {
        "title": String,
        "regularPrice": Integer,
        "discountPrice": Integer,
        "discountRate": Integer,
        "discountFixed": Integer
      },
      "isAd": Boolean,
      "adult": Boolean,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 名前 |	タイプ| 	説明                                                 |
|---|---|-----------------------------------------------------------|
|header|	Object| 	ヘッダ領域                                              |
|- resultCode|	Integer| 	結果コード                                              |
|- resultMessage|	String| 結果メッセージ                                              |
|- isSuccessful|	Boolean| 成否                                               |
|message|	Object| 	メッセージ                                                |
|- requestId | String | 	リクエストID                                                    |
|- recipientSeq | Integer | 	受信者シーケンス番号                                         |
|- plusFriendId | String | 	プラスフレンドID                                                 |
|- senderKey | String | 発信キー                                                |
|- recipientNo | String | 	受信番号                                              |
|- requestDate | String | 	リクエスト日時                                              |
|- createDate | String | 登録日時                                               |
|- receiveDate | String | 	受信日時                                              |
|- content | String | 	本文                                                 |
|- messageStatus | String | 	リクエスト状態(COMPLETED:成功、 FAILED:失敗)                        |
|- resendStatus | String | 	再送信ステータスコード                                          |
|- resendStatusName | String | 	再送信ステータスコード名                                         |
|- resendResultCode | String | 再送信結果コードSMS結果コード                                 |
|- resendRequestId | String | 再送信SMSリクエストID                                             |
|- resultCode | String | 	受信結果コード                                           |
|- resultCodeName | String | 	受信結果コード名                                          |
|- createUser | String | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)                               |
|- imageSeq|	Integer| 画像番号                                              |
|- imageName|	String| 画像名(アップロードしたファイル名)                                            |
|- imageUrl|	String| 画像URL                                                   |
|- imageLink|	String| 画像リンク                                              |
|- wide  | boolean | ワイド画像の可否                                          |
|- buttons | List | 	ボタンリスト                                             |
|-- ordering | Integer | 	ボタン順序                                              |
|-- type | String | 	ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達)         |
|-- name | String | 	ボタン名(最大28文字、ワイドアイテムリストの場合、 9文字)                        |
|-- linkMo | String | 	モバイルWebリンク(WLタイプの場合は必須フィールド)                                |
|-- linkPc | String | 	PC Webリンク(WLタイプの場合は任意フィールド)                                 |
|-- schemeIos | String | 	iOSアプリリンク(ALタイプの場合は必須フィールド)                                |
|-- schemeAndroid | String | 	Androidアプリリンク(ALタイプの場合は必須フィールド)                              |
|-- chatExtra|	String| 	BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報            |
|-- chatEvent|	String| BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名                        |
|-- bizFormKey|	String| 	BF(ビジネスフォーム)タイプボタンの場合、Bizフォームキー                           |
|-- target|	String| 	Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信 |
|- header | String | ヘッダ(ワイドアイテムリストメッセージタイプ使用時、必須、最大25文字)                    |
|- additionalContent | String | 付加情報(最大34文字)、コマース型でのみ使用可能                                                                                                                      |
|- item | Object | ワイドアイテム                                              |
|-- list | List | ワイドアイテムリスト(最小3個、最大4個)                                   |
|---------------------------------------- title | String | アイテムタイトル(最初のアイテムの場合、最大25文字、 2～4番目のアイテムの場合、最大30文字)          |
|--- imageUrl | String | アイテム画像URL                                               |
|--- linkMo | String | モバイルWebリンク                                             |
|---------------------------------------- linkPc | String | PC Webリンク                                             |
|---------------------------------------- schemeIos | String | iOSアプリリンク                                            |
|---------------------------------------- schemeAndroid | String | Androidアプリリンク                                          |
|- carousel | Object | カルーセル                                                 | 
|-- head | String | カルーセルイントロ情報                                          | 
|---------------------------------------- header | String | カルーセルイントロヘッダ(最大20文字)                                        |  
|---------------------------------------- content | String | カルーセルイントロ内容(最大50文字)                                        |
|---------------------------------------- imageUrl | String | カルーセルイントロ画像アドレス                                      |
|---------------------------------------- linkMo | String | モバイル環境でイントロクリック時に移動するWebリンク                           |
|---------------------------------------- linkPc | String | PC環境でイントロクリック時に移動するWebリンク                            |
|---------------------------------------- schemeIos | String | iOS環境でイントロクリック時に移動するアプリリンク                           |
|---------------------------------------- schemeAndroid | String | Android環境でイントロクリック時に移動するアプリリンク                         |
|-- list | List  | カルーセルリスト(最小2個、最大10個)                                     |
|---------------------------------------- header | String | カルーセルアイテムタイトル(最大20文字)                                        | 
|---------------------------------------- message | String | カルーセルアイテムメッセージ(最大180文字)                                      | 
|---------------------------------------- additionalContent | String | 付加情報(最大34文字)                                             | 
|---------------------------------------- attachment | Object | カルーセルアイテム画像、ボタン情報                                  | 
|----------------------------------------- buttons | List | ボタンリスト(最大2個)                                             | 
|----- name| String | 	ボタン名(ボタンがある場合は必須、最大8文字)                               |
|----- type| String | 	ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
|------------------------------------------ linkMo| String | 	モバイルWebリンク(WLタイプの場合は必須フィールド)                                |
|------------------------------------------ linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)                                  |
|------------------------------------------ schemeIos | String | iOSアプリリンク(ALタイプの場合は必須フィールド)                                 |
|------------------------------------------ schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド)                               |
|----------------------------------------- image | Object | 画像                                                 | 
|------------------------------------------ imageUrl|	String| 	画像URL                                                  |
|------------------------------------------ imageLink|	String| 	画像リンク                                             |
|----------------------------------------- coupon | Object | クーポン                                                  | 
|------------------------------------------ title| String | 	クーポンtitle                                                 |
|------------------------------------------ description| String | 	クーポン詳細説明                                           |
|----- linkMo| String | モバイルWebリンク                                             |
|------------------------------------------ linkPc | String | 	PC Webリンク                                            |
|------------------------------------------ schemeIos | String | iOSアプリリンク                                            |
|------------------------------------------ schemeAndroid | String | Androidアプリリンク                                          |
|----------------------------------------- commerce | Object | コマース                                                 | 
|------------------------------------------ title | String | 商品タイトル(最大30文字)                                             |
|------------------------------------------ regularPrice | Integer | 正常価格(0 ～ 99,999,999)                                      |
|------------------------------------------ discountPrice | Integer | 割引価格(0 ～ 99,999,999)                                      |
|------------------------------------------ discountRate | Integer | 割引率(0 ～ 100)、割引価格が存在する時の割引率                          |
|------------------------------------------ discountFixed | Integer | 定額割引価格(0 ～ 999,999)                                       |
|-- tail | Object | さらに表示ボタン情報                                           | 
|--- linkMo| String | 	モバイルWebリンク                                            |
|---------------------------------------- linkPc | String | 	PC Webリンク                                            |
|---------------------------------------- schemeIos | String | iOSアプリリンク                                            |
|---------------------------------------- schemeAndroid | String | Androidアプリリンク                                         |
|- coupon | Object | クーポン                                                  | 
|-- title| String | 	クーポンtitle                                                 |
|-- description| String | 	クーポン詳細説明                                           |
|-- linkMo| String | モバイルWebリンク                                             |
|-- linkPc | String | 	PC Webリンク                                            |
|-- schemeIos | String | iOSアプリリンク                                            |
|-- schemeAndroid | String | Androidアプリリンク                                          |
|- video | Object | ビデオ                                                 | 
|-- videoUrl | String | カカオTV動画URL                  |
|-- thumbnailUrl | String | 動画サムネイル用画像URL               |
|- commerce | Object | コマース                                                 | 
|-- title | String | 商品タイトル(最大30文字)                                             |
|-- regularPrice | Integer | 正常価格(0 ～ 99,999,999)                                      |
|-- discountPrice | Integer | 割引価格(0 ～ 99,999,999)                                      |
|-- discountRate | Integer | 割引率(0 ～ 100)、割引価格が存在する時の割引率                          |
|-- discountFixed | Integer | 定額割引価格(0 ～ 999,999)                                       |
|- isAd | Boolean | 広告かどうか                                               |
|- adult | Boolean | 成人向けのメッセージかどうか                                           |
|- senderGroupingKey | String | 発信グルーピングキー                                            |
|- recipientGroupingKey | String | 	受信者グルーピングキー                                          |

## メッセージ
### メッセージ送信取消

#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}
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
| X-Secret-Key | String | O    | コンソールで作成できます。 |

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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### メッセージ結果アップデート照会

#### リクエスト

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/message-results
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 大量送信
### 大量送信リクエストリスト照会

#### リクエスト
[URL]
```
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages
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
https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
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
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages/{requestId}/recipients
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
https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
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
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
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
https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/${RECIPIENT_SEQ}?requestId='"${REQUEST_ID}" \
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
          "head": {
            "header": String,
            "content": String,
            "imageUrl": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
          },
          "list": [
            {
              "header": String,
              "message": String,
              "additionalContent": String,
              "attachment": {
                "buttons": [
                  {
                    "name": String,
                    "type": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeAndroid": String,
                    "schemeIos": String,
                },
                "commerce": {
                  "title": String,
                  "regularPrice": Integer,
                  "discountPrice": Integer,
                  "discountRate": Integer,
                  "discountFixed": Integer
                  }
                ],
                "image":{
                  "imageUrl": String,
                  "imageLink": String
                },
                "coupon": {
                  "title": String,
                  "description": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeAndroid": String,
                  "schemeIos": String
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
        "video": {
          "videoUrl": String,
          "thumbnailUrl": String
        },
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
        "isAd": Boolean,
        "adult": Boolean,
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
| -- bizFormKey|	String|	BF(ビジネスフォーム)タイプボタンの場合、ビズフォームキー |
| -- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| - header | String | ヘッダ(ワイドアイテムリストメッセージタイプ使用時、必須、最大25文字) |
| - item | Object | ワイドアイテム |
| -- list | List | ワイドアイテムリスト(最小3個、最大4個) |
| --- title | String | アイテムタイトル(最大25文字) |
| --- imageUrl | String | アイテム画像URL |
| --- linkMo | String | モバイルWebリンク |
| --- linkPc | String | PC Webリンク |
| --- schemeIos | String | iOSアプリリンク |
| --- schemeAndroid | String | Androidアプリリンク |
| - carousel | Object | カルーセル | 
|-- head | String | カルーセルイントロ情報                                          | 
|---------------------------------------- header | String | カルーセルイントロヘッダ(最大20文字)                                        |  
|---------------------------------------- content | String | カルーセルイントロ内容(最大50文字)                                        |
|---------------------------------------- imageUrl | String | カルーセルイントロ画像アドレス                                      |
|---------------------------------------- linkMo | String | モバイル環境でイントロクリック時に移動するWebリンク                           |
|---------------------------------------- linkPc | String | PC環境でイントロクリック時に移動するWebリンク                            |
|---------------------------------------- schemeIos | String | iOS環境でイントロクリック時に移動するアプリリンク                           |
|---------------------------------------- schemeAndroid | String | Android環境でイントロクリック時に移動するアプリリンク                         |
|-- list | List  | カルーセルリスト(最小2個、最大10個)                                     |
|---------------------------------------- header | String | カルーセルアイテムタイトル(最大20文字)                                        | 
|---------------------------------------- message | String | カルーセルアイテムメッセージ(最大180文字)                                      | 
|---------------------------------------- additionalContent | String | 付加情報(最大34文字)                                             | 
|---------------------------------------- attachment | Object | カルーセルアイテム画像、ボタン情報                                  | 
|----------------------------------------- buttons | List | ボタンリスト(最大2個)                                             | 
|----- name| String | 	ボタン名(ボタンがある場合は必須、最大8文字)                               |
|----- type| String | 	ボタンタイプ(WL：Webリンク、AL：アプリリンク、BK：Botキーワード、MD：メッセージ伝達、 BF:ビジネスフォーム)                                                                                          |
|------------------------------------------ linkMo| String | 	モバイルWebリンク(WLタイプの場合は必須フィールド)                                |
|------------------------------------------ linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)                                  |
|------------------------------------------ schemeIos | String | iOSアプリリンク(ALタイプの場合は必須フィールド)                                 |
|------------------------------------------ schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド)                               |
|----------------------------------------- image | Object | 画像                                                 | 
|------------------------------------------ imageUrl|	String| 	画像URL                                                  |
|------------------------------------------ imageLink|	String| 	画像リンク                                             |
|----------------------------------------- coupon | Object | クーポン                                                  | 
|------------------------------------------ title| String | 	クーポンtitle                                                 |
|------------------------------------------ description| String | 	クーポン詳細説明                                           |
|----- linkMo| String | モバイルWebリンク                                             |
|------------------------------------------ linkPc | String | 	PC Webリンク                                            |
|------------------------------------------ schemeIos | String | iOSアプリリンク                                            |
|------------------------------------------ schemeAndroid | String | Androidアプリリンク                                          |
|----------------------------------------- commerce | Object | コマース                                                 | 
|------------------------------------------ title | String | 商品タイトル(最大30文字)                                             |
|------------------------------------------ regularPrice | Integer | 正常価格(0 ～ 99,999,999)                                      |
|------------------------------------------ discountPrice | Integer | 割引価格(0 ～ 99,999,999)                                      |
|------------------------------------------ discountRate | Integer | 割引率(0 ～ 100)、割引価格が存在する時の割引率                          |
|------------------------------------------ discountFixed | Integer | 定額割引価格(0 ～ 999,999)                                       |
|-- tail | Object | さらに表示ボタン情報                                           | 
|--- linkMo| String | 	モバイルWebリンク                                            |
|---------------------------------------- linkPc | String | 	PC Webリンク                                            |
|---------------------------------------- schemeIos | String | iOSアプリリンク                                            |
|---------------------------------------- schemeAndroid | String | Androidアプリリンク                                         |
|- coupon | Object | クーポン                                                  | 
|-- title| String | 	クーポンtitle                                                 |
|-- description| String | 	クーポン詳細説明                                           |
|-- linkMo| String | モバイルWebリンク                                             |
|-- linkPc | String | 	PC Webリンク                                            |
|-- schemeIos | String | iOSアプリリンク                                            |
|-- schemeAndroid | String | Androidアプリリンク                                          |
|- video | Object | ビデオ                                                 | 
|-- videoUrl | String | カカオTV動画URL                  |
|-- thumbnailUrl | String | 動画サムネイル用画像URL               |
|- commerce | Object | コマース                                                 | 
|-- title | String | 商品タイトル(最大30文字)                                             |
|-- regularPrice | Integer | 正常価格(0 ～ 99,999,999)                                      |
|-- discountPrice | Integer | 割引価格(0 ～ 99,999,999)                                      |
|-- discountRate | Integer | 割引率(0 ～ 100)、割引価格が存在する時の割引率                          |
|-- discountFixed | Integer | 定額割引価格(0 ～ 999,999)                                       |
|- isAd | Boolean | 広告かどうか                                               |
|- adult | Boolean | 成人向けのメッセージかどうか                                           |
|- createDate | String | 作成日 |

## イメージの管理

### イメージの登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Request parameter]

| 値 | タイプ | 必須 | 説明 |
| ----- | ---- | ---- | ---- |
| image | File | O    | イメージ |
| wide  | boolean | X | ワイドイメージの可否(基本: false) |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
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

### ワイドアイテムリスト画像登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/wide-itemlist/images
Content-Type: multipart/form-data
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリキー|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できます。  |

[Request parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|image|	File|	O |	画像 |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/wide-itemlist/images" -F "image=@friend-ricecake02.jpeg"
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|image|	Object|	本文領域|
|- imageSeq | Integer |	画像番号(カカともへのメッセージメッセージ送信時に使用)|
|- imageUrl | String |	画像URL |
|- imageName | String |	画像名(アップロードしたファイル名) |

### カルーセル画像登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/carousel/images
Content-Type: multipart/form-data
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリキー|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できます。  |

[Request parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|image|	File|	O |	画像 |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/carousel/images" -F "image=@friend-ricecake02.jpeg"
```

#### レスポンス
```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "image": {
      "imageSeq": Integer,
      "imageUrl": String,
      "imageName": String
    }
}
```

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|image|	Object|	本文領域|
|- imageSeq | Integer |	画像番号(カカともへのメッセージ送信時に使用)|
|- imageUrl | String |	画像URL |
|- imageName | String |	画像名(アップロードしたファイル名) |

### カルーセルコマース画像登録
#### リクエスト

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/carousel/commerce-images
Content-Type: multipart/form-data
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリキー|

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できます。  |

[Request parameter]

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|image|	File|	O |	画像 |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/carousel/images" -F "image=@friend-ricecake02.jpeg"
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|image|	Object|	本文領域|
|- imageSeq | Integer |	画像番号(カカともへのメッセージメッセージ送信時に使用)|
|- imageUrl | String |	画像URL |
|- imageName | String |	画像名(アップロードしたファイル名) |


### イメージの照会
#### リクエスト

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値  | タイプ | 必須 | 説明      |
| -------- | ------- | ---- | ------------- |
|imageTypes|	String|	X| - IMAGE:一般画像<br/> - WIDE_IMAGE:ワイド画像<br/> - WIDE_ITEMLIST_IMAGE:ワイドアイテムリスト画像<br/> - CAROUSEL_IMAGE:カルーセル画像<br/> IMAGE, WIDE_IMAGE(default) |
| pageNum  | Integer | X    | ページ番号(基本：1) |
| pageSize | Integer | X    | 照会件数(基本：15) |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images?pageNum=1&pageSize=15"
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
| -- imageType     | String |	- IMAGE:一般画像<br/> - WIDE_IMAGE:ワイド画像<br/> - WIDE_ITEMLIST_IMAGE:ワイドアイテムリスト画像<br/> - CAROUSEL_IMAGE:カルーセル画像<br/> |
| -- createDate   | String  | 作成日時            |
| - totalCount    | Integer | 総個数                  |

* イメージは、最近登録した順にソートされてレスポンスを返します。


### イメージの削除
#### リクエスト

[URL]

```
DELETE  /friendtalk/v2.4/appkeys/{appkey}/images
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値  | タイプ | 必須 | 説明 |
| -------- | ------ | ---- | ------ |
| imageSeq | String | O    | イメージ番号 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images?imageSeq=1,2,3"
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

## アップロード
### ビジネスフォーム登録
[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリキー|
|senderKey|	String|	発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できます。  |


[Request body]

```
{
    "bizFormId": Integer
}
```

| 名前 |	タイプ|	必須|	説明|
|---|---|---|---|
|bizFormId|	Integer|	O | ビジネスフォームID |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/senders/{senderKey}/biz-form -d '{"bizFormId": 1}
```

#### レスポンス
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

| 名前 |	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|
|bizFormKey | String | ビジネスフォームキー |

## 代替送信管理
### SMSアプリキー登録

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/failback/appkey
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
| X-Secret-Key | String | O    | コンソールで作成できます。 |


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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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
POST  /friendtalk/v2.4/appkeys/{appkey}/failback
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
| X-Secret-Key | String | O    | コンソールで作成できます。 |


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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@プラスフレンド","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
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
