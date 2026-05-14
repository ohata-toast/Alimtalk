## Notification > KakaoTalk Bizmessage > エラーコード

## APIレスポンスコード

| カテゴリー     | 成否 | 結果コード | 結果コードメッセージ                                                                                                                                          | APIレスポンスメッセージ                                                                                                                                                                                                                                                                                                                                                                |
|-----------|-------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 共通       | true  | 0     | 成功                                                                                                                                                 | SUCCESS                                                                                                                                                                                                                                                                                                                                                                    |
| 共通       | false | 4     | パラメータ有効性検証失敗(resultMessage参考)                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                            |
| 共通 | false | -1000 | 無効なアプリキー | Invalid appKey. |
| 共通 | false | -1001 | 無効な秘密鍵(secretKey) | Invalid secretKey. |
| 共通 | false | -1002 | 無効なSMSアプリキー | Invalid Sms appkey |
| 共通 | false | -1003 | 無効なSMS発信番号 | Invalid Sms Sendno |
| 共通 | false | -1005 | 10分間、同じX-NC-API-IDEMPOTENCY-KEY値でメッセージ送信をリクエストした場合 | The same 'X-NC-API-IDEMPOTENCY-KEY' was used within the last 10 minute. |
| 共通 | false | -1006 | 発信プロフィールが発信キーを持っていない場合 | Plus friend don't have senderKey. |
| 共通 | false | -1019 | 無効な代替送信080受信拒否番号 | Invalid Sms UnSubscribeno |
| 共通 | false | -1020 | 無効なUUID | Invalid uuid |
| 共通 | false | -1027 | 発信プロフィールがブロック状態の場合 | The sender is blocked status. |
| 共通 | false | -1028 | テンプレート変数が置換される際、その差が14文字を超える部分がある場合(初回ユーザー制限) | Blacklist can't use more than 14 characters in template value. |
| 共通 | false | -2000 | 特定のフィールド値が最大長を超過する場合 | The '{}' must be at less than or equal to {}. |
| 共通 | false | -2001 | 特定のフィールド値が必須値で空の場合 | The '{}' must not be blank. |
| 共通 | false | -2002 | 特定のフィールド値がNULLの場合 | The '{}' must not be null. |
| 共通 | false | -2003 | 特定のフィールド値が最小長より短い場合 | The '{}' must be greater than or equal to {}. |
| 共通 | false | -2004 | 特定のフィールド値が最大長より大きい場合 | The '{}' must be between {} and {}. |
| 共通 | false | -2005 | 特定のフィールド値が制限された範囲内の値ではない場合 | The '{}' must be less than or equal to {}. |
| 共通 | false | -2017 | 存在しない発信プロフィール | Not exist plus friend. |
| 共通 | false | -2018 | 無効なボタンパラメータ | Button parameter is invalid. |
| 共通 | false | -2033 | テンプレートアイテムパラメータが無効な場合 | TemplateItem parameter is invalid. |
| 共通 | false | -2034 | テンプレートアイテムハイライトパラメータが無効な場合 | TemplateItemHighlight parameter is invalid. |
| 共通 | false | -2036 | TemplateRepresentLinkパラメータが無効な場合 | TemplateRepresentLink parameter is invalid. |
| 共通 | false | -2037 | アイテムリストを含むテンプレート登録時、メッセージ本文の文字数が700文字を超過した場合 | The 'content' is too long. If you request the 'templateItem', It must be less than 1000 characters. |
| 共通 | false | -2502 | 送信失敗設定がされておらず、代替送信をリクエストした場合 | Please set up plus-friend resend setting. |
| 共通 | false | -2504 | 無効なクイックリプライ | The quickReplies parameter is invalid. |
| 共通 | false | -3000 | Webリンク(WL)ボタンタイプのテンプレートの場合、linkMoは必須フィールド | If template have a WL(webLink), must input linkMo |
| 共通 | false | -3002 | 必要なリクエスト本文(request body)の内容を読み取れない場合 | Field is not valid. |
| 共通 | false | -3003 | 存在しないテンプレート | Template does not exist. |
| 共通       | false | -3004 | テンプレートパラメータエラー                                                                                                                                        | Please check template parameter.                                                                                                                                                                                                                                                                                                                                           |
| 共通 | false | -3005 | テンプレート状態エラー(承認前の送信リクエスト時) | Please check template status. |
| 共通 | false | -3006 | linkMo/linkPC は必ず http:// または https:// を含む必要があります。 | The linkMo/linkPc must include http:// or https:// |
| 共通 | false | -3007 | アプリリンク(AL)は schemeAndroid、schemeIos、linkMo のうち少なくとも2つ以上入力する必要があります。 | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, linkMo must be entered. |
| 共通 | false | -3008 | ボタン名には置換パラメータを入力できません。 | The button name can't include the replacement parameter. |
| 共通       | false | -3009 | ボタン名が存在しません。                                                                                                                                     | Not exist button name.                                                                                                                                                                                                                                                                                                                                                     |
| 共通       | false | -3010 | 登録したテンプレート本文と一致しない場合                                                                                                                             | The contents are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                                           |
| 共通 | false | -3026 | ACタイプボタンの名前は「チャンネル追加」のみ登録可能                                                                                                                      | AC type button name must haveチャンネル追加                                                                                                                                                                                                                                                                                                                                       |
| 共通 | false | -3038 | templateItem summaryのtitleは置換パラメータを持つことができません | The templateItem's summary title can't include the replacement parameter. |
| 共通 | false | -3040 | サムネイルがあるアイテムハイライトのtitleは最大21文字、descriptionは最大13文字 | The itemHighlight's title with thumbnail can not exceed 21 characters, and description can not excced 13 characters |
| 共通 | false | -3041 | imageUrlは http:// または https:// を必ず含む必要があります。 | The imageUrl must include http:// or https:// |
| 共通 | false | -3051 | TNボタンの名前は「電話接続」、「カスタマーセンター接続」、「オペレーター接続」のいずれかである必要があります。                                                                                                  | TN button name must be one of:電話接続、カスタマーセンター接続、オペレーター接続                                                                                                                                                                                                                                                                                                                     |
| 共通       | false | -3052 | TNボタンにはtelNumberフィールドが必須です。                                                                                                                        | TN button must have telNumber field.                                                                                                                                                                                                                                                                                                                                       |
| 共通 | false | -3053 | 無効な電話番号形式です。数字とハイフンのみ使用可能で、最大14文字まで入力できます。 | Invalid telephone number format. Must be digits/hyphens only, max 14 characters. |
| 共通 | false | -3101 | クイックリプライ名が存在しない場合 | Not exist quickReply name. |
| 共通 | false | -3102 | クイックリプライ名には置換パラメータを含めることができません | The quickReply name can't include the replacement parameter. |
| 共通 | false | -3303 | 必須入力値が漏れています。 | {} is empty. |
| 共通 | false | -3304 | 入力値が有効な範囲を超えました。指定された範囲内の値を入力してください。 | The '{}' must be between {} and {}. |
| 共通 | false | -3305 | 選択したchatBubbleTypeでは特定のフィールドを使用できません。 | The chatBubbleType {} is not allowed {} field. |
| 共通 | false | -3306 | コマースタイプメッセージには必ずボタンを含める必要があります。 | The commerce type require buttons. |
| 共通 | false | -3307 | linkTypeの必須入力項目が漏れています。 | linkType {} is required field {} |
| 共通 | false | -3308 | linkTypeの複数の必須入力項目が漏れています。 | linkType {} is required fields {} |
| 共通 | false | -3309 | テンプレートにアプリリンク(AL)を使用する場合、schemeAndroid、schemeIos、linkMo のうち少なくとも2つを入力する必要があります。 | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, linkMo must be entered. |
| 共通 | false | -3310 | WebリンクURL(linkMo/linkPc)は 'http://' または 'https://' を含む必要があります。 | The linkMo/linkPc must include http:// or https:// |
| 共通 | false | -3311 | メッセージタイプがテキストまたは画像の場合、チャンネル追加(AC)ボタンは最初のボタンである必要があります                                                                                               | When the type is TEXT or IMAGE, AC  button type must be the first button.                                                                                                                                                                                                                                                                                                  |
| 共通 | false | -3312 | メッセージタイプがテキストまたは画像ではない場合、チャンネル追加(AC)ボタンは最後のボタンである必要があります。 | When the type is not TEXT or IMAGE, AC button type must be the last button. |
| 共通 | false | -3313 | クーポンURL情報が正しくありません。基本クーポン使用時はlinkMoフィールドが、チャンネルクーポンURL使用時はschemeAndroidまたはschemeIosフィールドが必要です。 | If a basic coupon is used instead of a channel coupon URL, the linkMo field is required. If a channel coupon URL (format: alimtalk=coupon://) is used, either schemeAndroid or schemeIos must be provided. |
| 共通 | false | -3314 | クーポン情報入力時、必須項目が漏れています。 | The coupon field require {} |
| 共通 | false | -3315 | クーポンタイトルが無効です。 | The title is not valid coupon title |
| 共通 | false | -3316 | コマース情報入力時、必須項目が漏れています。 | commerce field require {} |
| 共通 | false | -3317 | 動画情報入力時、必須項目が漏れています。 | video field require {} |
| 共通 | false | -3318 | 動画URLの形式が無効です。 | The videoUrl is not valid video url |
| 共通 | false | -3319 | 画像情報入力時、必須項目が漏れています。 | image field require {} |
| 共通 | false | -3320 | カルーセルアイテムの個数が許可された範囲(最小/最大値)を超えました。 | The carousel list size must be between {} and {} |
| 共通 | false | -3321 | カルーセルヘッダにリンクを入力する場合、linkMoは必須です。 | If at least one link is entered in the Carousel Head, linkMo cannot be empty. |
| 共通 | false | -3322 | カルーセルヘッダ情報入力時、必須項目が漏れています。 | The carousel head field require {} |
| 共通 | false | -3323 | カルーセルフッター情報入力時、必須項目が漏れています。 | The carousel tail field require {} |
| 共通       | false | -3324 | テンプレート変数値が正しくありません。                                                                                                                                   | The template parameter is invalid.                                                                                                                                                                                                                                                                                                                                         |
| 共通 | false | -3325 | アイテム(item)情報入力時、必須項目が漏れています。 | item field require {} |
| 共通 | false | -3326 | 画像URLは 'http://' または 'https://' を含む必要があります。 | The imageUrl must include http:// or https:// |
| 共通 | false | -3327 | 画像リンクURLは 'http://' または 'https://' を含む必要があります。 | The imageLink must include http:// or https:// |
| 共通 | false | -3328 | 許可されていないボタンタイプです。 | The button type {} is not allowed. |
| 共通 | false | -3330 | チャンネル追加ボタンがない縦並びボタン配列の場合、BFボタンは1番目でなければなりません。 | BF button must be the first button in vertical layout without channel-add button. |
| 共通 | false | -3331 | 許可されていないボタン名です。 | The button is not allowed to have {} as its name. |
| 共通 | false | -3332 | カルーセルコマースアイテム情報入力時、必須項目が漏れています。 | The carousel commerce item field require {} |
| 共通 | false | -3333 | カルーセルコマースアイテムに許可されていない情報が含まれています。 | The carousel commerce field not allowed {} |
| 共通 | false | -3334 | カルーセル一般アイテム情報入力時、必須項目が漏れています。 | The carousel item field require {} |
| 共通 | false | -3335 | カルーセル一般アイテムに許可されていない情報が含まれています。 | The carousel item field not allowed {} |
| 共通 | false | -3336 | チャンネル追加ボタンがある縦並びボタン配列の場合、BFボタンは2番目でなければなりません。 | BF button must be the second button in vertical layout with channel-add button. |
| 共通 | false | -3337 | チャンネル追加ボタンがない横並びボタン配列の場合、BFボタンは右側(2番目)でなければなりません。 | BF button must be the second (right side) button in horizontal layout without channel-add button. |
| 共通 | false | -3338 | チャンネル追加ボタンがある横並びボタン配列の場合、BFボタンは左側(1番目)でなければなりません。 | BF button must be the first (left side) button in horizontal layout with channel-add button. |
| 共通 | false | -3340 | 無効な080受信拒否番号 | Invalid unsubscribeNo format. Expected format: 080-xxx-xxxx or 080-xxxx-xxxx or 080xxxxxxx or 080xxxxxxxx. |
| 共通 | false | -3341 | 無効な080認証番号 | Invalid format. Only digits are allowed, up to a maximum of 9. |
| 共通       | false | -3347 | テンプレートパラメータのキーに'@'文字は使用不可                                                                                                                     | Template parameter key cannot contain '@' character. Invalid key: {}                                                                                                                                                                                                                                                                                                       |
| 共通       | false | -3348 | ACタイプのボタン名は「チャンネル追加」である必要がある                                                                                                                           | AC type button name must be 'チャンネル追加'.                                                                                                                                                                                                                                                                                                                                    |
| 共通       | false | -3349 | カルーセル全体でACボタンは1つのみ使用可能                                                                                                                        | AC button can only be used once across all carousel items.                                                                                                                                                                                                                                                                                                                  |
| 共通 | false | -4000 | 無効なパラメータ | Invalid parameter |
| 共通 | false | -4003 | 照会範囲が1か月を超過 | Search is possible within 31 days. |
| 共通       | false | -4004 | 存在しないアプリキー                                                                                                                                         | Not exist appkey.                                                                                                                                                                                                                                                                                                                                                          |
| 共通       | false | -4005 | 使用終了状態のアプリキー                                                                                                                                       | Appkey is disabled status.                                                                                                                                                                                                                                                                                                                                                 |
| 共通       | false | -4007 | ファイルサイズ超過                                                                                                                                          | The file size is less than {}.                                                                                                                                                                                                                                                                                                                                             |
| 共通 | false | -4009 | 無効なファイル拡張子 | Check the file extension. |
| 共通 | false | -4010 | ファイルが見つかりません | Not found file. |
| 共通 | false | -4011 | 受信者リストが見つかりません | Not found recipientList. |
| 共通 | false | -4014 | ファイルに recipient_no ヘッダがありません | There is no recipient_no header in the file. |
| 共通 | false | -4016 | データが存在しません | Not exist data. |
| 共通       | false | -4018 | ファイルアップロードエラー                                                                                                                                          | Upload attach file error.                                                                                                                                                                                                                                                                                                                                                  |
| 共通 | false | -4020 | ファイル読み込み失敗 | Failed to reading the file. |
| 共通 | false | -4023 | 商品を無効化するには、全ての発信者を削除する必要があります | For disabling the product, have to delete all senders |
| 共通 | false | -4101 | 無効な統計照会パラメータ | Invalid search parameter. |
| 共通 | false | -4103 | 無効なリクエストIDまたはリクエスト開始日時/リクエスト終了日時 | RequestId or startRequestDate/endRequestDate is invalid. |
| 共通 | false | -5000 | 無効な受信番号 | RecipientNo is invalid. |
| 共通       | false | -7000 | ベンダーリクエストAPI失敗                                                                                                                                       | Vender request API is failed.                                                                                                                                                                                                                                                                                                                                              |
| 共通 | false | -8001 | 画像ファイルが正常ではない場合 | The image you upload is invalid. |
| 共通 | false | -8002 | 画像が見つからない場合。<br/>カルーセルフィード - カルーセルタイプ画像を使用<br/> カルーセルコマース - コマースタイプ画像を使用 <br/> コマースタイプ - 画像タイプを使用 | It is not found any images. If you want to send carousel-feed type messages, you must use carousel type images. If you want to send carousel-commerce type messages, you must use commerce type images. If you want to send commerce type messages, you must use IMAGE type images. |
| 共通 | false | -8007 | ストレージ設定が空です | The storage configs can't empty. |
| 共通       | false | -8009 | すでに共有された プロジェクト                                                                                                                                        | This project has already been shared.                                                                                                                                                                                                                                                                                                                                      |
| 共通 | false | -8010 | サーバーの問題により画像ファイルアップロード失敗 | Uploading image file has failed for an unexpected error. |
| 共通 | false | -9992 | フェードアウトしたカカともへのメッセージ APIを呼び出した場合 | The API is no longer supported. Please migrate to the new brand-message endpoint/API. |
| 共通 | false | -9993 | 必須リクエスト部分が漏れています | Required request part is not present. |
| 共通 | false | -9994 | メソッド引数のタイプが予想と異なります | A method argument has not the expected type. |
| 共通 | false | -9996 | Content-typeがapplication/jsonではない場合 | Only application/json Content-type is supported. |
| 共通 | false | -9997 | 不適切なリクエストによる失敗 | Client Error. |
| 共通       | false | -9998 | 存在しないAPI                                                                                                                                         | Not exist API                                                                                                                                                                                                                                                                                                                                                              |
| 共通       | false | -9999 | システムエラー                                                                                                                                             | System error. Please inquire at support@toast.com.                                                                                                                                                                                                                                                                                                                         |
| 発信プロフィール | false | -3342 | マーケティング受信同意証跡資料アップロード失敗 | Failed to upload marketing agreement file. |
| 発信プロフィール | false | -3343 | M/Nタイプ使用申請失敗 | {} |
| 発信プロフィール | false | -3345 | 080受信拒否情報が無効 | Failed to update unsubscribe content. message: {} |
| 発信プロフィールグループ | false | -1010 | 発信プロフィールグループが存在しない場合 | Not exist plus friend group. |
| 発信プロフィールグループ | false | -1013 | すでに存在する発信プロフィールグループの場合 | Already exist plus friend group. |
| 発信プロフィールグループ | false | -1018 | 発信プロフィールがすでに発信プロフィールグループに登録されている場合 | This is a plusFriend that has already been added. |
| 発信プロフィールグループ | false | -1022 | 発信プロフィールが発信プロフィールグループに登録されていない場合 | This is not a plusFriend added to the group. |
| 発信プロフィールグループ | false | -1023 | 発信プロフィールグループ内の発信プロフィールが最大10個を超過した場合 | The max group size is 10. |
| 発信プロフィールグループ | false | -1025 | 発信プロフィールグループ削除不可 | The sender-group can't deleted. |
| 発信プロフィールグループ | false | -1026 | 発信プロフィールグループのメンバーが5000個を超過した場合 | The maximum number of members in a group is 5000. |
| 発信プロフィールグループ | false | -1029 | 発信プロフィールがブラックリストの場合、発信プロフィールグループに登録不可 | Blacklist can't join the group. |
| テンプレート | false | -1014 | 有効化されていない発信プロフィールの場合 | Not active status plus friend. |
| テンプレート | false | -2505 | クイックリプライを含む送信時、最大ボタン個数2個を超過した場合 | The 'buttons' is too many. If you request the 'quickReplies', the buttons size muse be 2 or less. |
| テンプレート      | false | -3001 | すでに存在するテンプレートコードまたはテンプレート名                                                                                                                             | Already exist templateCode or templateName.                                                                                                                                                                                                                                                                                                                                |
| テンプレート | false | -3012 | 修正できないテンプレート状態(承認/却下状態のみ可能) | Only templates with TSC03(APPROVE)/TSC04(REJECT) can be modified. |
| テンプレート | false | -3013 | すでに修正中のテンプレートが存在 | There are templates already being modified. |
| テンプレート | false | -3016 | 強調表記型テンプレートはtemplateTitle、templateSubtitle必須フィールド | The Template which emphasizeType is 'TEXT' must have templateTitle, templateSubtitle. |
| テンプレート | false | -3017 | templateSubtitleは置換変数使用不可 | The templateSubtitle can't include the replacement parameter. |
| テンプレート      | false | -3018 | 付加情報型 テンプレートはtemplateExtra必須フィールド                                                                                                                    | The Template which messageType is 'EX' must have templateExtra.                                                                                                                                                                                                                                                                                                            |
| テンプレート      | false | -3020 | 複合型テンプレートはtemplateExtra必須フィールド                                                                                                                       | The Template which messageType is 'MI' must have templateExtra.                                                                                                                                                                                                                                                                                                            |
| テンプレート      | false | -3021 | templateExtraは置換変数使用不可                                                                                                                        | The templateExtra can't include the replacement parameter.                                                                                                                                                                                                                                                                                                                 |
| テンプレート | false | -3024 | ACタイプボタンはチャンネル追加型複合型テンプレートのみ登録可能 | The button of AC type can using only templateMessageType (AD/MI). |
| テンプレート | false | -3025 | ACタイプボタンは単独使用または最上段に配置する必要があり、該当制約に違反した場合 | AC type buttons should be located alone or on top. |
| テンプレート | false | -3027 | テンプレート強調タイプ選択なし(NONE)で登録時、templateTitle、templateSubtitleフィールドを登録不可 | The Template which emphasizeType is 'NONE' can't have templateTitle, templateSubtitle. |
| テンプレート | false | -3028 | テンプレートメッセージタイプが基本型(BA)の場合、templateExtraフィールドを登録不可 | The Template which messageType is 'BA' can't have templateExtra. |
| テンプレート | false | -3030 | テンプレートメッセージタイプがチャンネル追加型(AD)の場合、templateExtraフィールドを登録不可 | The Template which messageType is 'AD' can't have templateExtra. |
| テンプレート | false | -3032 | テンプレート強調タイプが画像型(IMAGE)の場合、templateImageName、templateImageUrlフィールドが必須 | The Template which emphasizeType is 'IMAGE' must have templateImageName, templateImageUrl. |
| テンプレート | false | -3037 | templateItem listのtitleは置換変数を持つことができません | The templateItem's title can't include the replacement parameter. |
| テンプレート | false | -3047 | テンプレート登録時にフォントスタイルを適用しようとした場合(フォントスタイルは送信時に適用可能です。) | The templateTitle and templateItemHighlight's title can't end with \s. |
| テンプレート | false | -3050 | テンプレートメッセージタイプ(AD/MI)はACタイプのボタンを持つ必要があります | The templateMessageType (AD/MI) must have button of AC type. |
| テンプレート | false | -3100 | テンプレート申請/承認状態の場合、commentを追加できません | Could not add comment on registered/completed template status. |
| テンプレート | false | -3103 | buttonまたはクイックリプライの形式が正しくない場合(json形式ではない、またはエスケープ処理されていない文字が含まれている) | The button or quickReply has an invalid format. |
| テンプレート      | false | -4021 | ファイルサイズ超過(10MB)                                                                                                                                    | The file size is less than 10MB.                                                                                                                                                                                                                                                                                                                                           |
| テンプレート | false | -4024 | 休止テンプレート解除失敗 | Failed to release the dormant template. |
| テンプレート | false | -4025 | テンプレートアップロード時、一度にアップロード可能な最大個数である20個を超過した場合 | Only upload up to 20 templates at a time. |
| テンプレート | false | -4026 | テンプレートアップロードファイルのヘッダが無効な場合 | Uploaded template's headers are invalid. |
| テンプレート | false | -4027 | チャンネル追加型への変換失敗。buttonの長さ超過 | Conversion to AD/MI type failed. The 'buttons' length does not exceed its maximum length. |
| テンプレート | false | -4028 | チャンネル追加型への変換失敗。承認されていないテンプレート使用 | Conversion to AD/MI type failed. The template must be approved |
| 送信/照会 | false | -1016 | requestIdあるいはrecipientSeqでメッセージが見つからない場合 | It is not found any messages responding with that requestId or recipientSeq. |
| 送信/照会 | false | -1024 | 発信プロフィールがグループでメッセージを送信する場合 | The sender-group can't send the message. |
| 送信/照会 | false | -1031 | 送信リクエストした全受信者が送信に失敗した場合 | All of receivers are failed to send. |
| 送信/照会 | false | -2019 | テンプレート本文が1,300文字を超過したため失敗 | The content that is replaced by the template parameters can not exceed 1,300 characters. |
| 送信/照会 | false | -2025 | 予約日時が過去の日時の場合 | You can not send messages on past dates. Please check `requestDate` again. |
| 送信/照会 | false | -2026 | 予約日時が90日以降の日時の場合(最大90日まで可能) | You can not send messages after 90 days. Please check `requestDate` again. |
| 送信/照会 | false | -2027 | 日付形式が異なり、パースエラー発生 | The 'requestDate' has invalid format. |
| 送信/照会 | false | -2028 | 無効なリクエストID | The `requestId` is invalid |
| 送信/照会 | false | -2029 | リクエストしたメッセージがない、またはキャンセルできるメッセージがない場合 | All of your messages to cancel are not found or do not meet conditions to cancel. |
| 送信/照会 | false | -2030 | ワイド画像送信時、最大本文長さである76文字を超過した場合 | The 'content' is too long. If you request the 'wide-image', It must be less than 76 characters. |
| 送信/照会 | false | -2031 | ワイド画像送信時、最大ボタン個数2個を超過した場合 | The 'buttons' is too many. If you request the 'wide-image', It must be less than 2 buttons size. |
| 送信/照会 | false | -2032 | テンプレートタイトルが50文字を超過した場合 | The templateTitle that is replaced by the template parameters can not exceed 50 characters. |
| 送信/照会 | false | -2035 | テンプレートヘッダが16文字を超過した場合 | The templateHeader that is replaced by the template parameters can not exceed 16 characters. |
| 送信/照会 | false | -2500 | 大量送信リクエストが見つからない場合 | Not found mass message request |
| 送信/照会 | false | -2501 | リクエストしたメッセージがない、またはキャンセルできるメッセージがない場合 | Send request is failed. because the deadline is expired |
| 送信/照会    | false | -3011 | 登録したテンプレートボタンと一致しない場合                                                                                                                             | The buttons or quickReplies are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                            |
| 送信/照会 | false | -3033 | ボタンまたはクイックリプライがテンプレートに登録されていない場合 | The button or quickReply is not exist in template. |
| 送信/照会 | false | -3034 | 削除するテンプレートに3日以内の送信履歴がある場合 | The template could not be deleted because recently sent message. requestId: {} |
| 送信/照会 | false | -3035 | テンプレート強調タイプをアイテムリスト型(ITEM_LIST)で登録時、templateHeader、templateImageName、templateImageUrl、templateItem、templateItemHighlightフィールドのうち少なくとも1つを含む必要があります | The Template which emphasizeType is 'ITEM_LIST' must have at least one of templateImageInfo, templateHeader, templateItem and templateItemHighlight. |
| 送信/照会 | false | -3036 | テンプレート強調タイプをアイテムリスト型(ITEM_LIST)で登録時、セキュリティテンプレートとして登録不可 | The Template which emphasizeType is 'ITEM_LIST' can't be a security template. |
| 送信/照会 | false | -3039 | templateItem summaryはtemplateItem listなしでは存在不可 | The TemplateItem's summary can't exist without templateItem list |
| 送信/照会    | false | -3042 | templateHeaderがテンプレートと一致しない場合                                                                                                                    | The templateHeader are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                                     |
| 送信/照会    | false | -3043 | templateItemまたはtemplateItemHighlightがテンプレートと一致しない場合                                                                                             | The templateItem or templateItemHighlight are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                              |
| 送信/照会 | false | -3044 | ビジネスフォーム(BF)タイプボタンは単独使用または最上段に配置する必要があり、該当制約に違反した場合 | BF type buttons should be located on top. |
| 送信/照会 | false | -3045 | ビジネスフォーム(BF)タイプのnameが "トークで予約"、"トークでアンケート"、"トークで応募" ではない場合、または登録されていないbizFormKeyの場合 | The button linkType 'BF' must follow this constraint. The BF button must have a bizFormKey. The BF button name must be in "トークで予約する", "トークでアンケートする", "トークで応募する". |
| 送信/照会    | false | -3046 | templateRepresentLinkがテンプレートと一致しない場合                                                                                                             | The templateRepresentLink is not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                               |
| 送信/照会 | false | -3048 | テンプレートパラメータは1300文字を超えることはできません | The template parameter's length can not exceed 1300 characters. |
| 送信/照会 | false | -3049 | テンプレートパラメータがテンプレートと一致しない | The template parameter is not matched to template. |
| 送信/照会 | false | -3298 | ターゲティング情報が設定されていない | The targeting is invalid. |
| 送信/照会 | false | -3299 | コマース変数は指定された組み合わせでのみ使用可能: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed'] | The commerce variable must be used in the following combinations: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed'] |
| 送信/照会 | false | -3300 | 受信拒否処理された番号が見つかりません | The unsubscribe number could not be found. |
| 送信/照会 | false | -3301 | 受信拒否したユーザーが含まれています | The unsubscribed recipient are found. |
| 送信/照会 | false | -3344 | 画像変更パラメータの数がテンプレート画像の数と一致しません | The image parameter is invalid. image parameter's size must be equal to a number of template images. |
| 送信/照会 | false | -4017 | 90日以前のメッセージは照会できません | You can not search the messages before 90 days. |
| 送信/照会 | false | -4019 | 無効な受信者番号 | Invalid recipient number. |
| 送信/照会    | false | -4022 | データエクスポート失敗                                                                                                                                        | Failed to exporting data.                                                                                                                                                                                                                                                                                                                                                  |
| 送信/照会 | false | -4029 | 発信プロフィールのマーケティング受信同意が有効化されていない場合 | To send 'M/N type' messages to users who are not friends with your profile, you must first apply to have the 'M/N type' messaging feature enabled. Please check if this option is active in your sender profile settings. |
| 送信/照会 | false | -4104 | RequestIdが空です | RequestId is empty value. |
| 送信/照会 | false | -4200 | 無効な代替送信メッセージ | Resend Message is invalid. |
| 送信/照会 | false | -8006 | 認証メッセージ送信時、テンプレート内容に認証文言がない場合 | The content must contain auth guidement. |
| 送信/照会 | false | -8008 | メッセージ内容に禁止ワードが含まれている場合 | The content has banned word. |
| 画像 | false | -8000 | 画像シーケンス(imageSeq)がない場合 | The 'imageSeq' is empty. |
| 画像 | false | -8003 | 画像の削除に失敗した場合 | It is failed to delete images. |
| 画像 | false | -8004 | createUserフィールドの最大文字数100文字を超過した場合 | The 'createUser' is too long. It can be less than 100 characters. |
| 画像 | false | -8005 | 画像アップロード時、プロジェクトに登録されたプラスフレンドがない場合 | Your project doesn't have any plus friends. Please register it at first. |
| 画像 | false | -8011 | 存在しない画像タイプ | The image type is invalid. |
| 禁止ワード | false | -8012 | 禁止ワードが存在しない | Banned word does not exist. |
| 禁止ワード | false | -8013 | 禁止ワードがすでに存在 | Banned word already exists. |
| 本人認証 | false | -1030 | 本人認証を受けていない場合 | Self verification is required to use this service. |
| カカともへのメッセージ互換送信 | false | -2023 | メッセージ本文が400文字を超過する場合(画像添付)                                                                                                                    | The 'content' is too long. If you request the 'image', It can be less than 400 characters.                                                                                                                                                                                                                                                                                 |
| カカともへのメッセージ互換送信 | false | -2024 | メッセージ本文が1,300文字を超過する場合                                                                                                                         | The 'content' is too long. It can be less than 1,300 characters without any image.                                                                                                                                                                                                                                                                                         |
| カカともへのメッセージ互換送信 | false | -3200 | ワイドアイテムリストに名前がない | Friendtalk wide item must have a title. |
| カカともへのメッセージ互換送信 | false | -3201 | ワイドアイテムリストに画像がない | Friendtalk wide item must have a image. |
| カカともへのメッセージ互換送信 | false | -3202 | ワイドアイテムリストにlinkMoがない | Friendtalk wide item must have a linkMo. |
| カカともへのメッセージ互換送信 | false | -3203 | ワイドアイテムリストは3~4個のリストとヘッダが必要                                                                                                                     | Friendtalk wide item must have 3 ~ 4 list and a header.                                                                                                                                                                                                                                                                                                                    |
| カカともへのメッセージ互換送信 | false | -3204 | カルーセルにヘッダがない                                                                                                                                         | Friendtalk carousel must have a header.                                                                                                                                                                                                                                                                                                                                    |
| カカともへのメッセージ互換送信 | false | -3205 | カルーセルにメッセージがない                                                                                                                                        | Friendtalk carousel must have a message.                                                                                                                                                                                                                                                                                                                                   |
| カカともへのメッセージ互換送信 | false | -3206 | カルーセルに添付ファイルがない                                                                                                                                       | Friendtalk carousel must have a attachment.                                                                                                                                                                                                                                                                                                                                |
| カカともへのメッセージ互換送信 | false | -3207 | カルーセルに画像がない | Friendtalk carousel must have a image. |
| カカともへのメッセージ互換送信 | false | -3208 | カルーセルは2～10個のリストが必要<br/>カルーセルコマースタイプにintroがある場合は1～10個のリストが必要 | Friendtalk carousel must have 2 ~ 10 list. If message type is friendtalk carousel-commerce and carousel intro exists, carousel must have 1 ~ 10 list. |
| カカともへのメッセージ互換送信 | false | -3209 | カルーセル末尾にlinkMoがない | Friendtalk carousel tail must have linkMo. |
| カカともへのメッセージ互換送信 | false | -3210 | クーポンにはタイトルと説明が必要 | Friendtalk coupon must have a title and a description. |
| カカともへのメッセージ互換送信 | false | -3211 | カカともへのメッセージテキスト/画像タイプメッセージのクーポン説明は12文字超過不可、FW/FLタイプは18文字超過不可 | If the message type is friendTalk text/image type, the friendtalk coupon description's length cannot exceed 12 characters. If the message type is friendTalk wide-image/wide-item-list type, the friendtalk coupon description's length cannot exceed 18 characters |
| カカともへのメッセージ互換送信 | false | -3212 | クーポンタイトルの内容が無効 | Friendtalk coupon title is invalid. |
| カカともへのメッセージ互換送信 | false | -3213 | クーポンはモバイルリンクまたはチャンネルタイプios/androidリンクが必要 | Friendtalk must have a mobile link or a channel formatted ios/android link |
| カカともへのメッセージ互換送信 | false | -3215 | ワイドアイテムリストとカルーセルは広告タイプでのみ使用可能 | Friendtalk wide item / carousel can only be sent in AD type. |
| カカともへのメッセージ互換送信 | false | -3216 | ワイドアイテムリストの1番目のアイテムタイトルは25文字超過不可、2～4番目のアイテムタイトルは30文字超過不可 | Friendtalk first wide item's title length cannot exceed 25 characters, and 2nd ~ 4th wide item's title length cannot exceed 30 characters. |
| カカともへのメッセージ互換送信 | false | -3217 | カカともへのメッセージテキスト/画像タイプはボタン5個超過不可、クーポンを含む場合は4個まで可能、ワイド画像/ワイドアイテムリストは2個超過不可、プレミアムビデオタイプは1個超過不可、コマースタイプは1～2個まで可能 | Friendtalk button size is invalid. The button size must be 5 or less. If coupon is included, the button size must be 4 or less. If the message type is friendTalk wide-image/wide-item-list type, the button size must be 2 or less. If the message type is video type, the button size must be 1 or less. If the message type is commerce, the button size must be 1 or 2 |
| カカともへのメッセージ互換送信 | false | -3218 | カカともへのメッセージのvideoUrlが無効 | Friendtalk video url is invalid. |
| カカともへのメッセージ互換送信 | false | -3219 | カカともへのメッセージの内容が最大長を超過。プレミアムビデオタイプの内容の最大長は76文字 | The 'content' is too long. If you request the 'video', It must be less than 76 characters. |
| カカともへのメッセージ互換送信 | false | -3220 | カカともへのメッセージのヘッダが最大長を超過。プレミアムビデオタイプのヘッダの最大長は20文字 | The 'header' is too long. If you request the 'video', It must be 20 characters or fewer. |
| カカともへのメッセージ互換送信 | false | -3221 | カルーセルフィードタイプはカルーセルイントロ使用不可                                                                                                                           | Friendtalk carousel feed type cannot have a 'head' field.                                                                                                                                                                                                                                                                                                                  |
| カカともへのメッセージ互換送信 | false | -3222 | カルーセルフィードタイプは付加情報フィールド使用不可 | Friendtalk carousel feed type cannot have a 'additionalContent' field. |
| カカともへのメッセージ互換送信 | false | -3223 | カルーセルフィードタイプはコマース使用不可                                                                                                                               | Friendtalk carousel feed type cannot have a 'commerce' field.                                                                                                                                                                                                                                                                                                              |
| カカともへのメッセージ互換送信 | false | -3224 | カルーセルコマースタイプはヘッダとメッセージフィールド使用不可                                                                                                                       | Friendtalk carousel commerce type cannot have 'header' & 'message' fields.                                                                                                                                                                                                                                                                                                 |
| カカともへのメッセージ互換送信 | false | -3225 | カルーセルボタンが無効。カルーセルフィードタイプはボタン2個超過不可、カルーセルコマースタイプは1～2個のボタンが必要 | Friendtalk carousel button size is invalid. If the message type is friendtalk carousel-feed, the button size must be 2 or less. If the message type is friendtalk carousel-commerce, the button size must be 1 ~ 2. |
| カカともへのメッセージ互換送信 | false | -3226 | コマースにdiscountPriceフィールドがある場合、discountRateまたはdiscountFixedフィールドが必要 | If commerce has 'discountPrice' field, commerce must have a 'discountRate' or 'discountFixed' field. |

## 送信結果コード

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>コード値</th>
		<th>意味</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>1000</td>
		<td>成功</td>
	</tr>
  <tr>
		<td>1001</td>
		<td>Request BodyがJSON形式ではない</td>
	</tr>
  <tr>
		<td>1002</td>
		<td>ハブパートナーキーが無効</td>
	</tr>
  <tr>
		<td>1003</td>
		<td>発信プロフィールキーが無効</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>Request Body(JSON)でnameが見つからない</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>削除された発信プロフィール(カスタマーサポートに問い合わせ)</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>ブロック状態の発信プロフィール(カスタマーサポートに問い合わせ)</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>契約情報が見つからない(カスタマーサポートに問い合わせ)</td>
	</tr>
  <tr>
		<td>1012</td>
		<td>無効な形式のユーザーキーリクエスト</td>
	</tr>
  <tr>
		<td>1013</td>
		<td>無効なアプリ接続</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>無効な事業者番号</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>無効なapp user idリクエスト</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>事業者登録番号不一致</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>電話番号またはapp user idが無効、または未入力のリクエスト</td>
	</tr>
 	<tr>
		<td>1021</td>
		<td>ブロック状態のカカオトークチャンネル</td>
	</tr>
	<tr>
		<td>1022</td>
		<td>閉鎖状態のカカオトークチャンネル</td>
	</tr>
	<tr>
		<td>1023</td>
		<td>削除された カカオトークチャンネル</td>
	</tr>
	<tr>
		<td>1024</td>
		<td>削除待機状態のカカオトークチャンネル</td>
	</tr>
	<tr>
		<td>1025</td>
		<td>チャンネル制裁状態によるメッセージ送信失敗</td>
	</tr>
	<tr>
		<td>1027</td>
		<td>チャンネルメッセージ制裁状態によるメッセージ送信失敗</td>
	</tr>
    <tr>
       <td>1028</td>
       <td>該当ターゲティングオプションを使用不可</td>
    </tr>
	<tr>
		<td>1030</td>
		<td>誤ったパラメータリクエスト</td>
	</tr>
    <tr>
       <td>1033</td>
       <td>テンプレートメッセージタイプとchat\_bubble\_typeパラメータ不一致</td>
    </tr>
	<tr>
		<td>2001</td>
		<td>メッセージ送信不可(予期せぬエラー発生)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>メッセージ送信失敗(テストサーバーでカカオトークチャンネルを追加していない場合)</td>
	</tr>
    <tr>
		<td>2005</td>
		<td>カカオ内部システムエラーにより画像情報の読み込みに失敗</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>予期せぬエラー発生</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>メッセージを送信したが受信確認が取れない(成功不確実、サーバーには暗号化されて保管され3日以内に受信可能)</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>内部システムエラーによりメッセージ送信失敗</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>電話番号エラー</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>予期せぬエラー発生</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>メッセージが存在しない</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>カカオ通信失敗</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>メッセージが空</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>メッセージ長制限エラー</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>テンプレートが見つからない</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>メッセージ内容がテンプレートと一致しない</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>メッセージを送信できません<br>1. Android端末ユーザーの場合、携帯電話のUSIMとカカオトーク使用番号が異なる人<br>2. アクティブユーザーではない場合<br>アクティブユーザーとは？<br> * サーバーと接続されているカカオトークユーザー<br> * 送信当日に加入したユーザーを除き、直近7日(168時間)以内にカカオトークを使用したユーザー<br>3. 制裁ユーザーなど<br></td>
	</tr>
    <tr>
		<td>3019</td>
		<td>メッセージを送信できません<br>カカオトークユーザーではありません</td>
	</tr>
    <tr>
		<td>3020</td>
		<td>メッセージを送信できません<br>お知らせトーク受信拒否</td>
	</tr>
    <tr>
		<td>3021</td>
		<td>メッセージを送信できません<br>カカオトーク最小バージョン未対応</td>
	</tr>
	<tr>
		<td>3022</td>
		<td>送信可能な時間ではありません(カカともへのメッセージメッセージは08時から20時50分まで送信可能)</td>
	</tr>
	<tr>
		<td>3023</td>
		<td>メッセージ構文エラー(JSON形式エラー)</td>
	</tr>
	<tr>
		<td>3024</td>
		<td>メッセージに含まれる画像を転送できません(画像アドレスまたはリンクが正しくない、または画像が規格に合っていません)</td>
	</tr>
	<tr>
		<td>3025</td>
		<td>変数文字数制限超過</td>
	</tr>
	<tr>
		<td>3026</td>
		<td>相談/ボット切り替えボタン extra、event文字数制限超過</td>
	</tr>
	<tr>
		<td>3027</td>
		<td>メッセージボタン/クイックリプライがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3028</td>
		<td>メッセージ強調表記タイトルがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3029</td>
		<td>メッセージ強調表記タイトル長制限超過(50文字)</td>
	</tr>
	<tr>
		<td>3030</td>
		<td>メッセージタイプとテンプレート強調タイプが一致しません</td>
	</tr>
	<tr>
		<td>3031</td>
		<td>ヘッダがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3032</td>
		<td>ヘッダ長制限超過(16文字)</td>
	</tr>
	<tr>
		<td>3033</td>
		<td>アイテムハイライトがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3034</td>
		<td>アイテムハイライトタイトル長制限超過(画像がない場合30文字、画像がある場合21文字)</td>
	</tr>
	<tr>
		<td>3035</td>
		<td>アイテムハイライトディスクリプション長制限超過(画像がない場合19文字、画像がある場合13文字)</td>
	</tr>
	<tr>
		<td>3036</td>
		<td>アイテムリストがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3037</td>
		<td>アイテムリストのアイテムのディスクリプション長制限超過(23文字)</td>
	</tr>
	<tr>
		<td>3038</td>
		<td>アイテム要約情報がテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3039</td>
		<td>アイテム要約情報のディスクリプション長制限超過(14文字)</td>
	</tr>
	<tr>
		<td>3040</td>
		<td>アイテム要約情報のディスクリプションに許可されていない文字が含まれています(通貨記号/コード、数字、カンマ、小数点、空白を除く文字が含まれています)</td>
	</tr>
	<tr>
		<td>3041</td>
		<td>ワイドアイテムリスト個数が最大最小個数不一致</td>
	</tr>
	<tr>
		<td>3042</td>
		<td>代表リンクがテンプレートと一致しません</td>
	</tr>
	<tr>
		<td>3043</td>
		<td>画像変数個数がテンプレートと不一致</td>
	</tr>
	<tr>
		<td>3044</td>
		<td>クーポン変数がテンプレートと不一致</td>
	</tr>
	<tr>
		<td>3045</td>
		<td>コマース情報変数がテンプレートと不一致</td>
	</tr>
	<tr>
		<td>3046</td>
		<td>付加情報最大長制限エラー</td>
	</tr>
	<tr>
		<td>3047</td>
		<td>コマース情報商品名最大長制限エラー</td>
	</tr>
	<tr>
		<td>3048</td>
		<td>無効なグループタグキー入力</td>
	</tr>
    <tr>
       <td>3050</td>
       <td>受信同意拒否スペック(Nタイプ)未対応</td>
    </tr>
	<tr>
		<td>3051</td>
		<td>カルーセルアイテムリスト個数が最小、最大個数不一致</td>
	</tr>
	<tr>
		<td>3052</td>
		<td>カルーセルアイテムメッセージ長超過</td>
	</tr>
	<tr>
		<td>3053</td>
		<td>カルーセルテンプレート不一致</td>
	</tr>
    <tr>
       <td>3054</td>
       <td>カルーセルボタンテンプレート不一致</td>
    </tr>
    <tr>
       <td>3055</td>
       <td>カルーセルクーポンテンプレート不一致</td>
    </tr>
	<tr>
		<td>3056</td>
		<td>ワイドアイテムリストタイトル長制限エラー</td>
	</tr>
    <tr>
       <td>3057</td>
       <td>カルーセルコマーステンプレート不一致</td>
    </tr>
	<tr>
		<td>3058</td>
		<td>カルーセルヘッダ長制限エラー</td>
	</tr>
	<tr>
		<td>4000</td>
		<td>メッセージ送信結果が見つかりません</td>
	</tr>
	<tr>
		<td>4001</td>
		<td>不明なメッセージ状態</td>
	</tr>
    <tr>
       <td>4100</td>
       <td>requestIdエラー</td>
    </tr>
    <tr>
       <td>4101</td>
       <td>リクエスト日付エラー</td>
    </tr>
    <tr>
       <td>4102</td>
       <td>Templateリクエストエラー</td>
    </tr>
    <tr>
       <td>4103</td>
       <td>有効なハブパートナーが見つかりません</td>
    </tr>
    <tr>
       <td>4104</td>
       <td>有効な発信プロフィールが見つかりません</td>
    </tr>
    <tr>
       <td>4110</td>
       <td>無効なチャットバブルタイプまたはメッセージタイプリクエスト</td>
    </tr>
    <tr>
       <td>4120</td>
       <td>メッセージリクエストペイロード生成エラー</td>
    </tr>
    <tr>
       <td>4121</td>
       <td>メッセージ送信対象エラー</td>
    </tr>
    <tr>
       <td>4122</td>
       <td>メッセージ結果照会エラー</td>
    </tr>
    <tr>
       <td>4130</td>
       <td>requestIdパターンエラー</td>
    </tr>
    <tr>
       <td>4131</td>
       <td>重複requestId</td>
    </tr>
    <tr>
       <td>4132</td>
       <td>テンプレート変数不一致</td>
    </tr>
    <tr>
       <td>4133</td>
       <td>中断されたテンプレート</td>
    </tr>
    <tr>
       <td>4134</td>
       <td>変更された テンプレート</td>
    </tr>
    <tr>
       <td>4137</td>
       <td>メッセージリクエスト失敗</td>
    </tr>
    <tr>
       <td>4138</td>
       <td>ブランドメッセージのメッセージ数制限</td>
    </tr>
    <tr>
       <td>4139</td>
       <td>メッセージリクエスト失敗</td>
    </tr>
    <tr>
       <td>4140</td>
       <td>メッセージリクエスト失敗</td>
    </tr>
    <tr>
       <td>4141</td>
       <td>メッセージリクエスト失敗</td>
    </tr>
    <tr>
       <td>4142</td>
       <td>メッセージリクエスト失敗</td>
    </tr>
    <tr>
       <td>4143</td>
       <td>期限切れのリクエスト</td>
    </tr>
    <tr>
       <td>4144</td>
       <td>本文長制限(30KB)超過</td>
    </tr>
    <tr>
       <td>4156</td>
       <td>最大送信数超過</td>
    </tr>
    <tr>
       <td>4161</td>
       <td>処理中のメッセージ</td>
    </tr>
    <tr>
		<td>9998</td>
		<td>システムに問題が発生し、担当者が確認中(現在サービス提供中ではありません)</td>
	</tr>
    <tr>
		<td>9999</td>
		<td>システムに問題が発生し、担当者が確認中(システムに不明なエラー発生)</td>
	</tr>
</tbody>
</table>
