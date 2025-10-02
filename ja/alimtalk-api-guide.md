## Notification > KakaoTalk Bizmessage > お知らせトーク > API v2.3 Guide

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

## v2.3 API紹介
1. お知らせトーク大量送信照会、統計照会APIが追加されました。
2. 置換メッセージ送信時、buttonsフィールドが追加されました。
3. 専門メッセージ送信時、buttonsフィールドにchatExtra、chatEvent、targetフィールドが追加されました。
4. メッセージ照会時、buttonsフィールドにchatExtra、chatEvent、targetフィールドが追加されました。

## 一般メッセージ

### メッセージ置換送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同じkeyで要求すると、その要求を失敗処理します。 |

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
        "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String,
            "telNumber": String
          }
        ],
        "quickReplies": [
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 値               | タイプ | 必須 | 説明                                |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| senderKey              | String  | O    | 発信キー                            |
| templateCode           | String  | O    | 登録した送信テンプレートコード(最大20桁)                    |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信)<br>最大60日後まで予約可能 |
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| createUser             | String  | X    | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| recipientList          | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo          | String  | O    | 受信番号(最大15桁)                            |
| - templateParameter    | Object  | X    | テンプレートパラメータ<br>(テンプレートに置換する変数が含まれる時は必須)       |
| -- key                 | String  | X    | 置換キー(#{key})                             |
| -- value               | String  | X    | 置換キーにマッピングされるvalue値                |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| - buttons              | List    | X    | ボタン追加情報 |
| -- ordering            | Integer | X    |	ボタン順序(ボタンがある場合は必須) |
| -- chatExtra           | String  | X    |	BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | X    |	BT(Bot切替)タイプボタンの時、接続するBotイベント名 |
| -- target              | String  | X    |	Webリンクボタンの場合、"target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| -- telNumber | String | X | TN(電話する)タイプのボタンの場合、伝達する電話番号 |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |
| messageOption          | Object  | X    |	メッセージオプション                                         |
| - price                | Integer | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - currencyType         | String  | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| statsId                | String  | X    |	統計ID(発信検索条件には含まれません, 最大8文字) |

* <b>リクエスト日時は呼び出す時点から60日後まで設定可能です。</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信APIの仕様に応じてフィールドを入力する必要があります。(SMSサービスに登録された発信番号、各種フィールドの長さ制限など)</b>
* <b>SMSサービスは、国際SMSのみサポートします。国際受信者番号の場合、 resendType(代替送信タイプ)をSMSに変更すると正常に代替送信できます。</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は、途中で切れて代替送信されることがあります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)</b>
* <b>templateTitleとtemplateItemHighlight.titleフィールドの一番後ろに日本語識別子とtemplateParameterを利用して`\s`文字を追加する場合、取り消し線スタイルを適用できます。</b>
    * <b>ただし、テンプレート登録時にあらかじめ\sをフィールドに追加しておいた場合は適用されません。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","templateParameter":{"{日本語識別子フィールド}":"{置換データ}"}}]}'
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

| 値                | タイプ | 説明    |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域 |
| - resultCode            | Integer | 結果コード |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否  |
| message                 | Object  | 本文領域 |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号 |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー |

### メッセージ全文送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同じkeyで要求すると、その要求を失敗処理します。 |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle": String,
            "templateHeader": String,
            "templateItem": {
              "list": [{
                "title": String,
                "description": String
              }],
              "summary": {
                "title": String,
                "description": String
              }
            },
            "templateItemHighlight": {
              "title": String,
              "description": String,
              "imageUrl": String
            },
            "templateRepresentLink": {
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            },
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
                    "bizFormId": Integer,
                    "pluginId": String,
                    "relayId": String,
                    "oneClickId": String,
                    "productId": String,
                    "target": String,
                    "telNumber": String
                }
            ],
            "quickReplies": [
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
                    "bizFormId": Integer,
                    "target": String
                }
            ],
            "resendParameter": {
              "isResend": boolean,
              "resendType": String,
              "resendTitle": String,
              "resendContent": String,
              "resendSendNo": String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 値               | タイプ | 必須 | 説明                                |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| senderKey           | String  | O    | 発信キー                                   |
| templateCode           | String  | O    | 登録した送信テンプレートコード(最大20桁)                    |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信) |
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| recipientList          | List    | O    | 受信者リスト(最大1,000人)                        |
| - recipientNo          | String  | O    | 受信番号(最大15桁)                            |
| - content              | String  | O    | 内容(最大1000文字)                             |
| - templateTitle        | String  | X    | テンプレートハイライトタイトル(最大50桁) |
| - buttons              | List    | X    | ボタンリスト(最大5個)                             |
| -- ordering            | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type                | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| -- name                | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -- linkMo              | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大500文字)       |
| -- linkPc              | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大500文字)        |
| -- schemeIos           | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大500文字)       |
| -- schemeAndroid       | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大500文字)   |
| -- chatExtra           | String  | X    |	BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | X    |	BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| -- target              | String  | X    |	Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| -- telNumber | String | X | TN(電話する)タイプのボタンの場合、伝達する電話番号 |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |
| messageOption          | Object  | X    |	メッセージオプション                                         |
| - price                | Integer | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - currencyType         | String  | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| statsId                | String  | X    |	統計ID(発信検索条件には含まれません, 最大8文字) |

* <b>本文とボタンに置換が完了したデータを入れてください。</b>
* <b>リクエスト日時は呼び出す時点から60日後まで設定可能です。</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信APIの仕様に応じてフィールドを入力する必要があります。(SMSサービスに登録された発信番号、各種フィールドの長さ制限など)</b>
* <b>SMSサービスは、国際SMSのみサポートします。国際受信者番号の場合、 resendType(代替送信タイプ)をSMSに変更すると正常に代替送信できます。</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は、途中で切れて代替送信されることがあります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)<
* <b>送信時にtemplateTitleとtemplateItemHighlight.titleフィールドの一番後ろに`\s`文字を追加すると、取り消し線スタイルを適用できます。</b>
    * <b>ただし、テンプレート登録時にあらかじめ\sをフィールドに追加しておいた場合は適用されません。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/raw-messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","content":"{内容}","buttons":[{"ordering":"{ボタン順序}","type":"{ボタンタイプ}","name":"{ボタン名}","linkMo":"{モバイルWebリンク}"}]}]}'
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

| 値                | タイプ | 説明    |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域 |
| - resultCode            | Integer | 結果コード |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否  |
| message                 | Object  | 本文領域 |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号 |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー |

### メッセージリストの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Query parameter] 1番or(2番、 3番)の条件は必須

| 値             | タイプ | 必須 | 説明                                |
| -------------------- | ------- | --------- | ---------------------------------------- |
| requestId            | String  | 条件必須(1番) | リクエストID                                    |
| startRequestDate     | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)          |
| endRequestDate       | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)           |
| startCreateDate      | String  | 条件必須(3番) | 登録日開始値(yyy-MM-dd HH:mm)                   |
| endCreateDate        | String  | 条件必須(3番) | 登録日終値(yyy-MM-dd HH:mm)                    |
| recipientNo          | String  | X         | 受信番号                             |
| senderKey            | String  | X         | 発信キー                                 |
| templateCode         | String  | X         | テンプレートコード                            |
| senderGroupingKey    | String  | X         | 発信グルーピングキー                            |
| recipientGroupingKey | String  | X         | 受信者グルーピングキー                           |
| messageStatus        | String  | X         | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| resultCode           | String  | X         | 送信結果(MRC01 -> 成功、MRC02 -> 失敗)          |
| createUser           | String  | X         | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| pageNum              | Integer | X         | ページ番号(基本：1)                            |
| pageSize             | Integer | X         | 照会件数(基本：15、最大: 1000)                |

* 90日以上前の送信リクエストデータは照会されません。
* 送信リクエスト日時の範囲は最大30日です。

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
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "messageStatus": String,
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

| 値                   | タイプ | 説明                               |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                            |
| - resultCode                | Integer | 結果コード                            |
| - resultMessage             | String  | 結果メッセージ                           |
| - isSuccessful              | Boolean | 成否                             |
| messageSearchResultResponse | Object  | 本文領域                            |
| - messages                  | List    | メッセージリスト                           |
| -- requestId                | String  | リクエストID                                    |
| -- recipientSeq             | Integer | 受信者シーケンス番号                       |
| -- plusFriendId             | String  | プラスフレンドID                                 |
| -- senderKey                | String  | 発信キー                                  |
| -- templateCode             | String  | テンプレートコード                           |
| -- recipientNo              | String  | 受信番号                            |
| -- content                  | String  | 本文                               |
| -- requestDate              | String  | リクエスト日
| -- createDate               | String  | 登録日時                            |
| -- receiveDate              | String  | 受信日時                            |
| -- resendStatus             | String  | 再送信ステータスコード                        |
| -- resendStatusName         | String  | 再送信ステータスコード名                        |
| -- messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| -- createUser               | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| -- resultCode               | String  | 受信結果コード                         |
| -- resultCodeName           | String  | 受信結果コード名                         |
| -- buttons                  | List    | ボタンリスト                            |
| --- ordering                | Integer | ボタン順序                            |
| --- type                    | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| --- name                    | String  | ボタン名                            |
| --- linkMo                  | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| --- linkPc                  | String  | PC Webリンク(WLタイプの場合は任意フィールド)                 |
| --- schemeIos               | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| --- schemeAndroid           | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| --- chatExtra               | String  | BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| --- chatEvent               | String  | BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| --- target                  | String  | Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| -- telNumber | String | X | TN(電話する)タイプのボタンの場合、伝達する電話番号 |
| -- senderGroupingKey        | String  | 発信グルーピングキー                            |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                           |
| - totalCount                | Integer | 総個数                              |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS再送信ステータス
| 値 | 説明                      |
| ----- | ------------------------------- |
| RSC01 | 再送信の対象ではない                 |
| RSC02 | 再送信の対象(送信結果が失敗の時、再送信が行われます。) |
| RSC03 | 再送信中                    |
| RSC04 | 再送信成功                  |
| RSC05 | 再送信失敗                  |

### メッセージ単件照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------- | ---------- |
| appkey       | String  | 固有のアプリキー |
| requestId    | String  | リクエストID      |
| recipientSeq | Integer | 受信者シーケンス番号 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
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
      "recipientSeq": Integer,
      "plusFriendId": String,
      "senderKey": String,
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "templateTitle": String,
      "templateSubtitle": String,
      "templateExtra": String,
      "templateAd": String,
      "templateHeader": String,
      "templateItem": {
        "list": [{
          "title": String,
          "description": String
        }],
        "summary": {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight": {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink": {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "requestDate": String,
      "receiveDate": String,
      "createDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
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
          "bizFormId": Integer,
          "pluginId": String,
          "relayId": String,
          "oneClickId": String,
          "productId": String,
          "target": String,
          "telNumber": String
        }
      ],
      "quickReplies": [
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
          "bizFormId": Integer,
          "target": String
          }
      ],
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

| 値               | タイプ | 説明                                |
| ---------------------- | ------- | ---------------------------------------- |
| header                 | Object  | ヘッダ領域                             |
| - resultCode           | Integer | 結果コード                             |
| - resultMessage        | String  | 結果メッセージ                            |
| - isSuccessful         | Boolean | 成否                              |
| message                | Object  | メッセージ                               |
| - requestId            | String  | リクエストID                                    |
| - recipientSeq         | Integer | 受信者シーケンス番号                        |
| - plusFriendId         | String  | プラスフレンドID                                 |
| - senderKey            | String  | 発信キー                                   |
| - templateCode         | String  | テンプレートコード                            |
| - recipientNo          | String  | 受信番号                             |
| - content              | String  | 本文                                |
|- templateTitle         | String  | テンプレートハイライトタイトル              |
|- templateSubtitle      | String  | テンプレートハイライトサブタイトル           |
|- templateExtra         | String  | テンプレート付加情報                     |
|- templateAd            | String  | テンプレート内の受信同意または簡単な広告文句   |
| - templateHeader | String | X | テンプレートヘッダ(最大16文字) |
| - templateItem | Object | X | アイテム |
| -- list | List | X | アイテムリスト(最小2件、最大10件) |
| --- title | String | X | タイトル(最大6文字) |
| --- description | String | X | ディスクリプション(最大23文字) |
| -- summary | Object | X | アイテム要約情報 |
| --- title | String | X | タイトル(最大6文字) |
| --- description | String | X | ディスクリプション(変数及び通貨単位、数字、カンマ、ピリオドのみ使用可能、最大14文字) |
| - templateItemHighlight | Object  |    X     | アイテムハイライト                                                                                                                                                            |
| --- title | String | X | タイトル(最大30文字、サムネイル画像がある場合は21文字) |
| --- description | String | X | ディスクリプション(最大19文字、サムネイル画像がある場合は13文字) |
| --- imageUrl            | String  |    X     | サムネイル画像アドレス                                                                                                                                                           |
| - templateRepresentLink | Object  |    X     | 代表リンク                                                                                                                                                                |
| -- linkMo               | String  |    X     | モバイルWebリンク(最大500文字)                                                                                                                                                      |
| -- linkPc               | String  |    X     | PC Webリンク(最大500文字)                                                                                                                                                       |
| -- schemeIos            | String  |    X     | iOSアプリリンク(最大500文字)                                                                                                                                                      |
| -- schemeAndroid        | String  |    X     | Androidアプリリンク(最大500文字)                                                                                                                                                    |
| - requestDate          | String  | リクエスト日時                             |
| - receiveDate          | String  | 受信日時                             |
| - createDate           | String  | 登録日時                            |
| - resendStatus         | String  | 再送信ステータスコード                         |
| - resendStatusName     | String  | 再送信ステータスコード名                          |
| - resendResultCode      | String  |    X     | 代替送信結果コード[SMS結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId       | String  |    X     | 代替送信SMSリクエストID                                                                                                                                                        |
| - messageStatus        | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| - resultCode           | String  | 受信結果コード                          |
| - resultCodeName       | String  | 受信結果コード名                           |
| - createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| - buttons              | List    | ボタンリスト                             |
| -- ordering            | Integer | ボタン順序                             |
| -- type                | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| -- name                | String  | ボタン名                             |
| -- linkMo              | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | PC Webリンク(WLタイプの場合は任意フィールド)                 |
| -- schemeIos           | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| -- bizFormId | Integer | X | ビジネスフォームID(BFタイプの場合、必須) |
| -- pluginId             | String  |    X     | プラグインID(最大24文字)                                                                                                                                                        |
| -- relayId | String | X | プラグイン実行の際、X-Kakao-Plugin-Relay-Idヘッダを介して受け取る値 |
| -- oneClickId | String | X | ワンクリック決済プラグインで使用する決済情報 |
| -- productId | String | X | ワンクリック決済プラグインで使用する決済情報 |
| -- target              | String  | Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| - quickReplies | List | X | クイック返信リスト(最大5件) |
| -- ordering | Integer | X | クイック返信の順序(クイック返信がある場合、必須) |
| -- type | String | X | クイック返信タイプ(WL: Webリンク、AL:アプリリンク、BK: ボットキーワード、BC:相談トーク切り替え、BT: ボット切り替え、BF:ビジネスフォーム) |
| -- name | String | X | クイック返信名(クイック返信がある場合、必須、最大14文字) |
| -- linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド、最大500文字) |
| -- linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド、最大500文字) |
| -- schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド、最大500文字) |
| -- schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド、最大500文字) |
| -- pluginId             | String  |    X     | プラグインID(最大24文字)                                                                                                                                                        |
| -- target | String | X | Webリンクタイプの場合、「"target":"out"」属性を追加すると外部リンク<br>デフォルトではアプリ内リンクとして送信 |
| -- telNumber | String | X | TN(電話する)タイプのボタンの場合、伝達する電話番号 |
| - messageOption        | Object  |	メッセージオプション                                         |
| -- price               | Integer |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| -- currencyType        | String  |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - senderGroupingKey    | String  | 発信グルーピングキー                            |
| - recipientGroupingKey | String  | 受信者グルーピングキー                           |

## 認証メッセージ

<span id="precautions-authword"></span>
1. 認証メッセージの送信時、含まれる必要がある認証文言案内

| 区分 | 認証文言 |
| --- | --- |
| 認証メッセージ | auth、password、verif、にんしょう、認証、パスワード、認証 |

- 例1)認証メッセージAPI送信リクエストした時、全文(テンプレート日本語識別子含む)に認証文言が含まれていない場合は、送信に失敗します。
- 例2)認証文言が英文の場合、大文字/小文字の区別なしで有効性チェックが行われます。


### メッセージ置換送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser" : String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "resendParameter": {
          "isResend" : boolean,
          "resendType" : String,
          "resendTitle" : String,
          "resendContent" : String,
          "resendSendNo" : String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
          }
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 値               | タイプ | 必須 | 説明                                |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| senderKey              | String  | O    | 発信キー                         |
| templateCode           | String  | O    | 登録した送信テンプレートコード(最大20桁)                    |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信) |
| createUser             | String  | X    | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| recipientList          | List    | O    | 受信者リスト(最大1000人)                         |
| - recipientNo          | String  | O    | 受信番号(最大15桁)                            |
| - templateParameter    | Object  | X    | テンプレートパラメータ<br>(テンプレートに置換する変数が含まれる時は必須)       |
| -- key                 | String  | X    | 置換キー(#{key})                             |
| -- value               | String  | X    | 置換キーにマッピングされるvalue値                |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| - buttons              | List    | X    | ボタン追加情報 |
| -- ordering            | Integer | X    |	ボタン順序(ボタンがある場合は必須) |
| -- chatExtra           | String  | X    |	BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | X    |	BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| -- target              | String  | X    |	Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |
| messageOption          | Object  | X    |	メッセージオプション                                         |
| - price                | Integer | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - currencyType         | String  | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| statsId                | String  | X    |	統計ID(発信検索条件には含まれません, 最大8文字) |

* <b>リクエスト日時は呼び出す時点から60日後まで設定可能です。</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信APIの仕様に応じてフィールドを入力する必要があります。(SMSサービスに登録された発信番号、各種フィールドの長さ制限など)</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は、途中で切れて代替送信されることがあります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","templateParameter":{"{日本語識別子フィールド}":"{置換データ}"}}]}'
```

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messages": [
    {
      "requestId": String,
      "recipientSeq": Integer,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String
    }
  ]
}
```

| 値                | タイプ | 説明    |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域 |
| - resultCode            | Integer | 結果コード |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否  |
| message                 | Object  | 本文領域 |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号 |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー |

### メッセージ全文送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同じkeyで要求すると、その要求を失敗処理します。 |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle" : String,
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
                    "target": String
                }
            ],
            "resendParameter": {
              "isResend" : boolean,
              "resendType" : String,
              "resendTitle" : String,
              "resendContent" : String,
              "resendSendNo" : String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| 値               | タイプ | 必須 | 説明                                |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| senderKey              | String  | O    | 発信キー                         |
| templateCode           | String  | O    | 登録した送信テンプレートコード(最大20桁)                    |
| requestDate            | String  | X    | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信) |
| senderGroupingKey      | String  | X    | 発信グルーピングキー(最大100文字)                        |
| createUser             | String  | X    | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存) |
| recipientList          | List    | O    | 受信者リスト(最大1,000人)                        |
| - recipientNo          | String  | O    | 受信番号(最大15桁)                            |
| - content              | String  | O    | 内容(最大1000文字)                             |
| - templateTitle        | String  | X    | タイトル(最大50桁)                            |
| - buttons              | List    | X    | ボタンリスト(最大5個)                             |
| -- ordering            | Integer | X    | ボタン順序(ボタンがある場合は必須)                      |
| -- type                | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| -- name                | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -- linkMo              | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大500文字)       |
| -- linkPc              | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大500文字)        |
| -- schemeIos           | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大500文字)       |
| -- schemeAndroid       | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大500文字)   |
| -- chatExtra           | String  | X    |	BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | X    |	BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| -- target              | String  | X    |	Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| - resendParameter      | Object  | X    | 代替発送情報 |
| -- isResend            | boolean | X    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| -- resendType          | String  | X    | 代替送信タイプ(SMS、LMS)<br>値がない場合は、テンプレート本文の長さに応じてタイプが決まります。 |
| -- resendTitle         | String  | X    | LMS代替送信タイトル(最大20文字)<br>(値がない場合は、プラスフレンドIDで再送信されます。) |
| -- resendContent       | String  | X    | 代替送信内容(最大1000文字)<br>(値がない場合は、テンプレートの内容で再送信されます。) |
| -- resendSendNo        | String  | X    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |
| - recipientGroupingKey | String  | X    | 受信者グルーピングキー(最大100文字)                       |
| messageOption          | Object  | X    |	メッセージオプション                                         |
| - price                | Integer | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - currencyType         | String  | X    |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| statsId                | String  | X    |	統計ID(発信検索条件には含まれません, 最大8文字) |

* <b>本文とボタンに置換が完了したデータを入れてください。</b>
* <b>リクエスト日時は呼び出す時点から60日後まで設定可能です。</b>

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","content":"{内容}","buttons":[{"ordering":"{ボタン順序}","type":"{ボタンタイプ}","name":"{ボタン名}","linkMo":"{モバイルWebリンク}"}]}]}'
```

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "body": {
    "messages": [
      {
        "requestId": String,
        "requestDate": String,
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "masterStatusCode": String,
        "content": String,
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

| 値                | タイプ | 説明    |
| ----------------------- | ------- | ------------ |
| header                  | Object  | ヘッダ領域 |
| - resultCode            | Integer | 結果コード |
| - resultMessage         | String  | 結果メッセージ |
| - isSuccessful          | Boolean | 成否  |
| message                 | Object  | 本文領域 |
| - requestId             | String  | リクエストID        |
| - senderGroupingKey     | String  | 発信グルーピングキー |
| - sendResults           | Object  | 送信リクエスト結果 |
| -- recipientSeq         | Integer | 受信者シーケンス番号 |
| -- recipientNo          | String  | 受信番号 |
| -- resultCode           | Integer | 送信リクエスト結果コード |
| -- resultMessage        | String  | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  | 受信者グルーピングキー |

### メッセージリストの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Query parameter] 1番or(2番、 3番)の条件は必須

| 値             | タイプ | 必須 | 説明                                |
| -------------------- | ------- | --------- | ---------------------------------------- |
| requestId            | String  | 条件必須(1番) | リクエストID                                    |
| startRequestDate     | String  | 条件必須(2番) | 送信リクエスト日の開始値(yyyy-MM-dd HH:mm)          |
| endRequestDate       | String  | 条件必須(2番) | 送信リクエスト日の終了値(yyyy-MM-dd HH:mm)           |
| startCreateDate      | String  | 条件必須(3番) | 登録日開始値(yyy-MM-dd HH:mm)                   |
| endCreateDate        | String  | 条件必須(3番) | 登録日終値(yyy-MM-dd HH:mm)                    |
| recipientNo          | String  | X         | 受信番号                             |
| senderKey            | String  | X         | 発信キー                                 |
| templateCode         | String  | X         | テンプレートコード                            |
| senderGroupingKey    | String  | X         | 発信グルーピングキー                            |
| recipientGroupingKey | String  | X         | 受信者グルーピングキー                           |
| messageStatus        | String  | X         | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| createUser           | String  | X         | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| resultCode           | String  | X         | 送信結果(MRC01 -> 成功、MRC02 -> 失敗)          |
| pageNum              | Integer | X         | ページ番号(基本：1)                            |
| pageSize             | Integer | X         | 照会件数(基本：15、最大: 1000)                |

* 90日以上前の送信リクエストデータは照会されません。
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
      "senderKey"    :  String,
      "templateCode" :  String,
      "recipientNo" :  String,
      "content" :  String,
      "requestDate" :  String,
      "createDate" : String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String,
      "createUser" : String,
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
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount" :  Integer
  }
}
```

| 値                   | タイプ | 説明                               |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                            |
| - resultCode                | Integer | 結果コード                            |
| - resultMessage             | String  | 結果メッセージ                           |
| - isSuccessful              | Boolean | 成否                             |
| messageSearchResultResponse | Object  | 本文領域                            |
| - messages                  | List    | メッセージリスト                           |
| -- requestId                | String  | リクエストID                                    |
| -- recipientSeq             | Integer | 受信者シーケンス番号                       |
| -- plusFriendId             | String  | プラスフレンドID                                 |
| -- senderKey                | String  | 発信キー                                  |
| -- templateCode             | String  | テンプレートコード                           |
| -- recipientNo              | String  | 受信番号                            |
| -- content                  | String  | 本文                               |
| -- requestDate              | String  | リクエスト日時                            |
| -- createDate               | String  | 登録日時                            |
| -- receiveDate              | String  | 受信日時                            |
| -- resendStatus             | String  | 再送信ステータスコード                        |
| -- resendStatusName         | String  | 再送信ステータスコード名                        |
| -- messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| -- resultCode               | String  | 受信結果コード                         |
| -- resultCodeName           | String  | 受信結果コード名                         |
| -- createUser               | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| -- buttons                  | List    | ボタンリスト                            |
| --- ordering                | Integer | ボタン順序                            |
| --- type                    | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| --- name                    | String  | ボタン名                            |
| --- linkMo                  | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| --- linkPc                  | String  | PC Webリンク(WLタイプの場合は任意フィールド)                 |
| --- schemeIos               | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| --- schemeAndroid           | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| --- chatExtra               | String  | BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| --- chatEvent               | String  | BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| --- target                  | String  | Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| -- senderGroupingKey        | String  | 発信グルーピングキー                            |
| -- recipientGroupingKey     | String  | 受信者グルーピングキー                           |
| - totalCount                | Integer | 総個数                              |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

#### SMS/LMS再送信ステータス
| 値 | 説明                      |
| ----- | ------------------------------- |
| RSC01 | 再送信の対象ではない                 |
| RSC02 | 再送信の対象(送信結果が失敗の時、再送信が行われます。) |
| RSC03 | 再送信中                    |
| RSC04 | 再送信成功                  |
| RSC05 | 再送信失敗                  |

### メッセージ単件照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------- | ---------- |
| appkey       | String  | 固有のアプリキー |
| requestId    | String  | リクエストID      |
| recipientSeq | Integer | 受信者シーケンス番号 |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
```

#### レスポンス
```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
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

| 値               | タイプ | 説明                                |
| ---------------------- | ------- | ---------------------------------------- |
| header                 | Object  | ヘッダ領域                             |
| - resultCode           | Integer | 結果コード                             |
| - resultMessage        | String  | 結果メッセージ                            |
| - isSuccessful         | Boolean | 成否                              |
| message                | Object  | メッセージ                               |
| - requestId            | String  | リクエストID                                    |
| - recipientSeq         | Integer | 受信者シーケンス番号                        |
| - plusFriendId         | String  | プラスフレンドID                                 |
| - senderKey            | String  | 発信キー                                   |
| - templateCode         | String  | テンプレートコード                            |
| - recipientNo          | String  | 受信番号                             |
| - content              | String  | 本文                                |
| - templateTitle        | String  | テンプレートハイライトタイトル              |
| - templateSubtitle     | String  | テンプレートハイライトサブタイトル           |
| - templateExtra        | String  | テンプレート付加情報                     |
| - templateAd           | String  | テンプレート内の受信同意または簡単な広告文句   |
| - requestDate          | String  | リクエスト日時                             |
| - createDate           | String  | 登録日時                            |
| - receiveDate          | String  | 受信日時                             |
| - resendStatus         | String  | 再送信ステータスコード                         |
| - resendStatusName     | String  | 再送信ステータスコード名                          |
| - resendResultCode     | String  | 再送結果コード[SMS結果コード](https://docs.toast.com/ja/Notification/SMS/ja/error-code/#api) |
| - resendRequestId      | String  | 再送SMSリクエストID                                                  |
| - messageStatus        | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| - resultCode           | String  | 受信結果コード                          |
| - resultCodeName       | String  | 受信結果コード名                           |
| - createUser           | String  | 登録者(コンソールから送信する場合、ユーザーUUIDとして保存)|
| - buttons              | List    | ボタンリスト                             |
| -- ordering            | Integer | ボタン順序                             |
| -- type                | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| -- name                | String  | ボタン名                             |
| -- linkMo              | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| -- linkPc              | String  | PC Webリンク(WLタイプの場合は任意フィールド)                 |
| -- schemeIos           | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| -- schemeAndroid       | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- chatExtra           | String  | BC(相談トークに切替) / BT(Botに切替)タイプボタンの時、伝達するメタ情報 |
| -- chatEvent           | String  | BT(Botに切替)タイプボタンの時、接続するBotイベント名 |
| -- target              | String  | Webリンクボタンの場合、 "target":"out"プロパティ追加時のアウトリンク<br>基本インアプリリンクで送信 |
| - messageOption        | Object  |	メッセージオプション                                         |
| -- price               | Integer |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| -- currencyType        | String  |	message(ユーザーに伝達されるメッセージ)内に含まれた価格/金額/決済金額(モーメント広告に該当) |
| - senderGroupingKey    | String  | 発信グルーピングキー                            |
| - recipientGroupingKey | String  | 受信者グルーピングキー                           |

## メッセージ
### メッセージ送信取消

#### リクエスト

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| --------- | ------ | ------ |
| appkey    | String | 固有のアプリキー |
| requestId | String | リクエストID  |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Query parameter]

| 値    | タイプ | 必須 | 説明                               |
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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### メッセージ結果アップデートの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Query parameter]

| 値            | タイプ | 必須 | 説明                          |
| ------------------- | ------- | ---- | ---------------------------------- |
| startUpdateDate     | String  | O    | 結果アップデート照会開始時間(yyyy-MM-dd HH:mm) |
| endUpdateDate       | String  | O    | 結果アップデート照会終了時間(yyyy-MM-dd HH:mm) |
| alimtalkMessageType | String  | X    | お知らせトークメッセージタイプ(NORMAL、AUTH)           |
| pageNum             | Integer | X    | ページ番号(基本：1)                      |
| pageSize            | Integer | X    | 照会件数(基本：15、最大: 1000)          |

#### レスポンス
```
{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "messages" : [
    {
      "requestId" :  String,
      "recipientSeq" : Integer,
      "requestDate" :  String,
      "createDate" :  String,
      "receiveDate" : String,
      "resendStatus" :  String,
      "resendStatusName" :  String,
      "resendResultCode" :  String,
      "resendRequestId" :  String,
      "messageStatus" :  String,
      "resultCode" :  String,
      "resultCodeName" : String
    }
  ]
}
```

| 値                   | タイプ | 説明                               |
| --------------------------- | ------- | ---------------------------------------- |
| header                      | Object  | ヘッダ領域                            |
| - resultCode                | Integer | 結果コード                            |
| - resultMessage             | String  | 結果メッセージ                           |
| - isSuccessful              | Boolean | 成否                             |
| messages                  | List    | メッセージリスト                           |
| - requestId                | String  | リクエストID                                    |
| - recipientSeq             | Integer | 受信者シーケンス番号                       |
| - requestDate              | String  | リクエスト日時                            |
| - createDate               | String  | 作成日時                            |
| - receiveDate              | String  | 受信日時                            |
| - resendStatus             | String  | 再送信ステータスコード                        |
| - resendStatusName         | String  | 再送信ステータスコード名                        |
| - resendResultCode         | String  | 再送結果コード[SMS結果コード](https://docs.toast.com/ja/Notification/SMS/ja/error-code/#api)                        |
| - resendRequestId          | String  | 再送SMSリクエストID                        |
| - messageStatus            | String  | リクエストステータス(COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> キャンセル) |
| - resultCode               | String  | 受信結果コード                         |
| - resultCodeName           | String  | 受信結果コード名                         |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

## 大量送信
### 大量送信リクエストリスト照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
|X-Secret-Key|	String|	固有のシークレットキー |

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
https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
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
        "templateCode": String,
        "masterStatusCode": String,
        "content": String,
        "buttons": [
          {
            "ordering": 1,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
          }
        ],
        "fileId": String,
        "templateExtra": String,
        "templateAd": String,
        "templateTitle": String,
        "templateSubtitle": String,
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
| -- createDate | String | 作成日 |
| -- createUser | String | 作成日 |
| -- plusFriendId | String | プラスフレンドID |
| -- senderKey | String| 発信キー(40文字) |
| -- masterStatusCode | String | 大量送信ステータスコード(WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | 内容 |
| -- buttons | List | ボタンリスト |
| --- ordering | String | ボタンの順序 |
| --- type | String | ボタンの種類<br/> - WL：Webリンク<br/> - AL：アプリリンク<br/> - DS：配送照会<br/> - BK：Botキーワード<br/> - MD：メッセージ伝達<br/> - BC：相談トーク切り替え<br/> - BT：Bot切り替え<br/> - AC：チャンネル追加[広告追加/複合型のみ] |
| --- name | String | ボタン名 |
| --- linkMo | String | モバイルWebリンク(WLタイプの場合は必須フィールド) |
| --- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)|
| --- schemeIos | String | IOSアプリリンク(ALタイプの場合は必須フィールド) |
| --- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| --- chatExtra | String | BC：相談トークに切り替える時に伝達するメタ情報<br/> BT：Botに切り替える時に伝達するメタ情報 |
| --- chatEvent | String | BT：Botに切り替える時に接続するBotイベント名 |
| --- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| -- fileId | String | 添付ファイルID |
| -- templateCode |	String | テンプレートコード(最大20文字) |
| -- templateExtra | String | テンプレート付加情報(テンプレートメッセージタイプが[付加情報型/複合型]の場合は必須) |
| -- templateAd | String | テンプレート内の受信同意リクエストまたは簡単な広告文言 |
| -- tempalteTitle| String | テンプレートタイトル(最大50文字、Android：2行、23文字以上省略処理、IOS：2行、27文字以上省略処理) |
| -- templateSubtitle| String | テンプレート補助文言(最大50文字、Android：18文字以上省略処理、IOS：21文字以上省略処理) |
| -- autoSendYn | String | 自動送信を行うかどうか |
| -- statsId | String | 統計ID |
| -- createDate | String | 作成日 |
| -- createUser | String | 登録者(コンソールから送信時、ユーザーUUIDに保存) |
| - totalCount | Integer | 総数


### 大量送信大量送信受信者リスト照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String |	固有のアプリキー |
| requestId |	String |	リクエストID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	説明|
|---|---|---|
| X-Secret-Key |	String|	固有のシークレットキー |


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
https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
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

|値|	タイプ|	説明|
|---|---|---|
| header | Object |	ヘッダ領域 |
| - resultCode |	Integer |	結果コード |
| - resultMessage |	String | 結果メッセージ |
| - isSuccessful |	Boolean | 成否 |
| body | Object | 本文領域 |
| - messages | Object | メッセージリスト |
| -- requestId | String | リクエストID |
| -- recipientSeq | String | 受信者シーケンス番号 |
| -- recipientNo | String | 受信番号 |
| -- requestDate | String | リクエスト日 |
| -- receiveDate | String | 受信日 |
| -- messageStatus | String | メッセージ状態 |
| -- resultCode | String | 結果コード |
| -- resultCodeName | String | 結果コード内容 |
| - totalCount | Integer | 総数 |

### 大量送信大量送信受信者照会

#### リクエスト
[URL]
```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey |	String | 固有のアプリキー |
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
| pageNum | optional, Integer | - | X | ページ番号 |
| pageSize | optional, Integer | 1000 | X | 検索数 |

#### cURL
```
curl -X GET \
https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients/1?requestId='"${REQUEST_ID}" \
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
        "templateCode": String,
        "recipientNo": String,
        "content": String,
        "templateTitle": String,
        "templateSubtitle": String,
        "templateExtra": String,
        "templateAd": String,
        "requestDate": String,
        "receiveDate": String,
        "createDate": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "messageStatus": String,
        "resultCode": String,
        "resultCodeName": String,
        "createUser": String,
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
                "chatEvent": String
                "target": String
	      "pluginId": String,
	      "telNumber": String                
            }
        ],
        "messageOption": {
          "price": Integer,
          "currencyType": String
        }
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
| - requestId | String | リクエストID |
| - recipientSeq | String | 受信者シーケンス番号 |
| - plusFriendId | String | プラスフレンドID |
| - senderKey | String | 送信者ID |
| - templateCode |	String | テンプレートコード(最大20文字) |
| - recipientNo | String | 受信番号 |
| - content | String | 内容 |
| - tempalteTitle| String | テンプレートタイトル(最大50文字、Android：2行、23文字以上省略処理、IOS：2行、27文字以上省略処理) |
| - templateSubtitle| String | テンプレート補助文言(最大50文字、Android：18文字以上省略処理、IOS：21文字以上省略処理) |
| - templateExtra | String | テンプレート付加情報(テンプレートメッセージタイプが[付加情報型/複合型]の場合は必須) |
| - templateAd | String | テンプレート内の受信同意リクエストまたは簡単な広告文言 |
| - requestDate | String | リクエスト日 |
| - receiveDate | String | 受信日 |
| - createDate | String | 作成日 |
| - resendStatus | String | 代替送信ステータスコード(RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[以下の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)]参考) |
| - resendStatusName | String | 代替送信ステータス名 |
| - resendResultCode | String | 代替送信結果コード[SMS結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | 代替送信リクエストID |
| - messageStatus | String | 大量受信者送信ステータスコード(READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | 結果ステータスコード |
| - resultCodeName | String | 結果状態名 |
| - createUser | String | 作成ユーザーID |
| - buttons | List | ボタンリスト |
| -- ordering | String | ボタンの順序 |
| -- type | String | ボタンの種類<br/> - WL：Webリンク<br/> - AL：アプリリンク<br/> - DS：配送照会<br/> - BK：Botキーワード<br/> - MD：メッセージ配信<br/> - BC：相談トーク切り替え<br/> - BT：Bot切り替え<br/> - AC：チャンネル追加[広告追加/複合型のみ] |
| -- name | String | ボタン名 |
| -- linkMo | String | モバイルWebリンク(WLタイプの場合は必須フィールド) |
| -- linkPc | String | PC Webリンク(WLタイプの場合は任意フィールド)|
| -- schemeIos | String | IOSアプリリンク(ALタイプの場合は必須フィールド) |
| -- schemeAndroid | String | Androidアプリリンク(ALタイプの場合は必須フィールド) |
| -- chatExtra | String | BC：相談トークに切り替える時に伝達するメタ情報<br/> BT：Botに切り替える時に伝達するメタ情報 |
| -- chatEvent | String | BT：Botに切り替える時に接続するBotイベント名 |
| -- target|	String|	Webリンクボタンの場合、"target"："out"プロパティを追加時アウトリンク<br>基本アプリ内リンクで送信 |
| - messageOption | Boolean | メッセージオプション |
|-- price | Integer |	message(ユーザーに伝達されるメッセージ)内に含まれる価格/金額/決済金額(モーメント広告に該当) |
|-- currencyType | String |	message(ユーザーに伝達されるメッセージ)内に含まれる価格/金額/決済金額の通貨単位KRW、USD、EURなどの国際通貨コードを使用(モーメント広告に該当) |


## テンプレート

### テンプレートカテゴリー照会
#### リクエスト
[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
|---|---|---|---|
| X-Secret-Key | String | O    | コンソールで作成できます。 |

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "categories": [
    {
      "name": String,
      "subCategories": [
        {
          "code": String,
          "name": String,
          "groupName": String,
          "inclusion": String,
          "exclusion": String
        }
      ]
    }
  ]
}
```

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |
|categories|	List|	カテゴリー一覧 |
|- name | String | カテゴリー名 |
|- subCategories | List |	サブカテゴリーのリスト |
|-- code | String | カテゴリーコード(テンプレートの登録/変更する際、使用) |
|-- name | String |	カテゴリー名 |
|-- groupName | String |	カテゴリーグループ名 |
|-- inclusion | String |	カテゴリー対象テンプレートの説明 |
|-- exclusion| String| カテゴリの除外対象のテンプレートの説明 |

### テンプレートの登録
#### リクエスト
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request body]

```
{
  "templateCode" : String,
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
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

| 値       | タイプ | 必須 | 説明                               |
| --------------- | ------- | ---- | ---------------------------------------- |
| templateCode    | String  | O    | テンプレートコード(最大20文字)                           |
| templateName    | String  | O    | テンプレート名(最大150文字)                             |
| templateContent | String  | O    | テンプレート本文(最大1000文字)                         |
| templateMessageType| String | X  | テンプレートメッセージタイプ(BA:基本型、EX:付加情報型、AD:広告追加型、MI:複合型)<br>EX：templateExtraフィールド必須<br>MI：templateExtraフィールド必須」 |
|templateEmphasizeType| String| X  | テンプレートハイライトタイプ（NONE：基本、TEXT：ハイライト、ITEM_LIST:アイテムリスト型, default：NONE）<br>TEXT：templateTitle、templateSubtitleフィールド必須<br>ITEM_LIST: 画像、ヘッダ、アイテムハイライトアイテムリストのうち1つ以上必須 |
| templateExtra     | String  | X  | テンプレート付加情報 |
|tempalteTitle      | String  | X  | テンプレートのタイトル(最大50字、Android:2行、23字以上のコマ処理、iOS:2行、27字以上のコマ処理) |
|templateSubtitle   | String  | X  | テンプレートの補助フレーズ(最大50文字、Android:18字以上のコマを省く、iOS:21字以上のコマを省く) |
|templateImageName  | String  |	X  | 画像名（アップロードされたファイル名） |
|templateImageUrl   | String  |	X  | 画像のURL |
|securityFlag| Boolean | X| セキュリティテンプレートかどうか<br>OTPなどのセキュリティメッセージの場合、設定<br>発信当時のメインデバイスを除くすべてのデバイスにメッセージテキストミノチュル(default: false) |
|categoryCode| String | X | テンプレートのカテゴリコード(テンプレートカテゴリー照会API参考, default: 999999)<br>カテゴリーを入力し、テンプレートを優先審査 |
| buttons         | List    | X    | ボタンリスト(最大5個)                             |
| -ordering       | Integer | X    | ボタン順序(1~5)                               |
| -type           | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |[広告追加/複合型のみ]) |
| -name           | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -linkMo         | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大500文字)       |
| -linkPc         | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大500文字)        |
| -schemeIos      | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大500文字)       |
| -schemeAndroid  | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大500文字)   |

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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### テンプレートの修正
#### リクエスト
[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |
| senderKey | String | 発信キー |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request body]

```
{
  "templateName" : String,
  "templateContent" : String,
  "templateMessageType": String,
  "templateEmphasizeType" : String,
  "templateExtra": String,
  "templateTitle" : String,
  "templateSubtitle" : String,
  "templateImageName" : String,
  "templateImageUrl" : String,
  "securityFlag": Boolean,
  "categoryCode": String,
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

| 値       | タイプ | 必須 | 説明                               |
| --------------- | ------- | ---- | ---------------------------------------- |
| templateName    | String  | O    | テンプレート名(最大150文字)                             |
| templateContent | String  | O    | テンプレート本文(最大1000文字)                         |
| templateMessageType| String | X  | テンプレートメッセージタイプ(BA:基本型、EX:付加情報型、AD:広告追加型、MI:複合型)<br>EX：templateExtraフィールド必須<br>MI：templateExtraフィールド必須」 |
| templateEmphasizeType| String| X  | テンプレートハイライトタイプ（NONE：基本、TEXT：ハイライト、default：NONE）<br>TEXT：templateTitle、templateSubtitleフィールド必須 |
| templateExtra   | String  | X    |テンプレート付加情報 |
| tempalteTitle| String | X| テンプレートのタイトル(最大50字、Android:2行、23字以上のコマ処理、iOS:2行、27字以上のコマ処理) |
| templateSubtitle| String | X| テンプレートの補助フレーズ(最大50文字、Android:18字以上のコマを省く、iOS:21字以上のコマを省く) |
| templateImageName  | String  |	X  | 画像名（アップロードされたファイル名） |
| templateImageUrl   | String  |	X  | 画像のURL |
|securityFlag| Boolean | X| セキュリティテンプレートかどうか<br>OTPなどのセキュリティメッセージの場合、設定<br>発信当時のメインデバイスを除くすべてのデバイスにメッセージテキストミノチュル(default: false) |
|categoryCode| String | X | テンプレートのカテゴリコード(テンプレートカテゴリー照会API参考, default: 999999)<br>カテゴリーを入力し、テンプレートを優先審査 |
| buttons         | List    | X    | ボタンリスト(最大5個)                             |
| -ordering       | Integer | X    | ボタン順序(1~5)                               |
| -type           | String  | X    | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |[広告追加/複合型のみ]) |
| -name           | String  | X    | ボタン名(ボタンがある場合は必須、最大14文字)              |
| -linkMo         | String  | X    | モバイルWebリンク(WLタイプの場合は必須フィールド、最大500文字)       |
| -linkPc         | String  | X    | PC Webリンク(WLタイプの場合は任意フィールド、最大500文字)        |
| -schemeIos      | String  | X    | iOSアプリリンク(ALタイプの場合は必須フィールド、最大500文字)       |
| -schemeAndroid  | String  | X    | Androidアプリリンク(ALタイプの場合は必須フィールド、最大500文字)   |

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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### テンプレートの削除
#### リクエスト
[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |
| plusFriendId | String | 発信キー |
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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

* 承認されたテンプレートを削除する時、NHN Cloud内でのみ削除されます。(3日間未送信のテンプレートのみ削除可能)
* 承認されたテンプレートの場合、カカオトークBizメッセージの制約のため、カカオ内部データは削除できません。
* カカオに残っているテンプレートは1年間使っていないと休眠処理され、休眠状態が1年間続くと削除処理されます。(カカオでテンプレートが休眠に切り替わるときや、削除されるときは担当者に通知が送信されます。)


### テンプレートの問い合わせをする
#### リクエスト
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |
| plusFriendId | String | 発信キー |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request body]

```
{
  "comment" : String
}
```

| 値 | タイプ | 必須 | 説明 |
| ------- | ------ | ---- | ----- |
| comment | String | O    | お問い合わせ内容 |

* REJステータスのテンプレートにコメントを付けると、REQステータスに変更されます。

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

| 値       | タイプ | 説明 |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - resultCode    | Integer | 結果コード |
| - resultMessage | String  | 結果メッセージ |
| - isSuccessful  | Boolean | 成否 |

### ファイルを添付してテンプレートお問い合わせ
#### リクエスト
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のAppkey|
|plusFriendId|	String|	発信キー |
|templateCode|	String|	テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | コンソールで作成できます。 |

[Request Body]

```
{
  "comment" : String,
  "attachments" : File
}
```

|値|	タイプ|	必須| 	説明                                                                                                                                      |
|---|---|---|------------------------------------------------------------------------------------------------------------------------------------------|
|comment|	String |	O | お問い合わせ内容                                                                                                                                 |
|attachments| List<File> | X | 添付ファイルリスト(最大10個)<br>- 対応拡張子 ( .png, .jpg, .jpeg, .gif, .pdf, .hwp, .doc, .docx )<br>- 各個別ファイルの最大サイズ 50mb<br>- アップロード可能なファイルの総最大サイズ 100mb |

* REJステータスのテンプレートにコメントを付けると、REQステータスに変更されます。

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- resultCode|	Integer|	結果コード|
|- resultMessage|	String| 結果メッセージ|
|- isSuccessful|	Boolean| 成否|

### テンプレートリストの照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
| ------ | ------ | ------ |
| appkey | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Query parameter]

| 値      | タイプ | 必須 | 説明    |
| -------------- | ------- | ---- | ------------- |
| plusFriendId   | String  | X    | 発信キー      |
| templateCode   | String  | X    | テンプレートコード |
| templateName   | String  | X    | テンプレート名 |
| templateStatus | String  | X    | テンプレートステータスコード |
| pageNum        | Integer | X    | ページ番号(基本：1) |
| pageSize       | Integer | X    | 照会件数(基本：15、最大: 1000) |

| テンプレートステータスコード | 説明 |
| --------- | ---- |
| TSC01     | リクエスト |
| TSC02     | 検収中 |
| TSC03     | 承認 |
| TSC04     | 差し戻し |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/templates?plusFriendId={発信キー}&templateStatus={テンプレートステータスコード}"
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
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
              "templateMessageType" : String,
              "templateExtra" : String,
              "templateAd" : String,
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
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
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

| 値            | タイプ | 説明                                                                                                     |
| -------------------- | ------- |--------------------------------------------------------------------------------------------------------|
| header               | Object  | ヘッダ領域                                                                                                  |
| - resultCode         | Integer | 結果コード                                                                                                  |
| - resultMessage      | String  | 結果メッセージ                                                                                                |
| - isSuccessful       | Boolean | 成否                                                                                                     |
| templateListResponse | Object  | 本文領域                                                                                                   |
| - templates          | List    | テンプレートリスト                                                                                              |
| -- plusFriendId      | String  | プラスフレンドID                                                                                              |
| -- plusFriendType    | String  | プラスフレンドタイプ(NORMAL、GROUP)                                                                               |
| -- templateCode      | String  | テンプレートコード                                                                                              |
| -- templateName      | String  | テンプレート名                                                                                                |
| -- templateContent   | String  | テンプレート本文                                                                                               |
| -- templateEmphasizeType| String| テンプレートハイライトタイプ（NONE：基本、TEXT：ハイライト、default：NONE）<br>TEXT：templateTitle、templateSubtitleフィールド必須          |
| -- tempalteTitle     | String  | テンプレートのタイトル(最大50字、Android:2行、23字以上のコマ処理、iOS:2行、27字以上のコマ処理)                                             |
| -- templateSubtitle  | String  | テンプレートの補助フレーズ(最大50文字、Android:18字以上のコマを省く、iOS:21字以上のコマを省く)                                              |
| -- templateImageName | String  | 画像名（アップロードされたファイル名）                                                                                    |
| -- templateImageUrl  | String  | 画像のURL                                                                                                 |
| -- templateMessageType| String  | テンプレートメッセージタイプ(BA:基本型、EX:付加情報型、AD:広告追加型、MI:複合型)<br>EX：templateExtraフィールド必須<br>MI：templateExtraフィールド必須」 |
| -- templateExtra     | String  | テンプレート付加情報                                                                                             |
| -- templateAd        | String  | テンプレート内の受信同意または簡単な広告文句                                                                                 |
| -- buttons           | List    | ボタンリスト                                                                                                 |
| --- ordering         | Integer | ボタン順序(1~5)                                                                                             |
| --- type             | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加)              |
| --- name             | String  | ボタン名                                                                                                   |
| --- linkMo           | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                                                                           |
| --- linkPc           | String  | PC Webリンク(WLタイプの場合は任意フィールド)                                                                            |
| --- schemeIos        | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                                                                            |
| --- schemeAndroid    | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)                                                                        |
| -- comments          | List    | 検収結果                                                                                                   |
| --- id               | Integer | お問い合わせID                                                                                               |
| --- content          | String  | お問い合わせ内容                                                                                               |
| --- userName          | String  | 作成者                                                                                                    |
| --- createAt          | String  | 登録日                                                                                                    |
| --- attachment        | List | 添付ファイル                                                                                                 |
| ---- originalFileName | String | 添付ファイル名                                                                                                |
| ---- filePath         | String | 添付ファイルへのパス                                                                                             |
| --- status            | String  | 応答状態(INQ：お問い合わせ、APR：承認、REJ：差し戻し、REP：返信, REQ : 検査中)                                                     |
| -- status            | String  | テンプレートのステータス                                                                                           |
| -- statusName        | String  | テンプレートのステータス名                                                                                          |
| -- createDate        | String  | 作成日時                                                                                                   |
| - totalCount         | Integer | 総個数                                                                                                    |

### テンプレートチャンネル追加型に変更

#### リクエスト
[URL]
```
PUT  /alimtalk/v2.3/appkeys/{appKey}/senders/{senderKey}/templates/{templateCode}/convert-add-channel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前 |	タイプ|	説明|
|---|---|---|
|appkey|	String|	固有のアプリキー|
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
|X-Secret-Key|	String| O | コンソールで作成できます。  |

* グループプロフィールに登録されたテンプレートは、チャンネルタイプに変換することはできません

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

### テンプレートの修正リスト照会

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値 | タイプ | 説明 |
|---|---|---|
| appkey       | String | 固有のアプリキー |
| plusFriendId | String | プラスフレンドID |
| templateCode | String | テンプレートコード |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
|---|---|---|---|
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

#### レスポンス
```

{
  "header" : {
      "resultCode" :  Integer,
      "resultMessage" :  String,
      "isSuccessful" :  boolean
  },
  "templateModificationsResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateContent": String,
              "templateEmphasizeType": String,
              "templateTitle" : String,
              "templateSubtitle" : String,
              "templateImageName" : String,
              "templateImageUrl" : String,
              "templateMessageType" : String,
              "templateExtra" : String,
              "templateAd" : String,
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
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                    }  
                ],
                "status": String,
                "statusName": String,
                "activated": boolean,
                "createDate": String
            }
        ],
        "totalCount": Integer
    }
}
```

| 値            | タイプ | 説明                               |
| -------------------- | ------- | ---------------------------------------- |
| header               | Object  | ヘッダ領域                            |
| - resultCode         | Integer | 結果コード                            |
| - resultMessage      | String  | 結果メッセージ                           |
| - isSuccessful       | Boolean | 成否                             |
| templateModificationsResponse | Object  | 本文領域                            |
| - templates          | List    | テンプレートリスト                          |
| -- plusFriendId      | String  | プラスフレンドID                                 |
| -- plusFriendType    | String  | プラスフレンドタイプ(NORMAL、GROUP)                  |
| -- templateCode      | String  | テンプレートコード                           |
| -- templateName      | String  | テンプレート名                             |
| -- templateContent   | String  | テンプレート本文                           |
| -- templateEmphasizeType| String| テンプレートハイライトタイプ（NONE：基本、TEXT：ハイライト、default：NONE）<br>TEXT：templateTitle、templateSubtitleフィールド必須 |
| -- tempalteTitle     | String  | テンプレートのタイトル(最大50字、Android:2行、23字以上のコマ処理、iOS:2行、27字以上のコマ処理) |
| -- templateSubtitle  | String  | テンプレートの補助フレーズ(最大50文字、Android:18字以上のコマを省く、iOS:21字以上のコマを省く) |
| -- templateImageName | String  | 画像名（アップロードされたファイル名） |
| -- templateImageUrl  | String  | 画像のURL |
| -- templateMessageType| String  | テンプレートメッセージタイプ(BA:基本型、EX:付加情報型、AD:広告追加型、MI:複合型)<br>EX：templateExtraフィールド必須<br>MI：templateExtraフィールド必須」 |
| -- templateExtra     | String  | テンプレート付加情報 |
| -- templateAd        | String  | テンプレート内の受信同意または簡単な広告文句 |
| -- buttons           | List    | ボタンリスト                            |
| --- ordering         | Integer | ボタン順序(1~5)                               |
| --- type             | String  | ボタンタイプ(WL：Webリンク、AL：アプリリンク、DS：配送照会、BK：Botキーワード、MD：メッセージ伝達、BC：相談トーク転換、BT：Bot転換、AC：チャンネル追加) |
| --- name             | String  | ボタン名                            |
| --- linkMo           | String  | モバイルWebリンク(WLタイプの場合は必須フィールド)                |
| --- linkPc           | String  | PC Webリンク(WLタイプの場合は任意フィールド)                 |
| --- schemeIos        | String  | iOSアプリリンク(ALタイプの場合は必須フィールド)                |
| --- schemeAndroid    | String  | Androidアプリリンク(ALタイプの場合は必須フィールド)            |
| -- comments          | List    | 検収結果                            |
| --- id               | Integer | お問い合わせID                                   |
| --- content          | String  | お問い合わせ内容                            |
| ---userName          | String  | 作成者                               |
| ---createAt          | String  | 登録日                            |
| --- attachment        | List | 添付ファイル                           |
| ---- originalFileName | String | 添付ファイル名                        |
| ---- filePath         | String | 添付ファイルへのパス                   |
| ---status            | String  | 応答状態(INQ：お問い合わせ、APR：承認、REJ：差し戻し、REP：返信, REQ : 検査中) |
| -- status            | String  | テンプレートのステータス                           |
| -- statusName        | String  | テンプレートのステータス名                           |
| -- activated         | Boolean  | 有効かどうか                            |
| -- createDate        | String  | 作成日時                            |
| - totalCount         | Integer | 総個数                              |

### テンプレート画像の登録
#### リクエスト
[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| 値 | タイプ | 説明 |
|---|---|---|
| appkey       | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値    | タイプ | 必須 | 説明                               |
|---|---|---|---|
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|file|	File|	O |	画像ファイル |

[例]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

#### レスポンス
```
{
  "header" : {
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| 値            | タイプ | 説明                               |
| -------------------- | ------- | ---------------------------------------- |
| header               | Object  | ヘッダ領域                            |
| - resultCode         | Integer | 結果コード                            |
| - resultMessage      | String  | 結果メッセージ                           |
| - isSuccessful       | Boolean | 成否                             |
| templateImage         |	Object |	本文領域|
|- templateImageName    | String |	画像名（アップロードされたファイル名） |
|- templateImageUrl     | String |	画像のURL |

## 代替送信管理
### SMS AppKey登録

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```

| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |


[Request body]

```
{
    "resendAppKey": String
}
```

| 値    | タイプ | 必須 | 説明                               |
|---|---|---|---|
|resendAppKey|	String|	O | 代替発送に設定するSMS AppKey |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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
POST  /alimtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ | 説明 |
| ------------ | ------ | -------- |
| appkey       | String | 固有のアプリキー |

[Header]
```
{
  "X-Secret-Key": String
}
```

| 値    | タイプ | 必須 | 説明                               |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。 |

[Request body]

```
{  
   "plusFriendId": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| 値               | タイプ | 必須 | 説明                                |
| ---------------------- | ------- | ---- | ---------------------------------------- |
| plusFriendId           | String  | O    | プラスフレンドID(最大30文字)                         |
| isResend             | boolean | O    | 送信失敗時、代替送信するかどうか<br>コンソールで送信失敗設定をした時、デフォルト設定は再送信になっています。 |
| resendSendNo         | String  | O    | 代替送信発信番号(最大13桁)<br><span style="color:red">(SMSサービスに登録された発信番号ではない場合、代替送信が失敗することがあります。)</span> |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"plusFriendId": "@プラスフレンド","isResend": true,"resendSendNo": "01012341234" }
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
