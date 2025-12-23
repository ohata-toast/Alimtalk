## Notification > KakaoTalk Bizmessage > ブランドメッセージ > API v1.0 Guide

## ブランドメッセージ

#### [APIドメイン]

| ドメイン                                                                        |
|------------------------------------------------------------------------------|
| [https://api-alimtalk.cloud.toast.com](https://api-alimtalk.cloud.toast.com) |

## v1.0 API紹介

## 非友だちメッセージ送信(ターゲティングM、N)管理

非友だちメッセージ送信(ターゲティングM、N)は、以下の条件を全て満たす場合に送信できます。

- ビジネス認証チャンネル
  - 事業者番号の登録
  - チャンネルカスタマーセンターの電話番号登録
  - チャンネルの友だち数が5万以上
  - 3か月以内にお知らせトークの送信成功履歴を保有

### マーケティング受信同意の証明資料のアップロード

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request parameter]

| 名前 | タイプ | 必須 | 説明          |
|------|------|----|---------------|
| file | File | O | マーケティング受信同意の証明資料 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

### 非友だちメッセージ送信(ターゲティングM、N)の使用申請

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

## メッセージ自由形送信リクエスト

* マーケティング受信同意ユーザーへの送信を利用できます。
  * targetingフィールドを指定して、メッセージ対象のタイプを指定できます。
      * M: 顧客企業の広告性情報受信に同意したユーザー(カカオトークでの受信に同意)
      * N: 顧客企業の広告性情報受信に同意したユーザー(カカオトークでの受信に同意) - チャネルの友だち
      * I: 顧客企業の送信リクエスト対象 ∩ チャネルの友だち
* 従来のカカともへのメッセージの8つのメッセージタイプを全て使用できます。
* BC、BTボタンタイプを使用できます。
* AC(チャンネル追加)ボタンは使用できません。
* BFボタンを使用する場合、カカオから発行されたビジネスフォームIDを入力して使用できます。
* 代替送信は、受信者ごとにresendParameterを介して設定できます。
* 代替送信をご利用になる場合、代替送信管理APIを介してSMS Appkeyの登録及び送信設定が必要です。
* <b>夜間送信制限(20:50～翌日08:00)</b>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### テキスト型送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "TEXT",
  "pushAlarm": boolean,
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| content | String | O | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式の入力が可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29回、URL形式の入力が可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1回)<br>- PREMIUM_VIDEOタイプの場合、このフィールドをオプションとして使用可能。最大76文字(改行:最大1回)<br>- その他のタイプの場合、このフィールドは使用しません |
| buttons | List | X | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### 画像形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "IMAGE",
  "pushAlarm": boolean,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| content | String | O | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式の入力が可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29回、URL形式の入力が可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1回)<br>- PREMIUM_VIDEOタイプの場合、このフィールドをオプションとして使用可能。最大76文字(改行:最大1回)<br>- その他のタイプの場合、このフィールドは使用しません |
| image | Object | O | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド |
| - imageUrl | String | O | 画像URL。一般画像としてアップロードされた画像URLを使用 |
| - imageLink | String | X | 画像をクリックしたときに移動するURL。1000文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用 |
| buttons | List | X | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### ワイド画像形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE",
  "pushAlarm": boolean,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| content | String | O | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式の入力が可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29回、URL形式の入力が可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1回)<br>- PREMIUM_VIDEOタイプの場合、このフィールドをオプションとして使用可能。最大76文字(改行:最大1回)<br>- その他のタイプの場合、このフィールドは使用しません |
| image | Object | O | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド |
| - imageUrl | String | O | 画像URL、ワイド画像としてアップロードされた画像URLを使用 |
| - imageLink | String | X | 画像をクリックしたときに移動するURL。1000文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用 |
| buttons | List | X | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### ワイドアイテムリスト形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "pushAlarm": boolean,
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| header | String | O | ヘッダ<br>- WIDE_ITEM_LISTタイプの場合、必須フィールドで最大20文字(改行:不可)<br>- PREMIUM_VIDEOタイプの場合、任意フィールドで最大20文字(改行:不可) |
| item | Object | O | ワイドリスト要素(WIDE_ITEM_LISTタイプでのみ使用可能) |
| - list                 | List    | O  | ワイドリスト(最小: 3、最大4)                                                                                                                                                                                                                                                         |
| -- title | String | O | アイテムのタイトル<br>- 1番目のアイテムは最大25文字制限(改行:最大1回、1番目のアイテムの場合、titleは必須値ではありません)<br>- 2～4番目のアイテムは最大30文字制限(改行:最大1回) |
| -- imageUrl | String | O | アイテムの画像URL<br>- 1番目のアイテムには、最初のワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>- 2～4番目のアイテムは、一般ワイドアイテムリスト画像としてアップロードされた画像URLを使用 |
| -- linkMo              | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                          |
| -- schemeAndroid       | String  | X  | Androidアプリリンク、1,000文字制限                                                                                                                                                                                                                                                       |
| -- schemeIos           | String  | X  | iOSアプリリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| buttons | List | X | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### プレミアム動画形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "pushAlarm": boolean,
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| content | String | X | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式の入力が可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29回、URL形式の入力が可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1回)<br>- PREMIUM_VIDEOタイプの場合、このフィールドをオプションとして使用可能、最大76文字(改行:最大1回)<br>- その他のタイプの場合、このフィールドは使用しません |
| header | String | X | ヘッダ<br>- WIDE_ITEM_LISTタイプの場合、必須フィールドで最大20文字(改行:不可)<br>- PREMIUM_VIDEOタイプの場合、任意フィールドで最大20文字(改行:不可) |
| video | Object | O | 動画要素(PREMIUM_VIDEOタイプのみ使用可能) |
| - videoUrl | String | O | カカオTV動画URL (カカオTVにアップロードされた動画アドレスのみ使用可能)、最大500文字制限 |
| - thumbnailUrl | String | X | 動画サムネイル用画像URL、一般画像としてアップロードされたURLのみ使用可能(ない場合、カカオTV動画の基本サムネイルを使用) 、最大500文字制限 |
| buttons | List | X | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### コマース形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| additionalContent | String | X | 付加情報(最大34文字、改行:最大1回)、コマース形式でのみ使用可能 |
| image | Object | O | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド |
| - imageUrl | String | O | 画像URL。一般画像としてアップロードされた画像URLを使用 |
| - imageLink | String | X | 画像をクリックしたときに移動するURL。1000文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用 |
| commerce | Object | O | コマース(COMMERCEタイプでのみ使用可能) |
| title | String | O | 商品名(最大30文字、改行:不可) |
| regularPrice | Integer | O | 通常価格(0～99,999,999) |
| discountPrice | Integer | X | 割引価格(0～99,999,999) |
| discountRate | Integer | X | 割引率(0～100)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須 |
| discountFixed | Integer | X | 定額割引価格(0 ～ 999,999)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須 |
| buttons | List | O | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件 |
| - name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| - type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| - chatExtra | String | X | BC/BTタイプのボタンの場合、伝達するメタ情報 |
| - chatEvent | String | X | BTタイプのボタンの場合、連携するボットイベント名 |
| - bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| - title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| - description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字。改行:不可<br>- その他のタイプの場合、最大12文字。改行:不可 |
| - linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - linkPc | String | X | PCWebリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| - schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### カルーセルフィード形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "pushAlarm": boolean,
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー(40文字). グループ発信キーは使用不可                                                                                                                                                                                                                                                     |
| chatBubbleType         | String  | O  | メッセージタイプ(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm | boolean | X | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                  | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| carousel               | Object  | O  | カルーセル                                                                                                                                                                                                                                                                         |
| - list | List | O | カルーセルリスト(最小2件、最大6件) |
| -- header | String | O | カルーセルアイテムのタイトル(最大20文字)。カルーセルフィード形式でのみ使用可能 |
| -- message | String | O | カルーセルアイテムのタイトル(最大20文字)、カルーセルアイテムのメッセージ(最大180文字)。カルーセルフィード形式でのみ使用可能 |
| -- imageUrl | String | O | 画像URL(カルーセルフィード形式の画像としてアップロードされた画像のみ使用可能) |
| -- imageLink           | String  | O  | 画像リンク、1000文字制限                                                                                                                                                                                                                                                            |
| -- buttons | List | O | カルーセルリストのボタンリスト最小1件、最大2件 |
| --- name | String | O | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字 |
| --- type | String | O | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、BC:相談トーク転換、BT:チャットボット転換、BF:ビジネスフォーム)<br>- BCタイプは相談トークを利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには以下の3つのフレーズのみ使用可能<br> - トークで予約する<br> - トークでアンケートする<br> - トークで応募する |
| --- linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限 |
| --- linkPc | String | X | PC Webリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| --- schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| --- schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限 |
| --- chatExtra          | String  | X  | BC / BTタイプのボタンの場合、伝達するメタ情報                                                                                                                                                                                                                                                 |
| --- chatEvent          | String  | X  | BTタイプのボタンの場合、接続するボットイベント名                                                                                                                                                                                                                                                     |
| --- bizFormKey | String | X | BFタイプのボタンの場合、ビズフォームキー |
| -- coupon              | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| --- title | String | O | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| --- description | String | O | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可 |
| --- linkMo | String | X | モバイルWebリンク(WLタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| --- linkPc | String | X | PC Webリンク(WLタイプの場合、任意フィールド)、1,000文字制限 |
| --- schemeAndroid | String | X | Androidアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| --- schemeIos | String | X | iOSアプリリンク(ALタイプの場合、必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。 |
| - tail | Object | X | もっと見るボタン情報 |
| -- linkMo              | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                          |
| -- schemeAndroid       | String  | X  | Androidアプリリンク、1,000文字制限                                                                                                                                                                                                                                                       |
| -- schemeIos           | String  | X  | iOSアプリリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X | 送信に失敗した場合、メッセージを代替送信するかどうか<br>コンソールで代替送信を設定した場合、基本設定として代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録されている送信者番号ではない場合、代替送信に失敗することがあります。)</span> |
| -- resendUnsubscribeNo | String | X | 代替送信080受信拒否番号<br><span style="color:red">(SMSサービスに登録された080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X | 登録者(コンソールから送信した場合、ユーザーUUIDで保存) |
| statsId                | String  | 	X | 統計ID(発信検索条件には含まれません。最大8文字)                                                                                                                                                                                                                                            |

#### カルーセルコマース形式の送信リクエスト

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "pushAlarm": boolean,
  "adult": boolean,
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
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
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
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      }
    }
  ],
  "createUser": String,
  "statsId": String
}
```

| 名前                 | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                                          |
|----------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O  | 送信キー(40文字)、グループ送信キーは使用不可 |
| chatBubbleType | String | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X  | メッセージプッシュ通知の送信有無(デフォルト: true) |
| adult                | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                                                       |
| carousel             | Object  | O  | カルーセル                                                                                                                                                                                                                                                                         |
| - head               | Object  | X  | カルーセルイントロ                                                                                                                                                                                                                                                                     |
| -- header            | String  | O  | カルーセルイントロヘッダ(最大20文字)                                                                                                                                                                                                                                                            |
| -- content           | String  | O  | カルーセルイントロ内容(最大50文字)                                                                                                                                                                                                                                                            |
| -- imageUrl | String | O  | カルーセルイントロ画像アドレス(カルーセルコマース形式の画像としてアップロードされた画像を使用、使用される画像はカルーセルの画像と比率が同じである必要があります) |
| -- linkMo | String | X  | モバイルWebリンク(linkMo、linkPc、schemeAndroid、schemeIosのいずれかを使用する場合、linkMoは必須値)、1,000文字制限 |
| -- linkPc            | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| -- schemeAndroid     | String  | X  | Androidアプリリンク、1,000文字制限                                                                                                                                                                                                                                                       |
| -- schemeIos         | String  | X  | iOSアプリリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| - list | List | O  | カルーセルリスト(headが存在する場合、最小1件、最大5件 / それ以外は最小2件、最大6件) |
| -- additionalContent | String | X  | 付加情報(最大34文字)、カルーセルコマース形式でのみ使用可能 |
| -- imageUrl | String | O  | 画像URL (カルーセルコマース形式の画像としてアップロードされた画像を使用) |
| -- imageLink         | String  | O  | 画像リンク、1000文字制限                                                                                                                                                                                                                                                            |
| -- commerce | Object | O  | コマース(CAROUSEL_COMMERCEタイプでのみ使用可能) |
| --- title            | String  | O  | 商品名(最大30文字、改行:不可)                                                                                                                                                                                                                                                       |
| --- regularPrice | Integer | O  | 通常価格(0～99,999,999) |
| --- discountPrice | Integer | X  | 割引価格(0 ～ 99,999,999) |
| --- discountRate | Integer | X  | 割引率(0 ～ 100)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須 |
| --- discountFixed | Integer | X  | 定額割引価格(0 ～ 999,999)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須 |
| -- buttons           | List    | O  | カルーセルリストボタンリスト最小1件、最大2件                                                                                                                                                                                                                                                  |
| --- name | String | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合は最大14文字<br>- それ以外のタイプの場合は最大8文字 |
| --- type | String | O  | ボタンのタイプ(WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、BC：チャット相談への切り替え、BT：チャットボットへの切り替え、BF：ビジネスフォーム)<br>- BCタイプはチャット相談を利用するカカオトークチャンネルのみ利用可能<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには次の3つの文言のみ使用可能<br> - トークで予約する<br> - トークでアンケートに答える<br> - トークで応募する |
| --- linkMo | String | X  | モバイルWebリンク(WLタイプの場合は必須フィールド)、1,000文字制限 |
| --- linkPc | String | X  | PC Webリンク(WLタイプの場合は任意フィールド)、1,000文字制限 |
| --- schemeAndroid | String | X  | Androidアプリリンク(ALタイプの場合は必須フィールド)、1,000文字制限 |
| --- schemeIos | String | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、1,000文字制限 |
| --- chatExtra | String | X  | BC / BTタイプのボタンの場合に転送するメタ情報 |
| --- chatEvent | String | X  | BTタイプのボタンの場合に接続するボットのイベント名 |
| --- bizFormKey | String | X  | BFタイプのボタンの場合のビズフォームキー |
| -- coupon            | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                       |
| --- title | String | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」 |
| --- description | String | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合は最大18文字、改行：不可<br>- それ以外のタイプの場合は最大12文字、改行：不可 |
| --- linkMo | String | X  | モバイルWebリンク(WLタイプの場合は必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式：alimtalk=coupon://)を入力する場合、残りのフィールドが選択事項(オプション)になります。 |
| --- linkPc | String | X  | PC Webリンク(WLタイプの場合は任意フィールド)、1,000文字制限 |
| --- schemeAndroid | String | X  | Androidアプリリンク(ALタイプの場合は必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式：alimtalk=coupon://)を入力する場合、残りのフィールドが選択事項(オプション)になります。 |
| --- schemeIos | String | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式：alimtalk=coupon://)を入力する場合、残りのフィールドが選択事項(オプション)になります。 |
| - tail | Object | X  | もっと見るボタンの情報 |
| -- linkMo            | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| -- linkPc            | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                          |
| -- schemeAndroid     | String  | X  | Androidアプリリンク、1,000文字制限                                                                                                                                                                                                                                                       |
| -- schemeIos         | String  | X  | iOSアプリリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| recipientList        | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                                                             |
| - recipientNo        | String  | O  | 受信番号                                                                                                                                                                                                                                                                       |
| - resendParameter    | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                    |
| -- isResend | boolean | X  | 送信失敗時に、テキストメッセージで代替送信するかどうか<br>コンソールで代替送信を設定した場合、デフォルトで代替送信されます。 |
| -- resendType | String | X  | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレートの本文の長さに応じてタイプが区別されます。 |
| -- resendTitle | String | X  | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X  | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X  | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録された送信者番号でない場合、代替送信に失敗することがあります。)</span> |
| createUser | String | X  | 登録者(コンソールで送信時にユーザーUUIDとして保存) |
| statsId | String | X  | 統計ID(送信検索条件には含まれません、最大8文字) |

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
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| 名前             | タイプ    | Not Null | 説明                                                         |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | レスポンスヘッダ情報                                                   |
| - isSuccessful   | boolean | O        | API呼び出し成否                                               |
| - resultCode | Integer | O | API呼び出し結果コード(成功：200、失敗時はエラーコード) |
| - resultMessage | String | O | API呼び出し結果メッセージ(成功時は「success」または関連する成功メッセージ、失敗時は失敗原因の詳細メッセージ) |
| message | Object | X | メッセージ送信結果情報(送信リクエストがある場合にのみ存在) |
| - requestId | String | X | 送信リクエストID(各送信リクエストを一意に識別するID) |
| - sendResults | Array | O | 受信者別の送信結果リスト |
| -- recipientSeq | Integer | O | 受信者リストの順番 |
| -- recipientNo | String | X | 受信者の電話番号 |
| -- resultCode | Integer | O | 受信者別の送信結果コード(成功及び様々な失敗コードが存在する可能性があります) |
| -- resultMessage | String | O | 受信者別の送信結果メッセージ(成功時は「success」または関連メッセージ、失敗時は失敗原因の詳細メッセージ) |

## メッセージ基本形送信リクエスト

* テンプレートを利用した送信です。
* 広告やプロモーション情報を受け取ることに同意したユーザーに対してメッセージ送信を使用できます。
  * targetingフィールドを指定して、メッセージ対象のタイプを指定できます。
      * M: 顧客企業の広告性情報受信に同意したユーザー(カカオトークでの受信に同意)
      * N: 顧客企業の広告性情報受信に同意したユーザー(カカオトークでの受信に同意) - チャネルの友だち
      * I: 顧客企業の送信リクエスト対象 ∩ チャネルの友だち
* 既存カカともへのメッセージの8つのメッセージタイプを全て使用できます。
* BC、BTボタンタイプは使用できません。
* AC(チャンネル追加)ボタンを使用できます。
* BFボタンを使用する場合、カカオから発行されたビジネスフォームIDをアップロードしてビズフォームキーを発行して使用できます。
* 代替送信は、受信者ごとにresendParameterを介して設定できます。
* 代替送信をご利用になる場合、代替送信管理APIを介してSMS Appkeyの登録及び送信設定が必要です。
* <b>夜間送信制限(20:50～翌日08:00)</b>

### 使用時の注意事項

- unsubscribeNo、unsubscribeAuthNoは080無料受信拒否電話番号と認証番号で、どちらか一方でも入力しない場合は、送信プロフィールに登録された無料受信拒否情報で送信されます。
- 送信時にunsubscribeNo、unsubscribeAuthNoを入力すると、送信プロフィールに登録されている無料受信拒否情報ではなく、入力した値で送信されます。
- 送信時にunsubscribeNo、unsubscribeAuthNoを入力しない場合、送信プロフィールに登録されている無料受信拒否情報で送信されます。
- unsubscribeNo、unsubscribeAuthNoは受信者ごとに入力でき、共通フィールドと受信者ごとフィールドの両方を入力した場合、共通フィールドが優先的に適用されます。

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### 送信リクエスト

```
{
  "senderKey": String,
  "templateCode": String,
  "pushAlarm": boolean,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "recipientList": [
    {
      "recipientNo": String,
      "targeting": String,
      "templateParameter": Object,
      "imageParameters": List,
      "videoParameter": Object,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 名前                   | タイプ    | 必須 | 説明                                                                                                                         |
|------------------------|---------|----|------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | 送信キー(40文字)、グループ送信キーは使用不可 |
| templateCode | String | O | 使用するテンプレートコード |
| pushAlarm | boolean | X | メッセージプッシュ通知を送信するかどうか(デフォルト値：true) |
| requestDate | String | X | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信)<br>最大60日後まで予約可能 |
| unsubscribeNo | String | X | 080無料受信拒否電話番号(全て未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080無料受信拒否認証番号(全て未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>unsubscribe_phone_numberなしでunsubscribe_auth_numberのみの入力は不可<br>ex) 1234 |
| recipientList          | List    | O  | 受信者リスト(最大1,000人)                                                                                                            |
| - recipientNo          | String  | O  | 受信番号                                                                                                                      |
| - targeting | String | O | メッセージ対象のタイプ(M：マーケティング受信同意ユーザー、N：友だちではないマーケティング受信同意ユーザーにのみ、I：友だちであるユーザー) |
| - templateParameter | Object | X | テンプレートパラメータ(テンプレートに置換する変数を含む場合、必須) |
| - imageParameters | List | X | テンプレートの画像フィールド値を変更できる動的パラメータ(テンプレートに存在する画像の数と同じサイズのJSONリストのみ使用できます。使用する場合、変更しない画像は空のJSONオブジェクトを入力する必要があります) |
| -- imageUrl | String | O | 画像のURL |
| -- imageLink | String | X | 画像のリンク |
| - videoParameter | List | X | テンプレートのビデオフィールド値を変更できる動的パラメータ |
| -- videoUrl | String | O | カカオTVの動画URL |
| -- thumbnailUrl | String | X | 動画のサムネイル用画像のURL |
| - resendParameter   | Object  | X  | 代替送信情報                                                                                                                           |
| -- isResend | boolean | X | 送信失敗時に、テキストメッセージで代替送信するかどうか<br>コンソールで代替送信を設定した場合、デフォルトで代替送信されます。 |
| -- resendType | String | X | 代替送信タイプ(SMS、LMS)<br>値がない場合、テンプレートの本文の長さに応じてタイプが区別されます。 |
| -- resendTitle | String | X | LMS代替送信の件名<br>(値がない場合、プラスフレンドIDで代替送信されます。) |
| -- resendContent | String | X | 代替送信の内容<br>(値がない場合、[メッセージ本文]で代替送信されます。) |
| -- resendSendNo | String | X | 代替送信の送信者番号<br><span style="color:red">(SMSサービスに登録された送信者番号でない場合、代替送信に失敗することがあります。)</span> |
| unsubscribeNo | String | X | 080無料受信拒否電話番号(両方とも未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080無料受信拒否認証番号(両方とも未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>unsubscribe_phone_numberなしでunsubscribe_auth_numberのみの入力は不可<br>ex) 1234 |
| createUser | String | X | 登録者(コンソールで送信時にユーザーUUIDとして保存) |
| statsId | String | X | 統計ID(送信検索条件には含まれません、最大8文字) |

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
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| 名前             | タイプ    | Not Null | 説明                                                         |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | レスポンスヘッダ情報                                                   |
| - isSuccessful   | boolean | O        | API呼び出し成否                                               |
| - resultCode | Integer | O | API呼び出し結果コード(成功：200、失敗時はエラーコード) |
| - resultMessage | String | O | API呼び出し結果メッセージ(成功時は「success」または関連する成功メッセージ、失敗時は失敗原因の詳細メッセージ) |
| message | Object | X | メッセージ送信結果情報(送信リクエストがある場合にのみ存在) |
| - requestId | String | X | 送信リクエストID(各送信リクエストを一意に識別するID) |
| - sendResults | Array | O | 受信者別の送信結果リスト |
| -- recipientSeq | Integer | O | 受信者リストの順番 |
| -- recipientNo | String | X | 受信者の電話番号 |
| -- resultCode | Integer | O | 受信者別の送信結果コード(成功及び様々な失敗コードが存在する可能性があります) |
| -- resultMessage | String | O | 受信者別の送信結果メッセージ(成功時は「success」または関連メッセージ、失敗時は失敗原因の詳細メッセージ) |

## 送信リストの照会

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Query parameter] 1番or 2番の条件が必須

| 名前             | タイプ   | 必須      | 説明                             |
|------------------|--------|-----------|----------------------------------|
| requestId | String | 条件付き必須(1番) | リクエストID |
| startRequestDate | String | 条件付き必須(2番) | 送信リクエスト日付の開始値(yyyy-MM-dd HH:mm) |
| endRequestDate | String | 条件付き必須(2番) | 送信リクエスト日付の終了値(yyyy-MM-dd HH:mm) |
| senderKey        | String | X         | 発信キー                           |
| templateCode     | String | X         | テンプレートコード                         |
| recipientNo      | String | X         | 受信番号                          |
| messageStatus | String | X | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| resultCode       | String | X         | 送信結果(MRC01:成功MRC02:失敗)      |
| pageNum          | String | X         | ページ番号(Default: 1)               |
| pageSize | String | X | 照会件数(Default: 15、Max: 1000) |

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
        "targeting": String,
        "requestDate": String,
        "createDate": String,
        "receiveDate": String,
        "chatBubbleType": String,
        "pushAlarm": boolean,
        "messageStatus": String,
        "resendStatusCode": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "isAddedChannel": boolean,
        "resultCode": String,
        "resultCodeName": String,
        "createUser": String
      }
    ],
    "totalCount": Integer
  }
}
```

| 名前                        | タイプ    | Not Null | 説明                                                                  |
|:----------------------------|:--------|:---------|:-----------------------------------------------------------------------|
| header                      | Object  | O        | ヘッダ領域                                                               |
| - resultCode                | Integer | O        | 結果コード                                                               |
| - resultMessage             | String  | O        | 結果メッセージ                                                              |
| - isSuccessful              | boolean | O        | 成否                                                               |
| messageSearchResultResponse | Object  | X        | 本文領域                                                               |
| - messages                  | Array   | O        | メッセージリスト                                                             |
| -- requestId                | String  | O        | リクエストID                                                                 |
| -- recipientSeq             | Integer | O        | 受信者シーケンス番号                                                          |
| -- plusFriendId             | String  | O        | 送信プロフィールID                                                             |
| -- senderKey                | String  | O        | 発信キー                                                                |
| -- templateCode             | String  | X        | テンプレートコード                                                              |
| -- recipientNo              | String  | O        | 受信番号                                                               |
| -- targeting | String | O | メッセージ対象のタイプ(M - マーケティング受信同意ユーザー、N - 友だちではないマーケティング受信同意ユーザーにのみ、I - 友だちであるユーザー) |
| -- requestDate              | String  | O        | リクエスト日時                                                               |
| -- createDate               | String  | O        | 登録日時                                                               |
| -- receiveDate              | String  | X        | 受信日時                                                               |
| -- chatBubbleType           | String  | O        | メッセージタイプ                                                              |
| -- pushAlarm | boolean | O | プッシュ通知を使用するかどうか |
| -- messageStatus | String | O | リクエストステータス(COMPLETED：成功、FAILED：失敗) |
| -- resendStatusCode         | String  | X        | 代替送信ステータスコード                                                         |
| -- resendStatusName | String | X | 代替送信ステータス名 |
| -- resendResultCode         | String  | X        | 代替送信結果コード                                                         |
| -- resendRequestId          | String  | X        | 代替送信リクエストID                                                           |
| -- isAddedChannel | boolean | O | チャンネルの友達かどうか |
| -- resultCode               | String  | X        | 受信結果コード                                                            |
| -- resultCodeName           | String  | X        | 受信結果コード名                                                           |
| -- createUser | String | X | 登録者(コンソールで送信時にユーザーUUIDとして保存) |
| - totalCount | Integer | O | 合計数 |

## 送信単件照会

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前         | タイプ    | 説明       |
|--------------|---------|------------|
| appkey       | String  | 固有のアプリキー   |
| requestId    | String  | リクエストID      |
| recipientSeq | Integer | 受信者シーケンス番号 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

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
    "targeting": String,
    "requestDate": String,
    "createDate": String,
    "receiveDate": String,
    "chatBubbleType": String,
    "content": String,
    "adult": boolean,
    "header": String,
    "additionalContent": String,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "chatExtra": String,
        "chatEvent": String,
        "bizFormKey": String
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String
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
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
    "carousel": {
      "head": {
        "header": String,
        "content": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String
      },
      "list": [
        {
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String,
              "chatExtra": String,
              "chatEvent": String,
              "bizFormKey": String
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
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
    "templateParameter": String,
    "pushAlarm": boolean,
    "messageStatus": String,
    "isAddedChannel": boolean,
    "resultCode": String,
    "resultCodeName": String,
    "resendStatusCode": String,
    "resendStatusName": String,
    "resendResultCode": String,
    "resendRequestId": String,
    "createUser": String
  }
}
```

| 名前                  | タイプ    | Not Null | 説明                                                                                             | 
 |:----------------------|:--------|:---------|:-------------------------------------------------------------------------------------------------| 
| header                | Object  | O        | ヘッダ領域                                                                                          | 
| - resultCode          | Integer | O        | 結果コード                                                                                          | 
| - resultMessage       | String  | O        | 結果メッセージ                                                                                         | 
| - isSuccessful        | boolean | O        | 成否                                                                                          | 
| message | Object | X | メッセージ本文領域(メッセージ失敗時は存在しない場合があります) |
| - requestId | String | O | リクエストID(messageオブジェクトが存在する場合Not Null) |
| - recipientSeq | Integer | O | 受信者シーケンス番号(messageオブジェクトが存在する場合Not Null) |
| - plusFriendId | String | O | 送信プロフィールID(messageオブジェクトが存在する場合Not Null) |
| - senderKey | String | O | 送信キー(messageオブジェクトが存在する場合Not Null) |
| - templateCode        | String  | X        | テンプレートコード                                                                                         | 
| - recipientNo | String | O | 受信番号(messageオブジェクトが存在する場合Not Null) |
| - targeting | String | O | メッセージ対象のタイプ(M - マーケティング受信同意ユーザー、N - 友だちではないマーケティング受信同意ユーザーにのみ、I - 友だちであるユーザー)(messageオブジェクトが存在する場合Not Null) |
| - requestDate | String | O | リクエスト日時(messageオブジェクトが存在する場合Not Null) |
| - createDate | String | O | 登録日時(messageオブジェクトが存在する場合Not Null) |
| - receiveDate         | String  | X        | 受信日時                                                                                          | 
| - chatBubbleType      | String  | O        | メッセージタイプ(messageオブジェクトが存在する場合Not Null)                                                                | 
| - content | String | X | メッセージの内容 |
| - adult | boolean | O | 成人向けメッセージかどうか(messageオブジェクトが存在する場合Not Null) |
| - header              | String  | X        | ヘッダ(メッセージ内)                                                                                       | 
| - additionalContent   | String  | X        | 付加情報(メッセージ内)                                                                                    |
| - image | Object | X | 画像要素 |
| -- imageUrl | String | O | 画像のURL(imageオブジェクトが存在する場合Not Null) |
| -- imageLink | String | X | 画像のリンク |
| - buttons             | Array   | X        | ボタンリスト                                                                                          | 
| --- name | String | O | ボタンのタイトル(buttons配列の項目が存在する場合Not Null) |
| --- type | String | O | ボタンのタイプ(buttons配列の項目が存在する場合Not Null) |
| -- linkMo             | String  | X        | モバイルWebリンク                                                                                       | 
| -- linkPc             | String  | X        | PC Webリンク                                                                                        | 
| -- schemeIos          | String  | X        | iOSアプリリンク                                                                                       | 
| -- schemeAndroid      | String  | X        | Androidアプリリンク                                                                                     | 
| --- chatExtra | String | X | BC / BTタイプのボタンの場合に転送するメタ情報 |
| --- chatEvent | String | X | BTタイプのボタンの場合に接続するボットのイベント名 |
| --- bizFormKey | String | X | BFタイプのボタンの場合のビズフォームキー |
| - item                | Object  | X        | ワイドリスト要素                                                                                     | 
| -- list | Array | X | ワイドリスト(itemオブジェクトが存在する場合Nullable) |
| --- title             | String  | X        | アイテムのタイトル                                                                                         | 
| --- imageUrl | String | O | アイテム画像のURL(item.listの項目が存在する場合Not Null) |
| --- linkMo | String | O | モバイルWebリンク(item.listの項目が存在する場合Not Null) |
| --- linkPc            | String  | X        | PC Webリンク                                                                                        | 
| --- schemeIos         | String  | X        | iOSアプリリンク                                                                                       | 
| --- schemeAndroid     | String  | X        | Androidアプリリンク                                                                                     | 
| - coupon              | Object  | X        | クーポン要素                                                                                          | 
| -- title              | String  | O        | クーポンのタイトル(couponオブジェクトが存在する場合Not Null)                                                                  | 
| -- description | String | O | クーポンの詳細説明(couponオブジェクトが存在する場合Not Null) |
| -- linkMo             | String  | X        | モバイルWebリンク                                                                                       | 
| -- linkPc             | String  | X        | PC Webリンク                                                                                        | 
| -- schemeAndroid      | String  | X        | Androidアプリリンク                                                                                     | 
| -- schemeIos          | String  | X        | iOSアプリリンク                                                                                       | 
| - commerce            | Object  | X        | コマース要素                                                                                         | 
| -- title              | String  | O        | 商品名(commerceオブジェクトが存在する場合Not Null)                                                                | 
| -- regularPrice       | Integer | X        | 通常価格                                                                                          | 
| -- discountPrice      | Integer | X        | 割引価格                                                                                           | 
| -- discountRate       | Integer | X        | 割引率                                                                                            | 
| -- discountFixed      | Integer | X        | 定額割引価格                                                                                         | 
| - video               | Object  | X        | 動画要素                                                                                         | 
| -- videoUrl | String | O | カカオTVの動画URL(videoオブジェクトが存在する場合Not Null) |
| -- thumbnailUrl | String | X | 動画のサムネイル用画像のURL |
| - carousel            | Object  | X        | カルーセル                                                                                            | 
| -- head | Object | X | カルーセルのイントロ(carouselオブジェクトが存在する場合Nullable) |
| --- header | String | O | カルーセルのイントロヘッダ(headオブジェクトが存在する場合Not Null) |
| --- content | String | O | カルーセルのイントロ内容(headオブジェクトが存在する場合Not Null) |
| --- imageUrl | String | O | カルーセルのイントロ画像のURL(headオブジェクトが存在する場合Not Null) |
| --- linkMo            | String  | X        | モバイルWebリンク                                                                                       | 
| --- linkPc            | String  | X        | PC Webリンク                                                                                        | 
| --- schemeIos         | String  | X        | iOSアプリリンク                                                                                       | 
| --- schemeAndroid     | String  | X        | Androidアプリリンク                                                                                     | 
| -- list               | Array   | O        | カルーセルリスト(carouselオブジェクトが存在する場合Not Null)                                                              | 
| --- header | String | X | カルーセルアイテムのヘッダ |
| --- message | String | O | カルーセルアイテムのメッセージ(listの項目が存在する場合Not Null) |
| --- additionalContent | String  | X        | 付加情報                                                                                          | 
| --- imageUrl | String | X | 画像のURL |
| --- imageLink | String | X | 画像のリンク |
| --- commerce | Object | X | コマース(カルーセル内) |
| ---- title | String | O | 商品のタイトル(carousel.list.commerceが存在する場合Not Null) |
| ---- regularPrice     | Integer | X        | 通常価格                                                                                          | 
| ---- discountPrice    | Integer | X        | 割引価格                                                                                           | 
| ---- discountRate     | Integer | X        | 割引率                                                                                            | 
| ---- discountFixed    | Integer | X        | 定額割引価格                                                                                         | 
| --- buttons           | Array   | X        | ボタンリスト(カルーセル内)                                                                                    | 
| ---- name | String | O | ボタンのタイトル(carousel.list.buttonsの項目が存在する場合Not Null) |
| ---- type | String | O | ボタンのタイプ(carousel.list.buttonsの項目が存在する場合Not Null) |
| ---- linkMo           | String  | X        | モバイルWebリンク                                                                                       | 
| ---- linkPc           | String  | X        | PC Webリンク                                                                                        | 
| ---- schemeAndroid    | String  | X        | Androidアプリリンク                                                                                     | 
| ---- schemeIos        | String  | X        | iOSアプリリンク                                                                                       | 
| ---- chatExtra | String | X | BC / BTタイプのボタンの場合に転送するメタ情報 |
| ---- chatEvent | String | X | BTタイプのボタンの場合に接続するボットのイベント名 |
| ---- bizFormKey | String | X | BFタイプのボタンの場合のビズフォームキー |
| --- coupon | Object | X | クーポン(カルーセル内) |
| ---- title | String | O | クーポンの件名(carousel.list.couponが存在する場合Not Null) |
| ---- description | String | O | クーポンの詳細説明(carousel.list.couponが存在する場合Not Null) |
| ---- linkMo           | String  | X        | モバイルWebリンク                                                                                       | 
| ---- linkPc           | String  | X        | PC Webリンク                                                                                        | 
| ---- schemeAndroid    | String  | X        | Androidアプリリンク                                                                                     | 
| ---- schemeIos        | String  | X        | iOSアプリリンク                                                                                       | 
| -- tail | Object | X | もっと見るボタンの情報(carouselオブジェクトが存在する場合Nullable) |
| --- linkMo | String | O | モバイルWebリンク(tailオブジェクトが存在する場合Not Null) |
| --- linkPc            | String  | X        | PC Webリンク                                                                                        | 
| --- schemeAndroid     | String  | X        | Androidアプリリンク                                                                                     | 
| --- schemeIos         | String  | X        | iOSアプリリンク                                                                                       | 
| - templateParameter   | String  | X        | テンプレートパラメータ                                                                                       | 
| - pushAlarm | boolean | O | プッシュ通知を使用するかどうか(messageオブジェクトが存在する場合Not Null) |
| - messageStatus | String | O | リクエストステータス(COMPLETED：成功、FAILED：失敗)(messageオブジェクトが存在する場合Not Null) |
| - isAddedChannel | boolean | O | チャンネルの友だちかどうか(messageオブジェクトが存在する場合Not Null) |
| - resultCode          | String  | X        | 受信結果コード(メッセージ内)                                                                                 | 
| - resultCodeName      | String  | X        | 受信結果コード名(メッセージ内)                                                                                | 
| - resendStatusCode    | String  | X        | 代替送信ステータスコード                                                                                    | 
| - resendStatusName    | String  | X        | 代替送信ステータスコード名                                                                                   | 
| - resendResultCode    | String  | X        | 代替送信結果コード                                                                                    | 
| - resendRequestId     | String  | X        | 代替送信リクエストID                                                                                      | 
| - createUser          | String  | X        | 登録者(コンソールから送信した場合、ユーザーUUIDで保存)                                                                      |

## テンプレート管理

### テンプレートリスト照会

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Query parameter]

| 名前         | タイプ    | 必須 | 説明                          |
|--------------|---------|----|-------------------------------|
| templateCode | String  | X  | テンプレートコード                      |
| templateName | String  | X  | テンプレート名                      |
| status       | String  | X  | テンプレートステータスコード                   |
| pageNum      | Integer | X  | ページ番号(Default: 1)            |
| pageSize     | Integer | X  | 照会件数(Default: 15, Max: 1000) |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateListResponse": {
    "templates": [
      {
        "plusFriendId": String,
        "plusFriendType": String,
        "templateCode": String,
        "templateName": String,
        "chatBubbleType": String,
        "content": String,
        "header": String,
        "additionalContent": String,
        "adult": boolean,
        "image": {
          "imageUrl": String,
          "imageLink": String
        },
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer,
          }
        ],
        "item": {
          "list": [
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String
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
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
        "video": {
          "videoUrl": String,
          "thumbnailUrl": String
        },
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
              "header": String,
              "message": String,
              "additionalContent": String,
              "imageUrl": String,
              "imageLink": String,
              "commerce": {
                "title": String,
                "regularPrice": Integer,
                "discountPrice": Integer,
                "discountRate": Integer,
                "discountFixed": Integer
              },
              "buttons": [
                {
                  "name": String,
                  "type": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeIos": String,
                  "schemeAndroid": String,
                  "bizFormId": Integer,
                }
              ],
              "coupon": {
                "title": String,
                "description": String,
                "linkMo": String,
                "linkPc": String,
                "schemeAndroid": String,
                "schemeIos": String
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
        "status": String,
        "createDate": String,
        "updateDate": String
      }
    ],
    "totalCount": Integer
  }
}
```

| 名前                    | タイプ    | Not Null | 説明                                   |
|:------------------------|:--------|:---------|:---------------------------------------|
| header                  | Object  | O        | ヘッダ領域                                |
| - resultCode            | Integer | O        | 結果コード                                |
| - resultMessage         | String  | O        | 結果メッセージ                               |
| - isSuccessful          | boolean | O        | 成否                                |
| templateListResponse    | Object  | O        | 本文領域                                |
| - templates             | Array   | O        | テンプレートリスト                              |
| -- plusFriendId         | String  | O        | 送信プロフィールID                              |
| -- plusFriendType       | String  | O        | 送信プロフィールタイプ                            |
| -- templateCode         | String  | O        | テンプレートコード                               |
| -- templateName         | String  | O        | テンプレート名                                |
| -- chatBubbleType       | String  | O        | メッセージタイプ                               |
| -- content              | String  | X        | メッセージの内容                                |
| -- header               | String  | X        | ヘッダ                                   |
| -- additionalContent    | String  | X        | (説明テーブル参考)                            |
| -- adult                | boolean | O        | 成人向けメッセージかどうか                            |
| -- image                | Object  | X        | 画像情報                                |
| --- imageUrl            | String  | O        | 画像のURL                                |
| --- imageLink           | String  | X        | 画像のリンク                                |
| -- buttons              | Array   | X        | ボタンリスト                                |
| --- name                | String  | O        | ボタンのタイトル                                 |
| --- type                | String  | O        | ボタンタイプ                                |
| --- linkMo              | String  | X        | モバイルWebリンク                             |
| --- linkPc              | String  | X        | PC Webリンク                              |
| --- schemeIos           | String  | X        | iOSアプリリンク                             |
| --- schemeAndroid       | String  | X        | Androidアプリリンク                           |
| --- bizFormId           | Integer | X        | ビズフォームID (JSON基準)                       |
| -- item                 | Object  | X        | ワイドリスト要素                           |
| --- list                | Array   | X        | ワイドリスト                              |
| ---- title              | String  | X        | アイテムのタイトル                                |
| ---- imageUrl           | String  | O        | アイテム画像のURL                            |
| ---- linkMo             | String  | O        | モバイルWebリンク                             |
| ---- linkPc             | String  | X        | PC Webリンク                              |
| ---- schemeAndroid      | String  | X        | Androidアプリリンク                           |
| ---- schemeIos          | String  | X        | iOSアプリリンク                             |
| -- coupon               | Object  | X        | クーポン要素                                |
| --- title               | String  | O        | クーポン名                                |
| --- description         | String  | O        | クーポンの詳細説明                              |
| --- linkMo              | String  | X        | モバイルWebリンク                             |
| --- linkPc              | String  | X        | PC Webリンク                              |
| --- schemeAndroid       | String  | X        | Androidアプリリンク                           |
| --- schemeIos           | String  | X        | iOSアプリリンク                             |
| -- commerce             | Object  | X        | コマース要素                               |
| --- title               | String  | O        | 商品名                                 |
| --- regularPrice        | Integer | X        | 通常価格                                |
| --- discountPrice       | Integer | X        | 割引価格                                 |
| --- discountRate        | Integer | X        | 割引率                                  |
| --- discountFixed       | Integer | X        | 定額割引価格                               |
| -- video                | Object  | X        | 動画要素                               |
| --- videoUrl            | String  | O        | カカオTVの動画URL                          |
| --- thumbnailUrl        | String  | X        | 動画のサムネイル用画像のURL                       |
| -- carousel             | Object  | X        | カルーセル                                  |
| --- head                | Object  | X        | カルーセルイントロ                              |
| ---- header             | String  | O        | カルーセルイントロヘッダ                           |
| ---- content            | String  | O        | カルーセルイントロ内容                           |
| ---- imageUrl           | String  | O        | カルーセルイントロ画像アドレス                        |
| ---- linkMo             | String  | X        | モバイルWebリンク                             |
| ---- linkPc             | String  | X        | PC Webリンク                              |
| ----- schemeAndroid     | String  | X        | Androidアプリリンク                           |
| ----- schemeIos         | String  | X        | iOSアプリリンク                             |
| ---- list               | Array   | O        | カルーセルリスト                              |
| ----- header            | String  | O        | カルーセルアイテムヘッダ                           |
| ----- message           | String  | O        | カルーセルアイテムメッセージ(Not Nullリストのcontentマッピング) |
| ----- additionalContent | String  | X        | 付加情報                                |
| ----- imageUrl          | String  | O        | 画像URL (カルーセルアイテム内)                    |
| ----- imageLink         | String  | X        | 画像リンク(カルーセルアイテム内)                     |
| ----- commerce          | Object  | O        | コマース(カルーセルアイテム内)                        |
| ------ title            | String  | O        | 商品名                                 |
| ------ regularPrice     | Integer | X        | 通常価格                                |
| ------ discountPrice    | Integer | X        | 割引価格                                 |
| ------ discountRate     | Integer | X        | 割引率                                  |
| ------ discountFixed    | Integer | X        | 定額割引価格                               |
| ----- buttons           | Array   | O        | カルーセルリストのボタン一覧                         |
| ------ name             | String  | O        | ボタンのタイトル                                 |
| ------ type             | String  | O        | ボタンタイプ                                |
| ------ linkMo           | String  | X        | モバイルWebリンク                             |
| ------ linkPc           | String  | X        | PC Webリンク                              |
| ------ schemeIos        | String  | X        | iOSアプリリンク                             |
| ------ schemeAndroid    | String  | X        | Androidアプリリンク                           |
| ------ bizFormId        | Integer | X        | ビズフォームID (JSON基準)                       |
| ----- coupon            | Object  | X        | クーポン要素(カルーセルアイテム内)                      |
| ------ title            | String  | X        | クーポン名                                 |
| ------ description      | String  | X        | クーポンの詳細説明                              |
| ------ linkMo           | String  | X        | モバイルWebリンク                             |
| ------ linkPc           | String  | X        | PC Webリンク                              |
| ------ schemeAndroid    | String  | X        | Androidアプリリンク                           |
| ------ schemeIos        | String  | X        | iOSアプリリンク                             |
| ---- tail               | Object  | X        | もっと見るボタン情報                             |
| ----- linkMo            | String  | O        | モバイルWebリンク                             |
| ----- linkPc            | String  | X        | PC Webリンク                              |
| ----- schemeAndroid     | String  | X        | Androidアプリリンク                           |
| ----- schemeIos         | String  | X        | iOSアプリリンク                             |
| --- status              | String  | O        | テンプレートの状態(A:登録、 S:ブロック)                   |
| --- createDate          | String  | O        | 登録日時                                |
| --- updateDate          | String  | X        | 修正日時                                |
| - totalCount            | Integer | O        | 合計数                                  |

### テンプレートの個別照会

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前         | タイプ   | 説明   |
|--------------|--------|--------|
| appkey       | String | 固有のアプリキー |
| senderKey    | String | 発信キー |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "plusFriendId": String,
    "plusFriendType": String,
    "templateCode": String,
    "templateName": String,
    "chatBubbleType": String,
    "content": String,
    "header": String,
    "additionalContent": String,
    "adult": boolean,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "bizFormId": Integer
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
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
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
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
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
              "bizFormId": Integer
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
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
    "status": String,
    "createDate": String,
    "updateDate": String
  }
}
```

| 名前                  | タイプ    | Not Null | 説明                 |
|:----------------------|:--------|:---------|:---------------------|
| header                | Object  | O        | ヘッダ領域              |
| - resultCode          | Integer | O        | 結果コード              |
| - resultMessage       | String  | O        | 結果メッセージ             |
| - isSuccessful        | boolean | O        | 成否              |
| template              | Object  | O        | テンプレート本文領域          |
| - plusFriendId        | String  | O        | 送信プロフィールID            |
| - plusFriendType      | String  | O        | 送信プロフィールタイプ          |
| - templateCode        | String  | O        | テンプレートコード             |
| - templateName        | String  | O        | テンプレート名              |
| - chatBubbleType      | String  | O        | メッセージタイプ             |
| - pushAlarm           | boolean | X        | プッシュ通知の有無            |
| - content             | String  | X        | メッセージの内容              |
| - adult               | boolean | O        | 成人向けメッセージかどうか          |
| - header              | String  | X        | ヘッダ(テンプレート内)           |
| - additionalContent   | String  | X        | 付加情報(テンプレート内)        |
| - image               | Object  | X        | 画像情報              |
| -- imageUrl           | String  | O        | 画像のURL              |
| -- imageLink          | String  | X        | 画像のリンク              |
| - buttons             | Array   | X        | ボタンリスト              |
| -- name               | String  | O        | ボタンのタイトル               |
| -- type               | String  | O        | ボタンタイプ              |
| -- linkMo             | String  | X        | モバイルWebリンク           |
| -- linkPc             | String  | X        | PC Webリンク            |
| -- schemeIos          | String  | X        | iOSアプリリンク           |
| -- schemeAndroid      | String  | X        | Androidアプリリンク         |
| -- bizFormId          | Integer | X        | ビズフォームID (JSON基準)     |
| - item                | Object  | X        | ワイドリスト要素         |
| -- list               | Array   | X        | ワイドリスト            |
| --- title             | String  | X        | アイテムのタイトル              |
| --- imageUrl          | String  | O        | アイテム画像のURL          |
| --- linkMo            | String  | O        | モバイルWebリンク           |
| --- linkPc            | String  | X        | PC Webリンク            |
| --- schemeIos         | String  | X        | iOSアプリリンク           |
| --- schemeAndroid     | String  | X        | Androidアプリリンク         |
| - coupon              | Object  | X        | クーポン要素              |
| -- title              | String  | O        | クーポン名               |
| -- description        | String  | O        | クーポンの詳細説明            |
| -- linkMo             | String  | X        | モバイルWebリンク           |
| -- linkPc             | String  | X        | PC Webリンク            |
| -- schemeIos          | String  | X        | iOSアプリリンク           |
| -- schemeAndroid      | String  | X        | Androidアプリリンク         |
| - commerce            | Object  | X        | コマース要素             |
| -- title              | String  | O        | 商品名              |
| -- regularPrice       | Integer | X        | 通常価格              |
| -- discountPrice      | Integer | X        | 割引価格               |
| -- discountRate       | Integer | X        | 割引率                |
| -- discountFixed      | Integer | X        | 定額割引価格             |
| - video               | Object  | X        | 動画要素             |
| -- videoUrl           | String  | O        | カカオTVの動画URL        |
| -- thumbnailUrl       | String  | X        | 動画のサムネイル用画像のURL     |
| - carousel            | Object  | X        | カルーセル                |
| -- head               | Object  | X        | カルーセルイントロ            |
| --- header            | String  | O        | カルーセルイントロヘッダ         |
| --- content           | String  | O        | カルーセルイントロ内容         |
| --- imageUrl          | String  | O        | カルーセルイントロ画像アドレス      |
| --- linkMo            | String  | X        | モバイルWebリンク           |
| --- linkPc            | String  | X        | PC Webリンク            |
| --- schemeIos         | String  | X        | iOSアプリリンク           |
| --- schemeAndroid     | String  | X        | Androidアプリリンク         |
| -- list               | Array   | O        | カルーセルリスト            |
| --- header            | String  | O        | カルーセルアイテムヘッダ         |
| --- message           | String  | O        | カルーセルアイテムメッセージ        |
| --- additionalContent | String  | X        | 付加情報(カルーセルアイテム内)    |
| --- imageUrl          | String  | O        | 画像URL (カルーセルアイテム内)  |
| --- imageLink         | String  | X        | 画像リンク(カルーセルアイテム内)   |
| --- commerce          | Object  | O        | コマース(カルーセルアイテム内)      |
| ---- title            | String  | O        | 商品名              |
| ---- regularPrice     | Integer | X        | 通常価格              |
| ---- discountPrice    | Integer | X        | 割引価格               |
| ---- discountRate     | Integer | X        | 割引率                |
| ---- discountFixed    | Integer | X        | 定額割引価格             |
| --- buttons           | Array   | O        | カルーセルリストのボタン一覧       |
| ---- name             | String  | O        | ボタンのタイトル               |
| ---- type             | String  | O        | ボタンタイプ              |
| ---- linkMo           | String  | X        | モバイルWebリンク           |
| ---- linkPc           | String  | X        | PC Webリンク            |
| ---- schemeIos        | String  | X        | iOSアプリリンク           |
| ---- schemeAndroid    | String  | X        | Androidアプリリンク         |
| ---- bizFormId        | Integer | X        | ビズフォームID (JSON基準)     |
| --- coupon            | Object  | X        | クーポン要素(カルーセルアイテム内)    |
| ---- title            | String  | X        | クーポン名               |
| ---- description      | String  | X        | クーポンの詳細説明            |
| ---- linkMo           | String  | X        | モバイルWebリンク           |
| ---- linkPc           | String  | X        | PC Webリンク            |
| ---- schemeIos        | String  | X        | iOSアプリリンク           |
| ---- schemeAndroid    | String  | X        | Androidアプリリンク         |
| -- tail               | Object  | X        | もっと見るボタン情報           |
| --- linkMo            | String  | O        | モバイルWebリンク           |
| --- linkPc            | String  | X        | PC Webリンク            |
| --- schemeIos         | String  | X        | iOSアプリリンク           |
| --- schemeAndroid     | String  | X        | Androidアプリリンク         |
| - status              | String  | O        | テンプレートの状態(A:登録、 S:ブロック) |
| - createDate          | String  | O        | 登録日時              |
| - updateDate          | String  | X        | 修正日時              |

### テンプレート登録

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### 注意事項

* クーポン名にプレースホルダーを適用する場合、次のような固定プレースホルダーを使用する必要があります。

```
- #{割引金額}KRW割引クーポン(#{割引金額}の範囲は1 ～ 99,999,999)
- #{割引率}%割引クーポン(#{割引率}の範囲は1 ～ 100)
- 送料割引クーポン
- #{商品名}無料クーポン(#{商品名}は最大7文字)
- #{商品名} UPクーポン(#{商品名}は最大7文字)
```

* コマースで商品名を除いたregularPrice、 discountPrice、 discountRate、 discountFixedフィールドには、ユーザーがプレースホルダーを指定することはできません。
    * 商品名を除く全てのフィールドの値を空にした場合、自動的に固定プレースホルダーが入力されて保存されます。
    * regularPrice -> #{通常価格}
    * discountPrice -> #{割引価格}
    * discountRate -> #{割引率}
    * discountFixed -> #{定額割引価格}

#### テキスト型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "TEXT",
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前            | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                |
|-----------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式入力可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29個、URL形式入力可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1個)<br>- PREMIUM_VIDEOタイプの場合、該当フィールドをオプションとして使用可能、最大76文字(改行:最大1個)<br>- その他のタイプの場合、該当フィールドは使用しません |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                                      |
| - name          | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                                           |
| - type          | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります                  |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                            |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - bizFormKey    | String  | X  | BFタイプのボタンの場合、ビズフォームキー<br>プレースホルダー使用不可                                                                                                                                                                                                                   |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                             |
| - title         | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                  |
| - description   | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                                           |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                       |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |

#### 画像型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "IMAGE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前            | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式入力可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29個、URL形式入力可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1個)<br>- PREMIUM_VIDEOタイプの場合、該当フィールドをオプションとして使用可能、最大76文字(改行:最大1個)<br>- その他のタイプの場合、該当フィールドは使用しません |
| image           | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド                                                                                                                                                                                                     |
| - imageUrl      | String  | O  | 画像URL、一般画像としてアップロードされた画像URLを使用<br>プレースホルダー使用不可                                                                                                                                                                                                     |
| - imageLink     | String  | X  | 画像をクリックした際に移動するURL、500文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用<br>プレースホルダー使用不可                                                                                                                                                                                   |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                                      |
| - name          | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                                           |
| - type          | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります                  |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                            |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - bizFormKey    | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                                                 |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                             |
| - title         | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                  |
| - description   | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                                           |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                       |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |

#### ワイド画像型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前            | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式入力可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29個、URL形式入力可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1個)<br>- PREMIUM_VIDEOタイプの場合、該当フィールドをオプションとして使用可能、最大76文字(改行:最大1個)<br>- その他のタイプの場合、該当フィールドは使用しません |
| image           | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド                                                                                                                                                                                                     |
| - imageUrl      | String  | O  | 画像URL、ワイド画像としてアップロードされた画像URLを使用<br>プレースホルダー使用不可                                                                                                                                                                                                    |
| - imageLink     | String  | X  | 画像をクリックした際に移動するURL、500文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用<br>プレースホルダー使用不可                                                                                                                                                                                   |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                                      |
| - name          | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                                           |
| - type          | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります                  |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                            |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - bizFormKey    | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                                                 |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                             |
| - title         | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                  |
| - description   | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                                           |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                       |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |

#### ワイドアイテムリスト型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前             | タイプ    | 必須 | 説明                                                                                                                                                                                                                              |
|------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName     | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                   |
| chatBubbleType   | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                             |
| adult            | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                           |
| header           | String  | O  | ヘッダ<br>- WIDE_ITEM_LISTタイプの場合、必須フィールドで最大20文字(改行:不可)<br>- PREMIUM_VIDEOタイプの場合、任意フィールドで最大20文字(改行:不可)                                                                                                                         |
| item             | Object  | O  | ワイドリスト要素(WIDE_ITEM_LISTタイプでのみ使用可能)                                                                                                                                                                                           |
| - list           | List    | O  | ワイドリスト(最小: 3、最大4)                                                                                                                                                                                                             |
| -- title         | String  | O  | アイテムのタイトル<br>- 1番目のアイテムは最大25文字に制限(改行:最大1個、1番目のアイテムの場合、titleは必須値ではありません)<br>- 1番目のアイテムはtitleが必須フィールドではありません<br>- 2～4番目のアイテムは最大30文字に制限(改行:最大1個)                                                                                     |
| -- imageUrl      | String  | O  | アイテム画像URL<br>- 1番目のアイテムには、最初のワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>- 2～4番目のアイテムは、一般のワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>プレースホルダー使用不可                                                                                                  |
| -- linkMo        | String  | O  | モバイルWebリンク、500文字制限                                                                                                                                                                                                             |
| -- linkPc        | String  | X  | PC Webリンク、500文字制限                                                                                                                                                                                                              |
| -- schemeAndroid | String  | X  | Androidアプリリンク、500文字制限                                                                                                                                                                                                           |
| -- schemeIos     | String  | X  | iOSアプリリンク、500文字制限                                                                                                                                                                                                             |
| buttons          | List    | X  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                    |
| - name           | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                         |
| - type           | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります |
| - linkMo         | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                            |
| - linkPc         | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| - schemeAndroid  | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                          |
| - schemeIos      | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                            |
| - bizFormKey     | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                               |
| coupon           | Object  | X  | クーポン要素                                                                                                                                                                                                                           |
| - title          | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                |
| - description    | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                         |
| - linkMo         | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| - linkPc         | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| - schemeAndroid  | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                     |
| - schemeIos      | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |

#### プレミアム動画型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前            | タイプ    | 必須 | 説明                                                                                                                                                                                                                                                |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                                             |
| content         | String  | X  | - TEXTタイプの場合、最大1,300文字(改行:最大99個、URL形式の入力が可能)<br>- IMAGEタイプの場合、最大400文字(改行:最大29回、URL形式の入力が可能)<br>- WIDEタイプの場合、最大76文字(改行:最大1回)<br>- PREMIUM_VIDEOタイプの場合、このフィールドをオプションとして使用可能、最大76文字(改行:最大1回)<br>- その他のタイプの場合、このフィールドは使用しません |
| header          | String  | X  | ヘッダ<br>- WIDE_ITEM_LISTタイプの場合、必須フィールドで最大20文字(改行:不可)<br>- PREMIUM_VIDEOタイプの場合、任意フィールドで最大20文字(改行:不可)                                                                                                                                           |
| video           | Object  | O  | 動画要素(PREMIUM_VIDEOタイプのみ使用可能)                                                                                                                                                                                                                    |
| - videoUrl      | String  | O  | カカオTV動画URL (カカオTVにアップロードされた動画アドレスのみ使用可能)、最大500文字制限<br>プレースホルダー使用不可                                                                                                                                                                                |
| - thumbnailUrl  | String  | X  | 動画サムネイル用画像URL。一般画像としてアップロードされたURLのみ使用可能(ない場合はカカオTV動画の基本サムネイルを使用)、最大500文字制限<br>プレースホルダー使用不可                                                                                                                                                    |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                                      |
| - name          | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                                           |
| - type          | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります                  |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                                            |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                                              |
| - bizFormKey    | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                                                 |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                             |
| - title         | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                  |
| - description   | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                                           |
| - linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |
| - linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                                               |
| - schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                       |
| - schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                                         |

#### コマース型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 名前              | タイプ    | 必須 | 説明                                                                                                                                                                                                                              |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                             |
| adult             | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                           |
| additionalContent | String  | X  | 付加情報(最大34文字、改行:最大1回)、コマース形式でのみ使用可能                                                                                                                                                                                         |
| image             | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCEタイプの場合、必須フィールド                                                                                                                                                                                   |
| - imageUrl        | String  | O  | 画像URL、一般画像としてアップロードされた画像URLを使用<br>プレースホルダー使用不可                                                                                                                                                                                    |
| - imageLink       | String  | X  | 画像をクリックした際に移動するURL、1000文字制限<br>未設定の場合、カカオトーク内の画像ビューアを使用<br>プレースホルダー使用不可                                                                                                                                                                 |
| commerce          | Object  | O  | コマース(COMMERCEタイプでのみ使用可能)                                                                                                                                                                                                        |
| title             | String  | O  | 商品名(最大30文字、改行:不可)                                                                                                                                                                                                           |
| regularPrice      | Integer | O  | 通常価格(0 ～ 99,999,999)<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{通常価格}`で保存されます                                                                                                                                                         |
| discountPrice     | Integer | X  | 割引価格(0 ～ 99,999,999)<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{割引価格}`で保存されます                                                                                                                                                           |
| discountRate      | Integer | X  | 割引率(0 ～ 100)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{割引率}`で保存されます                                                                                                                                     |
| discountFixed     | Integer | X  | 定額割引価格(0 ～ 999,999)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{定額割引価格}`で保存されます                                                                                                                           |
| buttons           | List    | O  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4件、それ以外は最大5件<br>- WIDE、WIDE_ITEM_LISTタイプの場合、最大2件<br>- PREMIUM_VIDEOタイプの場合、最大1件<br>- COMMERCEタイプの場合、最小1件、最大2件                                                                    |
| - name            | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダー使用不可                                                                                                                                                         |
| - type            | String  | O  | ボタンタイプ(WL: Webリンク、 AL:アプリリンク、 BK:ボットキーワード、 MD:メッセージ伝達、 AC:チャンネル追加、 BC:トーク相談転換、 BT:チャットボット転換、 BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプはTEXT、IMAGEの場合、最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります |
| - linkMo          | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                            |
| - linkPc          | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| - schemeAndroid   | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                          |
| - schemeIos       | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                            |
| - bizFormKey      | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                               |
| coupon            | Object  | X  | クーポン要素                                                                                                                                                                                                                           |
| - title           | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                |
| - description     | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                         |
| - linkMo          | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| - linkPc          | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| - schemeAndroid   | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                     |
| - schemeIos       | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |

#### カルーセルフィード型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| 名前              | タイプ    | 必須 | 説明                                                                                                                                                                                                                              |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                             |
| pushAlarm         | boolean | X  | メッセージプッシュ通知の送信有無(デフォルト: true)                                                                                                                                                                                                       |
| adult             | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                           |
| carousel          | Object  | O  | カルーセル                                                                                                                                                                                                                             |
| - list            | List    | O  | カルーセルリスト(最小2件、最大6件)                                                                                                                                                                                                            |
| -- header         | String  | O  | カルーセルアイテムのタイトル(最大20文字)、カルーセルフィード型でのみ使用可能                                                                                                                                                                                             |
| -- message        | String  | O  | カルーセルアイテムのタイトル(最大20文字)、カルーセルアイテムのメッセージ(最大180文字)、カルーセルフィード型でのみ使用可能。                                                                                                                                                                        |
| -- imageUrl       | String  | O  | 画像URL (カルーセルフィード型の画像としてアップロードされた画像のみ使用可能)<br>プレースホルダーは使用不可。                                                                                                                                                                              |
| -- imageLink      | String  | O  | 画像リンク、1000文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                    |
| -- buttons        | List    | O  | カルーセルリストのボタンリスト最小1件、最大2件                                                                                                                                                                                                       |
| --- name          | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダーは使用不可。                                                                                                                                                          |
| --- type          | String  | O  | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、AC:チャネル追加、BC:トーク相談切り替え、BT:チャットボット切り替え、BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプは、TEXT、IMAGEの場合は最初のボタンとして、それ以外のメッセージタイプの場合は最後のボタンとして登録する必要があります。 |
| --- linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                            |
| --- linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| --- schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                          |
| --- schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                            |
| --- bizFormKey    | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                               |
| -- coupon         | Object  | X  | クーポン要素                                                                                                                                                                                                                           |
| --- title         | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                |
| --- description   | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                         |
| --- linkMo        | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| --- linkPc        | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| --- schemeAndroid | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                     |
| --- schemeIos     | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| - tail            | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                        |
| -- linkMo         | String  | O  | モバイルWebリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                 |
| -- linkPc         | String  | X  | PC Webリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                  |
| -- schemeAndroid  | String  | X  | Androidアプリリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                               |
| -- schemeIos      | String  | X  | iOSアプリリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                 |
| recipientList     | List    | O  | 受信者リスト(最大1,000人)                                                                                                                                                                                                                 |
| - recipientNo     | String  | O  | 受信番号                                                                                                                                                                                                                           |
| createUser        | String  | X  | 登録者(コンソールから送信した場合、ユーザーUUIDで保存)                                                                                                                                                                                                       |

#### カルーセルコマース型テンプレート登録リクエスト

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "adult": boolean,
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
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
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
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| 名前                 | タイプ    | 必須 | 説明                                                                                                                                                                                                                              |
|----------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName         | String  | O  | テンプレート名(最大200文字)                                                                                                                                                                                                                   |
| chatBubbleType       | String  | O  | メッセージタイプ(TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE)                                                                                                                             |
| pushAlarm            | boolean | X  | メッセージプッシュ通知の送信有無(デフォルト: true)                                                                                                                                                                                                       |
| adult                | boolean | X  | 成人向けメッセージの有無(デフォルト値：false)                                                                                                                                                                                                           |
| carousel             | Object  | O  | カルーセル                                                                                                                                                                                                                             |
| - head               | Object  | X  | カルーセルイントロ                                                                                                                                                                                                                         |
| -- header            | String  | O  | カルーセルイントロヘッダ(最大20文字)                                                                                                                                                                                                                |
| -- content           | String  | O  | カルーセルイントロ内容(最大50文字)                                                                                                                                                                                                                |
| -- imageUrl          | String  | O  | カルーセルイントロ画像のURL(カルーセルコマース型の画像としてアップロードされた画像を使用、使用する画像はカルーセルの画像と比率が同一である必要があります)<br>プレースホルダーは使用不可。                                                                                                                                          |
| -- linkMo            | String  | X  | モバイルWebリンク(linkMo、linkPc、schemeAndroid、schemeIosのいずれかを使用する場合、linkMoは必須値)、500文字制限                                                                                                                                       |
| -- linkPc            | String  | X  | PC Webリンク、500文字制限                                                                                                                                                                                                             |
| -- schemeAndroid     | String  | X  | Androidアプリリンク、500文字制限                                                                                                                                                                                                           |
| -- schemeIos         | String  | X  | iOSアプリリンク、500文字制限                                                                                                                                                                                                             |
| - list               | List    | O  | カルーセルリスト(headが存在する場合、最小1件、最大5件 / それ以外は最小2件、最大6件)                                                                                                                                                                          |
| -- additionalContent | String  | X  | 付加情報(最大34文字)、カルーセルコマース形式でのみ使用可能                                                                                                                                                                                                 |
| -- imageUrl          | String  | O  | 画像URL (カルーセルコマース型の画像としてアップロードされた画像を使用)<br>プレースホルダーは使用不可。                                                                                                                                                                                 |
| -- imageLink         | String  | O  | 画像リンク、1000文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                    |
| -- commerce          | Object  | O  | コマース(CAROUSEL_COMMERCEタイプでのみ使用可能)                                                                                                                                                                                               |
| --- title            | String  | O  | 商品名(最大30文字、改行:不可)                                                                                                                                                                                                           |
| --- regularPrice     | Integer | O  | 通常価格(0～99,999,999)<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{通常価格}`で保存されます                                                                                                                                                         |
| --- discountPrice    | Integer | X  | 割引価格(0 ～ 99,999,999)<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{割引価格}`で保存されます                                                                                                                                                           |
| --- discountRate     | Integer | X  | 割引率(0 ～ 100)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{割引率}`で保存されます                                                                                                                                     |
| --- discountFixed    | Integer | X  | 定額割引価格(0 ～ 999,999)、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>プレースホルダーのユーザー指定は不可能、値を空にすると固定プレースホルダー`#{定額割引価格}`で保存されます                                                                                                                           |
| -- buttons           | List    | O  | カルーセルリストのボタンリスト最小1件、最大2件                                                                                                                                                                                                       |
| --- name             | String  | O  | ボタンのタイトル<br>- TEXT、IMAGEタイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>プレースホルダーは使用不可。                                                                                                                                                          |
| --- type             | String  | O  | ボタンタイプ(WL: Webリンク、AL:アプリリンク、BK:ボットキーワード、MD:メッセージ伝達、AC:チャネル追加、BC:トーク相談切り替え、BT:チャットボット切り替え、BF:ビジネスフォーム)<br>- テンプレートではBCタイプは利用できません <br>- BTタイプ <br>- テンプレートではBFタイプは利用できません<br>- ACタイプは、TEXT、IMAGEの場合は最初のボタンとして、それ以外のメッセージタイプの場合は最後のボタンとして登録する必要があります。 |
| --- linkMo           | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                            |
| --- linkPc           | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| --- schemeAndroid    | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限                                                                                                                                                                                          |
| --- schemeIos        | String  | X  | iOSアプリリンク(ALタイプの場合は必須フィールド)、500文字制限                                                                                                                                                                                            |
| --- bizFormKey       | String  | X  | BFタイプのボタンの場合、ビズフォームキー                                                                                                                                                                                                               |
| -- coupon            | Object  | X  | クーポン要素                                                                                                                                                                                                                           |
| --- title            | String  | O  | titleは5つの形式に制限されます<br>- 「${数字}KRW割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                |
| --- description      | String  | O  | クーポンの詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合、最大18文字、改行:不可<br>- その他のタイプの場合、最大12文字、改行:不可                                                                                                                         |
| --- linkMo           | String  | X  | モバイルWebリンク(WLタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| --- linkPc           | String  | X  | PC Webリンク(WLタイプの場合、任意フィールド)、500文字制限                                                                                                                                                                                             |
| --- schemeAndroid    | String  | X  | Androidアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                     |
| --- schemeIos        | String  | X  | iOSアプリリンク(ALタイプの場合、必須フィールド)、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは任意項目(オプション)となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL(形式: alimtalk=coupon://)を入力する場合、残りのフィールドが任意項目(オプション)になります。                                       |
| - tail               | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                        |
| -- linkMo            | String  | O  | モバイルWebリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                 |
| -- linkPc            | String  | X  | PC Webリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                  |
| -- schemeAndroid     | String  | X  | Androidアプリリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                               |
| -- schemeIos         | String  | X  | iOSアプリリンク、500文字制限<br>プレースホルダーは使用不可。                                                                                                                                                                                                 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "templateCode": String
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |
| template        | Object  | X        | テンプレート情報 |
| - templateCode  | String  | O        | テンプレートコード |

### テンプレート修正

#### リクエスト

[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前         | タイプ   | 説明   |
|--------------|--------|--------|
| appkey       | String | 固有のアプリキー |
| senderKey    | String | 発信キー |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request Body]

* テンプレート登録と仕様は同じです

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

### テンプレート削除

#### リクエスト

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前         | タイプ   | 説明   |
|--------------|--------|--------|
| appkey       | String | 固有のアプリキー |
| senderKey    | String | 発信キー |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

## 画像管理

### 画像アップロード

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request parameter]

| 名前      | タイプ   | 必須 | 説明                                                                                                                           |
|-----------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| image     | File   | O  | 画像                                                                                                                           |
| imageType | String | O  | 画像タイプ <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |

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
      "imageName": String,
  }
}
```

| 名前            | タイプ    | Not Null | 説明    |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |
| image           | Object  | X        | 画像領域 |
| - imageSeq      | Integer | O        | 画像シーケンス |
| - imageUrl      | String  | O        | 画像のURL |
| - imageName     | String  | X        | 画像名   |

#### アップロード画像規格
| 画像タイプ                      | 使用先                                                 | アップロード画像規格                                                                                                         |
| :------------------------- | :-------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- |
| IMAGE                      | 画像形式の送信リクエスト、コマース形式の送信リクエスト、プレミアム動画形式の送信リクエストのサムネイル | **推奨サイズ：**800 × 400px（横500px以上）<br/>**画像比率：**0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>**ファイル形式／容量制限：**jpg、png／最大5MB                 |
| WIDE_IMAGE                 | ワイド画像形式の送信リクエスト                                     | **推奨サイズ：**800 × 600px（横500px以上）<br/>**画像比率：**0.5 ≤ 縦 ÷ 横 ≤ 1<br/>**ファイル形式／容量制限：**jpg、png／最大5MB                     |
| MAIN_WIDE_ITEMLIST_IMAGE   | ワイドアイテムリスト形式の送信リクエストの1番目アイテム画像                      | **制限サイズ：**横500px以上<br/>**画像比率：**縦 ÷ 横 = 0.5<br/>**ファイル形式／容量制限：**jpg、png／最大5MB                                      |
| NORMAL_WIDE_ITEMLIST_IMAGE | ワイドアイテムリスト形式の送信リクエストの2〜4番目アイテム画像                    | **制限サイズ：**横500px以上<br/>**画像比率：**縦 ÷ 横 = 1<br/>**ファイル形式／容量制限：**jpg、png／各ファイル最大5MB                                   |
| CAROUSEL_FEED_IMAGE        | カルーセルフィード形式の送信リクエストのセル別画像                           | **推奨サイズ：**800 × 600px または 800 × 400px（横500px以上）<br/>**画像比率：**0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>**ファイル形式／容量制限：**jpg、png／最大5MB |
| CAROUSEL_COMMERCE_IMAGE    | コマース形式の送信リクエスト（カルーセルコマース形式）のイントロ画像、セル別画像            | **推奨サイズ：**800 × 600px または 800 × 400px（横500px以上）<br/>**画像比率：**0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>**ファイル形式／容量制限：**jpg、png／最大5MB |


### 画像照会

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request parameter]

| 名前       | タイプ   | 必須 | 説明                                                                                                                           |
|------------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| imageTypes | List   | O  | 画像タイプ <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |
| pageNum    | String | X  | ページ番号(基本: 1)                                                                                                                  |
| pageSize   | String | X  | 照会件数(基本: 15)                                                                                                                  |

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

| 名前            | タイプ    | Not Null | 説明    |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |
| image           | Object  | X        | 画像領域 |
| - imageSeq      | Integer | O        | 画像シーケンス |
| - imageUrl      | String  | O        | 画像のURL |
| - imageName     | String  | X        | 画像名   |

### 画像削除

#### リクエスト

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | タイプ   | 説明   |
|--------|--------|--------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Query parameter]

| 名前     | タイプ   | 必須 | 説明   |
|----------|--------|----|--------|
| imageSeq | String | O  | 画像番号 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

## アップロード

### ビズフォームキーのアップロード

#### リクエスト

[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

## 送信プロフィール管理

### 送信プロフィール照会

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

#### レスポンス

```
{
  "header":{
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "sender":{
    "plusFriendId" : String,
    "senderKey" : String,
    "categoryCode" : String,
    "unsubscribePhoneNumber": String,
    "unsubscribeAuthNumber": String,
    "status" : String,
    "statusName" : String,
    "kakaoStatus" : String,
    "kakaoStatusName" : String,
    "kakaoProfileStatus" : String,
    "kakaoProfileStatusName" : String,
    "profileSpamLevel" : String,
    "profileMessageSpamLevel" : String,
    "brandMessage" : {
      "resendAppKey": String,
      "isResend": boolean,
      "resendSendNo": String,
      "resendUnsubscribeNo": String
    },
    "dormant" : boolean,
    "marketingAgreement" : boolean,
    "block" : boolean,
    "createDate" : String,
    "initialUserRestriction" : boolean
  }
}
```

| 名前                      | タイプ    | Not Null | 説明                                                                                                                  |
|:--------------------------|:--------|:---------|:----------------------------------------------------------------------------------------------------------------------|
| header                    | Object  | O        | ヘッダ領域                                                                                                               |
| - resultCode              | Integer | O        | 結果コード                                                                                                               |
| - resultMessage           | String  | O        | 結果メッセージ                                                                                                              |
| - isSuccessful            | boolean | O        | 成否                                                                                                               |
| sender                    | Object  | X        | 送信プロフィール                                                                                                               |
| - plusFriendId            | String  | O        | プラスフレンドID                                                                                                              |
| - senderKey               | String  | O        | 発信キー                                                                                                                |
| - categoryCode            | String  | X        | カテゴリーコード                                                                                                              |
| - unsubscribePhoneNumber  | String  | X        | 無料受信拒否電話番号                                                                                                          |
| - unsubscribeAuthNumber   | String  | X        | 無料受信拒否認証番号                                                                                                          |
| - status                  | String  | X        | NHN Cloudプラスフレンドステータスコード <br>(YSC02:登録待機中、YSC03:正常登録)                                                              |
| - statusName              | String  | X        | NHN Cloudプラスフレンド状態名(登録待機中、正常登録)                                                                                   |
| - kakaoStatus             | String  | X        | カカオプラスフレンドステータスコード<br>(A:正常、S:ブロック)<br>statusがYSC02の場合、kakaoStatusはnull値になります。                                     |
| - kakaoStatusName         | String  | X        | カカオプラスフレンドステータス名(正常、ブロック)<br>statusがYSC02の場合、kakaoStatusNameはnull値になります。                                             |
| - kakaoProfileStatus      | String  | X        | カカオプラスフレンドプロフィールステータスコード<br>(A:有効、B:ブロック、C:無効、D:削除、E:削除処理中)<br>statusがYSC02の場合、kakaoProfileStatusはnull値になります。 |
| - kakaoProfileStatusName  | String  | X        | カカオプラスフレンドプロフィールステータス名(有効、無効、ブロック、削除処理中、削除)<br>statusがYSC02の場合、kakaoProfileStatusNameはnull値になります。              |
| - profileSpamLevel        | String  | X        | カカオトークチャネルのスパムステータス名(永久制限、警告制限、正常)<br>送信プロフィールのステータスが正常でない場合、null値になることがあります。                                           |
| - profileMessageSpamLevel | String  | X        | カカオトークメッセージのスパムステータス名(活動制限、警告制限、正常)<br>送信プロフィールのステータスが正常でない場合、null値になることがあります。                                          |
| - block                   | boolean | O        | 送信プロフィールのブロック有無                                                                                                         |
| - brandMessage            | Object  | X        | ブランドメッセージ設定情報                                                                                                        |
| -- resendAppKey           | String  | X        | 代替送信として設定するSMSサービスのアプリキー                                                                                               |
| -- isResend               | boolean | O        | 代替送信設定(再送信)の有無                                                                                                     |
| -- resendSendNo           | String  | X        | 再送信時、tc-smsの送信番号                                                                                                  |
| -- resendUnsubscribeNo    | String  | X        | 再送信時、tc-smsの080受信拒否番号                                                                                           |
| - dormant                 | boolean | O        | 送信プロフィールの休眠状態の有無                                                                                                         |
| - marketingAgreement      | boolean | O        | M/Nタイプの利用申請の有無                                                                                                      |
| - createDate              | String  | X        | 登録日                                                                                                                |
| - initialUserRestriction  | boolean | O        | 初回ユーザー制限の有無                                                                                                         |

### 送信プロフィールの080受信拒否番号の修正

#### リクエスト

[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前      | タイプ   | 説明   |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | タイプ   | 必須 | 説明             |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| 名前              | 	タイプ   | 	必須 | 	説明                                                                                                                          |
|-------------------|---------|-----|--------------------------------------------------------------------------------------------------------------------------------|
| unsubscribeNo     | 	String | 	O  | 080無料受信拒否電話番号(両方とも未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx  |
| unsubscribeAuthNo | 	String | 	X  | 080無料受信拒否認証番号(両方とも未入力の場合、送信プロフィールに登録された無料受信拒否情報で送信されます)<br>unsubscribe_phone_numberなしでunsubscribe_auth_numberのみの入力は不可<br>ex) 1234 |

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |

## 代替送信管理

### SMS AppKeyの登録

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | 	タイプ   | 	説明   |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | 	タイプ   | 	必須 | 	説明            |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request body]

```
{
    "resendAppKey": String
}
```

| 名前         | 	タイプ   | 	必須 | 	説明                  |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | 代替送信として設定するSMSサービスのアプリキー |

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

#### レスポンス

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

### 代替送信設定の登録

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前   | 	タイプ   | 	説明   |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前         | 	タイプ   | 	必須 | 	説明            |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 名前                | 	タイプ    | 	必須 | 	説明                                                                                                      |
|---------------------|----------|-----|------------------------------------------------------------------------------------------------------------|
| senderKey           | 	String  | 	O  | 発信キー                                                                                                     |
| isResend            | 	Boolean | 	O  | 送信に失敗した場合、SMSでの代替送信の有無<br>Consoleで代替送信を設定した場合、デフォルトで代替送信されます。                                           |
| resendSendNo        | 	String  | 	X  | 代替送信の送信番号<br><span style="color:red">(SMS商品に登録されている送信番号ではない場合、代替送信に失敗することがあります。)</span>                 |
| resendUnsubscribeNo | 	String  | 	X  | 代替送信の080受信拒否番号<br><span style="color:red">(SMSサービスに登録されている080受信拒否番号ではない場合、代替送信に失敗することがあります。)</span> |

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前            | タイプ    | Not Null | 説明   |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダ領域 |
| - resultCode    | Integer | O        | 結果コード |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成否 |
