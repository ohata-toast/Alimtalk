## Notification > KakaoTalk Bizmessage > Webhook > API Guide

<span id="webhook"></span>
## Webフック
KakaoTalk Bizmessageサービス内で特定のイベントが発生すると、Webhook設定で定義されたURLにPOSTリクエストを作成します。<br>
作成されたPOSTリクエストについてのAPIドキュメントです。

### Webフック送信

[URL]

|Http method|	URI|
|---|---|
| POST | Webフック設定に定義した対象URL |

[Header]

|値|	タイプ|	説明|
|---|---|---|
|X-Toast-Webhook-Signature|	String| Webフック設定時に入力した署名 |

[Request body]

|値|	タイプ|	説明|
|---|---|---|
|hooksId|	String| Webフック設定に定定義されたURLでPOSTリクエストをするたびに一意に作成されるID |
|webhookConfigId|	String|Webフック設定ID|
|productName|	String|	Webフックイベントが発生したサービス名 |
|appKey|	String| Webフックイベントが発生したサービスアプリケーションキー |
|event|	String| Webフックイベント名 |
|hooks|	List\<Map\> | Webフックイベント発生時のデータ<br>* 詳細は[イベントタイプ別フック(hook)定義](./webhook-api-guide/#event-hooks)を参照してください。 |

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

### イベントタイプ別hooks定義
Webフック設定に定義されたURLでPOSTリクエストを作成する時、イベントタイプ別フック(hook)データです。
#### テンプレート状態/お問い合わせのアップデート
|値|	タイプ|	説明|
|---|---|---|
|hooks|	List\<Map\> | Webフックイベント発生時のデータ |
|- hookId|	String| サービスでイベントが発生した時に作成される固有のID |
|- senderKey|	String|	発信キー |
|- templateCode|	String| テンプレートコード |
|- kakaoTemplateCode|	String| 原本テンプレートコード |
|- status|	String| テンプレート状態(TSC01:リクエスト、 TSC02:検収中、 TSC03:承認、 TSC04: 否認) |
|- comments|	List| 検収結果 |
|-- id|	String| お問い合わせID|
|-- content|	String|お問い合わせ内容 |
|-- userName|	String|作成者 |
|-- createdAt|	String|登録日 |
|-- attachment|	List|添付ファイル |
|--- originalFileName|	String|添付ファイル名 |
|--- filePath|	String|添付ファイルのパス |
|-- status|	String| コメント状態(INQ:お問い合わせ、 APR:承認、 REJ: 否認、REP: 回答) |
|- updateDate|	String| 修正日 |

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

#### メッセージ送信結果コードのアップデート
|値|	タイプ|	説明|
|---|---|---|
|hooks|	List\<Map\> | Webフックイベント発生時のデータ |
|- kakaoMessageType|	String| カカオメッセージタイプ<br>ALIMTALK_NORMAL<br>ALIMTALK_AUTH<br>ALIMTALK_MASS<br>FRIENDTALK_NORMAL<br>FRIENDTALK_MASS<br>BRAND_MESSAGE_NORMAL<br>BRAND_MESSAGE_MASS  |
|- requestId|	String| リクエストID |
|- recipientSeq|	Integer| 受信者シーケンス番号 |
|- requestDate|	String| リクエスト日 |
|- createDate|	String| 作成日 |
|- receiveDate|	String| 受信日 |
|- recipientNo|	String| 受信番号 |
|- resultCode|	String| 受信結果コード |
|- senderGroupingKey|	String| 発信グルーピングキー |
|- recipientGroupingKey|	String| 受信者グルーピングキー |
|- _links|	Object|	リンク |
|- self|	Object|	- |
|- href|	String|	メッセージ照会APIリンク |
|- hookId|	String| サービスでイベントが発生した時に作成される固有のID |

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
