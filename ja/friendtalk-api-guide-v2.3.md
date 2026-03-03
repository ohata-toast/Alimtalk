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
<td>https://kakaotalk-bizmessage.api.nhncloudservice.com</td>
</tr>
</tbody>
</table>

## v2.3 API紹介
1. カカともへのメッセージワイドアイテムリスト、カルーセルフィード型、クーポン、ビジネスフォームボタン機能が追加されました。
2. ワイドアイテムリスト画像登録、カルーセル画像登録APIが追加されました。
3. 画像照会時、 imageTypeフィールドが追加されました。
4. 送信時、 imageSeq -> imageUrlフィールドを使用するように変更されました。

## メッセージの送信

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
| X-Secret-Key | String | O    | コンソールで作成できます。  |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同じkeyで要求すると、その要求を失敗処理します。 |

* <b>リクエスト日時は呼び出した時点から60日後まで設定可能です。</b>
* <b>夜間送信制限(20:50~翌日08:00)</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信API仕様に従ってフィールドを入力する必要があります。(SMSサービスに登録された発信番号、 080受信拒否番号、各種フィールドの長さ制限など)</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は切り捨てられ、代替送信される場合があります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)</b>
* <b>カカともへのメッセージ広告メッセージは広告SMS APIで代替送信されるため、代替送信を行うには必ず080受信拒否番号を登録する必要があります。</b>
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
| recipientList          | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo          | String  | O    | 受信番号                              |
| - content              | String  | O    | 内容(最大1000文字)<br>イメージを含む時は最大400文字  |
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
| -- bizFormKey          |	String | X    | Biz from key for BF(Business from) type button |
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
| - imageUrl             | String  | X    |	画像URL |
| - imageLink            | String  | X    | イメージリンク                                |
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
| -- bizFormKey          |	String | X    | Biz from key for BF(Business from) type button |
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
| -- bizFormKey          |	String | X    | Biz from key for BF(Business from) type button |
| -- target              | String  | X    |	Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信 |
| - header               | String  | X    | Header(required when using the wide item list message type, up to 25 characters) |
| - item                 | Object  | X    | Wide item |
| -- list                | List    | X    | Wide item list(at lease 3, up to 4) |
| --- title              | String  | X    | Item title(For the first item, up to 25 characters; for items 2 to 4, up to 30 characters) |
| --- imageUrl           | String  | X    | Item image URL |
| --- linkMo | String | X | Mobile web link |
| --- linkPc | String | X | PC web link |
| --- schemeIos | String | X | iOS app link |
| --- schemeAndroid | String | X | Android app link |
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
| - carousel | Object | X | Carousel | 
| -- list | List | X |  Carousel list(at least 2, up to 6) | 
| --- header | String | X | Carousel item title(up to 20 characters) | 
| --- message | String | X | Carousel item message(up to 180 characters) | 
| --- attachment | Object | X | Carousel item images, button information | 
| ---- buttons | List | X | Button list(up to 2) | 
| ----- name| String |	X |	Button name(required, if there's a button, up to 8 characters)|
| ----- type| String |	X |	Button type(WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form) |
| ----- linkMo| String |	X |	Mobile web link(required for the WL type)|
| ----- linkPc | String |	X |PC web link(optional for the WL type) |
| ----- schemeIos | String | X |	iOS app link(required for the AL type) |
| ----- schemeAndroid | String | X |	Android app link(required for the AL type) |
| ---- image | Object | X | Image | 
| ----- imageUrl|	String|	X|	Image URL   |
| ----- imageLink|	String|	X|	Image link   |
| ---- coupon | Object | X | クーポン | 
| ----- title| String |	X |	titleの場合、5つの形式に制限される<br>"${数字}KRW割引クーポン"数字は1以上99,999,999以下<br>"${数字}%割引クーポン"数字は1以上100以下<br>"送料割引クーポン"<br><br>"${7文字以内}無料クーポン"<br>"${7文字以内} UPクーポン"|
| ----- description| String |	X |	クーポン詳細説明(一般テキスト、画像型、カルーセルフィード型最大12文字 / ワイド画像型、ワイドアイテムリスト型最大18文字)|
| ----- linkMo| String |	X |	モバイルWebリンク(下部の必須条件確認) |
| ----- linkPc | String |	X |PC Webリンク |
| ----- schemeIos | String | X |	iOSアプリリンク |
| ----- schemeAndroid | String | X |	Androidアプリリンク |
| -- tail | Object | X | Learn more button information | 
| --- linkMo| String |	X |	Mobile web link|
| --- linkPc | String |	X |PC web link |
| --- schemeIos | String | X |	iOS app link |
| --- schemeAndroid | String | X |	Android app link |
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


[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/messages -d '{"plusFriendId":"@プラスフレンド","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"内容","buttons":[{"ordering":1,"type":"WL","name":"ボタン1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}'
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
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messageSearchResultResponse": {
    "messages": [
        {
          "requestId": String,
          "recipientSeq": Integer,
          "plusFriendId": String,
          "senderKey": String,
          "recipientNo": String,
          "requestDate": String,
          "createDate": String,
          "receiveDate": String,
          "content": String,
          "messageStatus": String,
          "resendStatus": String,
          "resendStatusName": String,
          "resultCode": String,
          "resultCodeName": String,
          "createUser": String,
          "senderGroupingKey": String,
          "recipientGroupingKey": String
        }
    ],
    "totalCount": Integer
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
|-- senderKey   | String | 発信キー |
| -- recipientNo              | String  | 受信番号                       |
| -- requestDate              | String  | リクエスト日時                       |
| -- createDate               | String  | 登録日時                             |
|-- receiveDate | String |	受信日時 |
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値      | タイプ | 必須 | 説明   |
| ------------ | ------- | ---- | ---------- |
| requestId    | String  | O    | リクエストID      |
| recipientSeq | Integer | O    | 受信者シーケンス番号 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
|- senderKey   | String | 発信キー                                               |
| - recipientNo          | String  | 受信番号                              |
| - requestDate          | String  | リクエスト日時                              |
| - createDate           | String  | 登録日時                             |
| - receiveDate          | String  | 受信日時                              |
| - content              | String  | 本文                                 |
| - messageStatus        | String  | リクエストステータス(COMPLETED：成功、FAILED：失敗)      |
| - resendStatus         | String  | 再送信ステータスコード                          |
| - resendStatusName     | String  | 再送信ステータスコード名                             |
|- resendResultCode | String | 再送信結果コードSMS結果コード                                |
|- resendRequestId | String | 再送信SMSリクエストID                                            |
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
| -- chatExtra           | String  | BC(相談トーク切り替え) / BT(Bot切り替え)タイプボタンの場合に伝達するメタ情報 |
| -- chatEvent           | String  | BT(Bot切り替え)タイプボタンの場合に接続するBotイベント名 |
| -- bizFormKey|	String|	BF(ビジネスフォーム)タイプボタンの場合、ビズフォームキー |
| -- target              | String  | Webリンクボタンの場合、"target":"out"属性を追加すると、アウトリンク<br>デフォルトアプリ内リンクで送信 |
|- header | String | ヘッダ(ワイドアイテムリストメッセージタイプ使用時、必須、最大25文字) |
|- item | Object | ワイドアイテム |
|-- list | List | ワイドアイテムリスト(最小3個/最大4個) |
|--- title | String | アイテムタイトル(最大25文字) |
|--- imageUrl | String | アイテム画像URL |
|--- linkMo | String | モバイルWebリンク |
|--- linkPc | String | PC Webリンク |
|--- schemeIos | String | iOSアプリリンク |
|--- schemeAndroid | String | Androidアプリリンク |
|- carousel | Object | カルーセル | 
|-- list | List | カルーセルリスト(最小2個/最大6個) | 
|--- header | String | カルーセルアイテムタイトル(最大20文字) | 
|--- message | String | カルーセルアイテムメッセージ(最大180文字) | 
|--- attachment | Object | カルーセルアイテム画像、ボタン情報 | 
|---- buttons | List | ボタンリスト(最大2個) | 
|----- name| String |	ボタン名(ボタンがある場合は必須、最大8文字)|
|----- type| String |	ボタンタイプ(WL:Webリンク、 AL:アプリリンク、 BK:Botキーワード、 MD:メッセージ伝達、 BF:ビジネスフォーム) |
|----- linkMo| String |	モバイルWebリンク(WLタイプの場合は必須フィールド)|
|----- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド) |
|----- schemeIos | String | iOSアプリリンク(ALタイプの場合は必須フィールド) |
|----- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
|---- image | Object | 画像 | 
|----- imageUrl|	String|	画像URL   |
|----- imageLink|	String|	画像リンク |
|---- coupon | Object | クーポン | 
|----- title| String |	クーポンtitle |
|----- description| String |	クーポン詳細説明 |
|----- linkMo| String | モバイルWebリンク |
|----- linkPc | String |	PC Webリンク |
|----- schemeIos | String | iOSアプリリンク |
|----- schemeAndroid | String | Androidアプリリンク |
|-- tail | Object | さらに表示ボタン情報 | 
|--- linkMo| String |	モバイルWebリンク|
|--- linkPc | String |	PC Webリンク |
|--- schemeIos | String | iOSアプリリンク |
|--- schemeAndroid | String | XAndroidアプリリンク |
|- coupon | Object | クーポン | 
|-- title| String |	クーポンtitle |
|-- description| String |	クーポン詳細説明 |
|-- linkMo| String | モバイルWebリンク |
|-- linkPc | String |	PC Webリンク |
|-- schemeIos | String | iOSアプリリンク |
|-- schemeAndroid | String | Androidアプリリンク |
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
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
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
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messageSearchResultResponse": {
    "messages": [
    {
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
      "resultCode": String,
      "resultCodeName": String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount": Integer
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
|-- senderKey | String |	発信キー |
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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

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
https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
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
https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
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
https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/${RECIPIENT_SEQ}?requestId='"${REQUEST_ID}" \
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
| -- bizFormKey|	String|	BF(ビジネスフォーム)タイプボタンの場合、ビズフォームキー |
| -- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| - header | String | ヘッダ(ワイドアイテムリストメッセージタイプ使用時、必須、最大25文字) |
| - item | Object | ワイドアイテム |
| -- list | List | ワイドアイテムリスト(最小3個/最大4個) |
| --- title | String | アイテムタイトル(最大25文字) |
| --- imageUrl | String | アイテム画像URL |
| --- linkMo | String | モバイルWebリンク |
| --- linkPc | String | PC Webリンク |
| --- schemeIos | String | iOSアプリリンク |
| --- schemeAndroid | String | Androidアプリリンク |
| - carousel | Object | カルーセル | 
| -- list | List | カルーセルリスト(最小2個/最大10個) | 
| --- header | String | カルーセルアイテムタイトル(最大20文字) | 
| --- message | String | カルーセルアイテムメッセージ(最大180文字) | 
| --- attachment | Object | カルーセルアイテム画像、ボタン情報 | 
| ---- buttons | List | ボタンリスト(最大2個) | 
| ----- name| String |	ボタン名(ボタンがある場合は必須、最大8文字)|
| ----- type| String |	ボタンタイプ(WL:Webリンク、 AL:アプリリンク、 BK:Botキーワード、 MD:メッセージ伝達、 BF:ビジネスフォーム) |
| ----- linkMo| String |	モバイルWebリンク(WLタイプの場合は必須フィールド)|
| ----- linkPc | String |	PC Webリンク(WLタイプの場合は任意フィールド) |
| ----- schemeIos | String | iOSアプリリンク(ALタイプの場合は必須フィールド) |
| ----- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| ---- image | Object | 画像 | 
| ----- imageUrl|	String|	画像URL   |
| ----- imageLink|	String|	画像リンク |
| ---- coupon | Object | クーポン | 
| ----- title| String |	クーポンtitle |
| ----- description| String |	クーポン詳細説明 |
| ----- linkMo| String | モバイルWebリンク |
| ----- linkPc | String |	PC Webリンク |
| ----- schemeIos | String | iOSアプリリンク |
| ----- schemeAndroid | String | Androidアプリリンク |
| -- tail | Object | さらに表示ボタン情報 | 
| --- linkMo| String |	モバイルWebリンク|
| --- linkPc | String |	PC Webリンク |
| --- schemeIos | String | iOSアプリリンク |
| --- schemeAndroid | String | Androidアプリリンク |
|- coupon | Object | クーポン | 
|-- title| String |	クーポンtitle |
|-- description| String |	クーポン詳細説明 |
|-- linkMo| String | モバイルWebリンク |
|-- linkPc | String |	PC Webリンク |
|-- schemeIos | String | iOSアプリリンク |
|-- schemeAndroid | String | Androidアプリリンク |
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
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Request parameter]

| 値 | タイプ | 必須 | 説明 |
| ----- | ---- | ---- | ---- |
| image | File | O    | イメージ |
| wide  | boolean | X | ワイドイメージの可否(基本: false) |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
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
POST  /friendtalk/v2.3/appkeys/{appkey}/wide-itemlist/images
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
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/wide-itemlist/images" -F "image=@friend-ricecake02.jpeg"
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
POST  /friendtalk/v2.3/appkeys/{appkey}/carousel/images
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
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/carousel/images" -F "image=@friend-ricecake02.jpeg"
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
|---|---|---|---|
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値  | タイプ | 必須 | 説明      |
| -------- | ------- | ---- | ------------- |
|imageTypes|	String|	X| - IMAGE:一般画像<br/> - WIDE_IMAGE:ワイド画像<br/> - WIDE_ITEMLIST_IMAGE:ワイドアイテムリスト画像<br/> - CAROUSEL_IMAGE:カルーセル画像<br/> IMAGE, WIDE_IMAGE(default) |
| pageNum  | Integer | X    | ページ番号(基本：1) |
| pageSize | Integer | X    | 照会件数(基本：15) |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/images?pageNum=1&pageSize=15"
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
|---|---|---|
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
|---|---|---|---|
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

| 値  | タイプ | 必須 | 説明 |
|---|---|---|---|
| imageSeq | String | O    | イメージ番号 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/images?imageSeq=1,2,3"
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
|---|---|---|
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |


## アップロード
### ビジネスフォーム登録
[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/biz-form
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/biz-form -d '{"bizFormId": 1}
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
### SMS AppKey 登録

[URL]

```
POST  /friendtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | タイプ | 説明 |
|---|---|---|
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
|---|---|---|---|
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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
|---|---|---|
| appkey       | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値     | タイプ | 必須 | 説明                                |
|---|---|---|---|
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
|---|---|---|---|
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| isResend             | boolean | O    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| resendSendNo         | String  | O    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
|resendUnsubscribeNo|	String|	X | 代替送信080受信拒否番号<br><span style="color:red">(SMS商品に登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。)</span> |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/friendtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@プラスフレンド","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
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
