## Notification > KakaoTalk Bizmessage > Brand Message > API v1.0 Guide

## Brand Message

#### \[API Domain]

| Domain|
|----------|
| [https://kakaotalk-bizmessage.api.nhncloudservice.com](https://kakaotalk-bizmessage.api.nhncloudservice.com)|

## Introduce v1.0 API

## Manage non-friend message sending (targeting M, N)

Non-friend message sending (targeting M, N) can be sent if all the conditions below are met:

- Business verification channel
- Register a business registration number
- Register a channel customer service center phone number
- 50,000 or more channel friends
- Successfully send notification messages within the past three months

### Upload marketing consent evidence

#### Requested

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| file| File| O| Marketing consent evidence|

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

### Apply for using non-friend message sending (targeting M, N)

#### Requested

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

## Request to send a free-form message

* You can send a marketing consent.
  * You can specify the type of message target by specifying the targeting field.
      * M: Users who agree to receive advertising information from the client company (KakaoTalk message consent)
      * N: Users who agree to receive advertising information from the client company - Channel friend
      * I: Target of customer sending request ∩ Channel friend
* You can use all 8 message types of the existing FriendTalk.
* You can use BT, AC button types.
* Note the following constraints when using the AC (Add Channel) button:
    * Display as a highlighted button (Yellow).
    * Place the button in the designated position when multiple buttons are present:
        * TEXT, IMAGE: Must be the furst button (Top position)
        * Other types: Must be the second button (Right position)
    * Set the button name (name) exclusively to "Add Channel".
    * Limit usage to a maximum of one AC button across the entire carousel.
    * Allow only targeting types M and N..
* Support for the BF button by providing a Business Form ID issued by Kakao.
* Fallback can be set via resendParameter for each recipient.
  * When using fallback, you need to register the SMS Appkey and set its sending through the fallback management API.
  * <b>Nighttime delivery restrictions(8:50 PM - 8:00 AM the next day)</b>

#### Requested

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Request text-type sending

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "TEXT",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String, 
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| O| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally. Up 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| Opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request image-type sending

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "IMAGE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| O| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally. Up 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| image| Object| O| Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O| Image URL. Use an image URL uploaded as a general image|
| \- imageLink| String| X| URL to go to when the image is clicked. Limited to 1,000 characters<br>If not set, use the image viewer in KakaoTalk|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request wide image-type sending

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| O| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally. Up 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| image| Object| O| Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O| Image URL, use an image URL uploaded as a wide image|
| \- imageLink| String| X| URL to go to when the image is clicked. Limited to 1,000 characters<br>If not set, use the image viewer in KakaoTalk|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request to send wide item list type

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| header| String| O| Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)<br>- Optional field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)|
| item| Object| O| Wide list item (available only for WIDE_ITEM_LIST type)|
| \- list| List| O| Wide list (minimum: 3, maximum 4)|
| \-- title| String| O| Item title<br>- The first item is limited to up to 25 characters (linebreak: up to 1; for the first item, title is not required)<br>- 2 to 4th items are limited to up to 30 characters (linebreak: up 1 item)|
| \-- imageUrl| String| O| Item image URL<br>- The first item uses the image URL uploaded as the first wide item list image.<br>- 2 to 4th items use the image URL uploaded as a general wide item list image.|
| \-- linkMo| String| O| Mobile web link, limited to 1,000 characters|
| \-- linkPc| String| X| PC web link, limited to 1,000 characters|
| \-- schemeAndroid| String| X| Android app link, limited to 1,000 characters|
| \-- schemeIos| String| X| IOS app link, limited to 1,000 characters|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request to send premium video type

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| X| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally, 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| header| String| X| Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)<br>- Optional field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)|
| video| Object| O| Video elements (only PREMIUM_VIDEO type available)|
| \- videoUrl| String| O| KakaoTV video URL (only video addresses uploaded on KakaoTV available), limited to up to 500 characters|
| \- thumbnailUrl| String| X| Image URL for video thumbnail, only URLs uploaded as general images available (if none is available, the default KakaoTV video thumbnail is used). Limited to up to 500 characters.|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request to send commerce

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X| Send status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X| Message status for adults (defaults: false)|
| additionalContent| String| X| Additional information (up to 34 characters, linebreak: up to 1), available only for commerce type|
| image| Object| O| Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O| Image URL. Use an image URL uploaded as a general image|
| \- imageLink| String| X| URL to go to when the image is clicked. Limited to 1,000 characters<br>If not set, use the image viewer in KakaoTalk|
| commerce| Object| O| Commerce (available only for COMMERCE type)|
| title| String| O| Product title (up to 30 characters, linebreak: unavailable)|
| regularPrice| Integer| O| Regular price (0 to 99,999,999)|
| discountPrice| Integer| X| Discount price (0 to 99,999,999)|
| discountRate| Integer| X| Discount rate (0 to 100), discount rate when a discount price exists. One of the fixed discount price is required|
| discountFixed| Integer| X| Fixed discount price (0 to 999,999), discount rate when a discount price exists., One of the fixed discount prices is required|
| buttons| List| O| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters|
| \- chatExtra| String| X| Meta information to send when the button is BT type|
| \- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18 characters. Linebreak: unavailable<br>- For other types, up to 12 characters. Linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 1,000 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| iOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request to send carousel feed type
##### Carousel fixed placeholders (path-based template parameter)

Allow unique placeholder values for each item in a carousel-type template.

* **Usage format**: `key@$.carousel.list[index]`(e.g. `productName@$.carousel.list[0]`)
* If a carousel intro (head) is present, it occupies `list[0]`, while actual content items begin from `list[1]`.
* If no value is matched using the path-based key, it will fall back to the standard key.

| Category | Replaceable fields | path format |
|------|--------------|----------|
| Carousel intro (head) | header, content, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[0]`(if head exists) |
| Carousel item | header, message, additionalContent | `key@$.carousel.list[index]` |
| Carousel item button | linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item coupon | title, description, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item commerce | title | `key@$.carousel.list[index]` |

* **Tail**: Placeholders are not supported
* Button indices are not included in the path. All buttons within the same item are replaced using the same path value.

> **Caution** The @ character cannot be used in placeholder keys (#{key}). The @ character is reserved as a path delimiter within the system.

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| senderKey| String| O        | Outgoing key (40 characters). Group outgoing key unavailable|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X        | Send status for message push notifications (defaults: true)|
| requestDate| String| X        | Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X        | 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X        | 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X        | Message status for adults (defaults: false)|
| carousel| Object| O        | Carousel|
| \- list| List| O        | Carousel list (minimum 2, maximum 6)|
| \-- header| String| O        | Carousel item title (up to 20 characters). Available only for carousel feed type|
| \-- message| String| O        | Carousel item title (up to 20 characters), carousel item message (up to 180 characters). Available only for carousel feed type|
| \-- imageUrl| String| O        | Image URL (Only images uploaded as carousel feed type images are available)|
| \-- imageLink| String| X        | Image link, limited to 1,000 characters|
| \-- buttons| List| O        | Carousel list button list, minimum 1, maximum 2|
| \--- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \--- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 1,000 characters|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 1,000 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 1,000 characters|
| \--- schemeIos| String| X        | IOS app link (required for AL type), limited to 1,000 characters|
| \--- chatExtra| String| X        | Meta information to send when the button is BT type|
| \--- chatEvent| String| X        | Bot event name to connect when the button is BT type|
| \--- bizFormKey| String| X        | BizForm key when the button is BF type|
| \-- coupon| Object| X        | Coupon elements|
| \--- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \--- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 1,000 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- schemeIos| String| X        | IOS app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- tail| Object| X        | More button information|
| \-- linkMo| String| O        | Mobile web link, limited to 1,000 characters|
| \-- linkPc| String| X        | PC web link, limited to 1,000 characters|
| \-- schemeAndroid| String| X        | Android app link, limited to 1,000 characters|
| \-- schemeIos| String| X        | IOS app link, limited to 1,000 characters|
| recipientList| List| O        | Recipient list (up to 1,000)|
| \- recipientNo| String| O        | Incoming number|
| \- resendParameter| Object| X        | Fallback Information|
| \-- isResend| boolean| X        | Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X        | Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X        | LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X        | Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X        | Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \-- resendUnsubscribeNo| String| X        | Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X        | Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X        | 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X        | 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X        | Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X        | Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X        | Reseller code (used by resellers when sending)|
| createUser| String| X        | Registrant (stored as user UUID when sent from console)|
| statsId| String| X        | Statistics ID (not included in the outgoing search conditions. Up 8 characters)|

#### Request to send carousel commerce type

\[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
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
      },
      "targeting": String,
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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| senderKey| String| O        | Outgoing key (40 characters), group outgoing key unavailable|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X        | Sending status for message push notifications (defaults: true)|
| requestDate| String| X        | Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X        | 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X        | 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| adult| boolean| X        | Message status for adults (defaults: false)|
| carousel| Object| O        | Carousel|
| \- head| Object| X        | Carousel intro|
| \-- header| String| O        | Carousel intro header (up to 20 characters)|
| \-- content| String| O        | Carousel intro content (up to 50 characters)|
| \-- imageUrl| String| O        | Carousel intro image address (use an image uploaded as a carousel commerce type image; the used image should have the same image and ratio of the carousel.)|
| \-- linkMo| String| X        | Mobile web link (linkMo is required if one of the linkMo, linkPc, schemeAndroid, schemeIos will be used), limited to up to 1,000 characters|
| \-- linkPc| String| X        | PC web link, limited to 1,000 characters|
| \-- schemeAndroid| String| X        | Android app link, limited to 1,000 characters|
| \-- schemeIos| String| X        | IOS app link, limited to 1,000 characters|
| \- list| List| O        | Carousel list (If head exists, minimum 1, maximum 5 / otherwise minimum 2, maximum 6)|
| \-- additionalContent| String| X        | Additional info (up to 34 characters), available only for carousel commerce|
| \-- imageUrl| String| O        | Image URL (Only images uploaded as carousel commerce type images are available)|
| \-- imageLink| String| X        | Image link, limited to 1,000 characters|
| \-- commerce| Object| O        | Commerce (available only for CAROUSEL_COMMERCE type)|
| \--- title| String| O        | Product title (up to 30 characters, linebreak: unavailable)|
| \--- regularPrice| Integer| O        | Regular price (0 to 99,999,999)|
| \--- discountPrice| Integer| X        | Discount price (0 to 99,999,999)|
| \--- discountRate| Integer| X        | Discount rate (0 to 100), discount rate when a discount price exists., One of the fixed discount prices is required|
| \--- discountFixed| Integer| X        | Fixed discount price (0 to 999,999), discount rate when a discount price exists., One of the fixed discount prices is required|
| \-- buttons| List| O        | Carousel list button list, minimum 1, maximum 2|
| \--- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters|
| \--- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: message forwarding, AC: add channel, BT: chatbot conversion, BF: Business form)<br>- BT type is available only in the channel using the chatbot of Kakao i Open Builder<br>- BF type can only be used as the first button, and only the following three phrases can be used for names:<br>&nbsp;&nbsp;- Make a reservation via KakaoTalk<br>&nbsp;&nbsp;- Take a survey via KakaoTalk<br>&nbsp;&nbsp;- Apply via KakaoTalk|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 1,000 characters|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 1,000 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 1,000 characters|
| \--- schemeIos| String| X        | IOS app link (required for AL type), limited to 1,000 characters|
| \--- chatExtra| String| X        | Meta information to send when the button is BT type|
| \--- chatEvent| String| X        | Bot event name to connect when the button is BT type|
| \--- bizFormKey| String| X        | BizForm key when the button is BF type|
| \-- coupon| Object| X        | Coupon elements|
| \--- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \--- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 1,000 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- schemeIos| String| X        | IOS app link (required field for AL type), limited to 1,000 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- tail| Object| X        | More button information|
| \-- linkMo| String| O        | Mobile web link, limited to 1,000 characters|
| \-- linkPc| String| X        | PC web link, limited to 1,000 characters|
| \-- schemeAndroid| String| X        | Android app link, limited to 1,000 characters|
| \-- schemeIos| String| X        | IOS app link, limited to 1,000 characters|
| recipientList| List| O        | Recipient list (up to 1,000)|
| \- recipientNo| String| O        | Incoming number|
| \- resendParameter| Object| X        | Fallback Information|
| \-- isResend| boolean| X        | Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X        | Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X        | LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X        | Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X        | Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \- targeting| String| X        | Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- unsubscribeNo| String| X        | 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| \- unsubscribeAuthNo| String| X        | 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|
| \- recipientGroupingKey| String| X        | Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X        | Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X        | Reseller code (used by resellers when sending)|
| createUser| String| X        | Registrant (stored as user UUID when sent from console)|
| statsId| String| X        | Statistics ID (not included in the outgoing search conditions, up to 8 characters)|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Response header info|
| \- isSuccessful| boolean| O| Successful API call status|
| \- resultCode| Integer| O| API call result code (success: 0, error code for failure)|
| \- resultMessage| String| O| API call result message ("success" or a related success message on success, detailed message on failure, cause on failure)|
| message| Object| X| message sending result information (exists only if there is a send request)|
| \- requestId| String| X| Sending request ID (an ID that uniquely identifies each sending request)|
| \- sendResults| Array| O| List of sending results by recipient|
| \-- recipientSeq| Integer| O| Number in the recipient list|
| \-- recipientNo| String| X| Recipient’s phone number|
| \-- resultCode| Integer| O| Recipient-specific sending result code (success and various failure codes may exist)|
| \-- resultMessage| String| O| Recipient-specific sending result message ("success" or a related message on success, detailed message on failure, cause on failure)|

## Request to send basic message

* A sending using a template.
* You can use the marketing consent sending.
  * You can specify the type of message target by specifying the targeting field.
      * M: Users who agree to receive advertising information from the client company (KakaoTalk message consent)
      * N: Users who agree to receive advertising information from the client company - Channel friend
      * I: Target of customer sending request ∩ Channel friend
* You can use all 8 message types of the existing FriendTalk.
* You cannot use BT button types.
* You can use Add Channel (AC) button.
* When using the BF button, you can upload the Business Form ID issued by Kakao and receive and use a BizForm key.
* Fallback can be set via resendParameter for each recipient.
  * When using fallback, you need to register the SMS Appkey and set its send through the fallback management API.
* <b>Nighttime delivery restrictions(8:50 PM - 8:00 AM the next day)</b>

### Cautions for use

- unsubscribeNo and unsubscribeAuthNo are the 080 toll-free opt-out phone number and verification number. If either one is not entered, the message will be sent using the toll-free opt-out information registered in the outgoing profile.
- If you enter unsubscribeNo or unsubscribeAuthNo between deliveries, the message will be sent using the entered values, not the toll-free opt-out information registered in the outgoing profile.
- If you do not enter unsubscribeNo or unsubscribeAuthNo between deliveries, the message will be sent using the toll-free opt-out information registered in the outgoing profile.
- unsubscribeNo, unsubscribeAuthNo can be entered by recipient. When entering both common fields and recipient-specific fields, the common fields take precedence.

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Requested

```
{
  "senderKey": String,
  "templateCode": String,
  "pushAlarm": boolean,
  "requestDate": String,
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

| Name| Type| Required| Description|
|----------|----------|----------|--------------------------------------------------------------------------------------------------------------------|
| senderKey| String| O| Outgoing key (40 characters), group outgoing key unavailable|
| templateCode| String| O| Template code to use|
| pushAlarm| boolean| X| Sending status for message push notifications (defaults: true)|
| requestDate| String| X| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                 |
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234                                                                                 |
| recipientList| List| O| Recipient list (up to 1,000)|
| \- recipientNo| String| O| Incoming number|
| \- targeting| String| O| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \- templateParameter| Object| X| Template parameter (required if including variables to be replaced in the template)<br>- carousel type: Use `key@$.carousel.list[Index]` to apply unique placeholder values per item (e.g., `productName@$.carousel.list[0]`)<br>- If a head is present, it occupies list[0]; actual items start from list[1]<br>- `@` character is prohibited in placeholder keys<br>- Max 1,300 characters each for key and value|
| \- imageParameters| List| X| Dynamic parameters that can change the template image field value, carousel type is currently not supported (only JSON list with the same size as the number of images in the template can be used. If used, images that do not want to be changed must be entered as an empty JSON object)                                                 |
| \-- imageUrl| String| O| Image URL |
| \-- imageLink| String| X| Image link|
| \- resendParameter| Object| X| Fallback Information|
| \-- isResend| boolean| X| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| \-- resendType| String| X| Fallback type (SMS, LMS)<br>If no value exists, types are distinguished according to the length of the template body.|
| \-- resendTitle| String| X| LMS fallback title<br>(if no value exists, it will be replaced with PlusFriend ID.)|
| \-- resendContent| String| X| Fallback title<br>(if no value exists, it will be replaced with [message body].)|
| \-- resendSendNo| String| X| Fallback number<br><span style="color:red">(if the outgoing number is not registered with the SMS service, the fallback may fail.)</span>|
| \- unsubscribeNo| String| X| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                   |
| \- unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234                                                                                   |
| \- recipientGroupingKey| String| X| Recipient grouping key (you can specify grouping keys by recipient. Up 100 characters)|
| senderGroupingKey| String| X| Sender grouping key (you can specify grouping keys by sender. Up 100 characters)|
| resellerCode| String| X| Reseller code (used by resellers when sending)|
| createUser| String| X| Registrant (stored as user UUID when sent from console)|
| statsId| String| X| Statistics ID (not included in the outgoing search conditions, up to 8 characters)|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Response header info|
| \- isSuccessful| boolean| O| Successful API call status|
| \- resultCode| Integer| O| API call result code (success: 200, error code for failure)|
| \- resultMessage| String| O| API call result message ("success" or a related success message on success, detailed message on failure, cause on failure)|
| message| Object| X| Message sending result information (exists only if there is a sending request)|
| \- requestId| String| X| Sending request ID (an ID that uniquely identifies each sending request)|
| \- sendResults| Array| O| List of sending results by recipient|
| \-- recipientSeq| Integer| O| Number in the recipient list|
| \-- recipientNo| String| X| Recipient’s phone number|
| \-- resultCode| Integer| O| Recipient-specific sending result code (success and various failure codes may exist)|
| \-- resultMessage| String| O| Recipient-specific sending result message ("success" or a related message on success, detailed message on failure, cause on failure)|

## View sending list

#### Requested

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

[Query parameter] Condition 1 or 2 required

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| requestId| String| Condition required (1)| Request ID|
| startRequestDate| String| Condition required (2)| Start value of the sending request date (yyyy-MM-dd HH:mm)|
| endRequestDate| String| Condition required (2)| End value of the sending request date (yyyy-MM-dd HH:mm)|
| senderKey| String| X| Outgoing Key|
| templateCode| String| X| Template Code|
| recipientNo| String| X| Incoming number|
| messageStatus| String| X| Request status (COMPLETED: success, FAILED: failure)|
| resultCode| String| X| Sending result (MRC01: Success MRC02: failure )|
| senderGroupingKey| String| X| Outgoing grouping key|
| recipientGroupingKey| String| X| Recipient grouping key|
| pageNum| String| X| Page number (default: 1\)|
| pageSize| String| X| Number of views (default: 15, Max: 1,000)|

#### Response

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
        "createUser": String,
        "senderGroupingKey": String,
        "recipientGroupingKey": String
      }
    ],
    "totalCount": Integer
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| messageSearchResultResponse| Object| X| Body area|
| \- messages| Array| O| Message list|
| \-- requestId| String| O| Request ID|
| \-- recipientSeq| Integer| O| Recipient sequence number|
| \-- plusFriendId| String| O| Outgoing profile ID|
| \-- senderKey| String| O| Outgoing Key|
| \-- templateCode| String| X| Template Code|
| \-- recipientNo| String| O| Incoming number|
| \-- targeting| String| O| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends)|
| \-- requestDate| String| O| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance|
| \-- createDate| String| O| Registered on|
| \-- receiveDate| String| X| Received on|
| \-- chatBubbleType| String| O| Message type|
| \-- pushAlarm| boolean| O| Push notification status|
| \-- messageStatus| String| O| Request status (COMPLETED: success, FAILED: failure)|
| \-- resendStatusCode| String| X| Fallback status code|
| \-- resendStatusName| String| X| Fallback status name|
| \-- resendResultCode| String| X| Fallback result code|
| \-- resendRequestId| String| X| Fallback request ID|
| \-- isAddedChannel| boolean| O| Channel friend status|
| \-- resultCode| String| X| Received result code|
| \-- resultCodeName| String| X| Received result code name|
| \-- createUser| String| X| Registrant (stored as user UUID when sent from console)|
| \-- senderGroupingKey| String| X| Outgoing grouping key|
| \-- recipientGroupingKey| String| X| Recipient grouping key|
| \- totalCount| Integer| O| Total|

## View single sending

#### Requested

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| requestId| String| Request ID|
| recipientSeq| Integer| Recipient sequence number|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

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
    "createUser": String,
    "senderGroupingKey": String,
    "recipientGroupingKey": String
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| message| Object| X| Message body area (May not be present when the message fails)|
| \- requestId| String| O| Request ID (Not Null if message object exists)|
| \- recipientSeq| Integer| O| Recipient sequence number (Not Null if message object exists)|
| \- plusFriendId| String| O| Outgoing profile ID (Not Null if message object exists)|
| \- senderKey| String| O| Outgoing key (Not Null if message object exists)|
| \- templateCode| String| X| Template Code|
| \- recipientNo| String| O| Incoming number (Not Null if message object exists)|
| \- targeting| String| O| Type of message target (M: User with marketing consent, N: only users that agreed to marketing consent and are not friends, I: users who are friends) (Not Null if message object exists)|
| \- requestDate| String| O| Requested date (yyyy-MM-dd HH:mm)<br>(send immediately if not entered)<br>Available to schedule up to 60 days in advance (Not Null if message object exists)|
| \- createDate| String| O| Registered on (Not Null if message object exists)|
| \- receiveDate| String| X| Received on|
| \- chatBubbleType| String| O| Message type (Not Null if message object exists)|
| \- content| String| X| Message Content|
| \- adult| boolean| O| Message status for adults (Not Null if message object exists)|
| \- header| String| X| Header (within message)|
| \- additionalContent| String| X| Additional info (within message)|
| \- image| Object| X| Image elements|
| \-- imageUrl| String| O| Image URL (Not Null if image object exists)|
| \-- imageLink| String| X| Image link|
| \- buttons| Array| X| Button list|
| \-- name| String| O| Button title (Not Null if button array item exists)|
| \-- type| String| O| Button type (Not Null if button array item exists)|
| \-- linkMo| String| X| Mobile web link|
| \-- linkPc| String| X| PC web link|
| \-- schemeIos| String| X| IOS app link|
| \-- schemeAndroid| String| X| Android app link|
| \-- chatExtra| String| X| Meta information to send when the button is BT type|
| \-- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \-- bizFormKey| String| X| BizForm key when the button is BF type|
| \- item| Object| X| Wide list elements|
| \-- list| Array| X| Wide list (Nullable if item object exists)|
| \--- title| String| X| Item title|
| \--- imageUrl| String| O| Item image URL (Not Null if item.list item exists)|
| \--- linkMo| String| O| Mobile web link (Not Null if item.list item exists)|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \- coupon| Object| X| Coupon elements|
| \-- title| String| O| Coupon title (Not Null if coupon object exists)|
| \-- description| String| O| Coupon details (Not Null if coupon object exists)|
| \-- linkMo| String| X| Mobile web link|
| \-- linkPc| String| X| PC web link|
| \-- schemeAndroid| String| X| Android app link|
| \-- schemeIos| String| X| IOS app link|
| \- commerce| Object| X| Commerce elements|
| \-- title| String| O| Product title (Not Null if commerce object exists)|
| \-- regularPrice| Integer| X| Regular price|
| \-- discountPrice| Integer| X| Discount price|
| \-- discountRate| Integer| X| Discount rate|
| \-- discountFixed| Integer| X| Fixed discount price|
| \- video| Object| X| Video elements|
| \-- videoUrl| String| O| KakaoTV video URL (Not Null if video item exists)|
| \-- thumbnailUrl| String| X| Image URL for video thumbnail|
| \- carousel| Object| X| Carousel|
| \-- head| Object| X| Carousel intro (Nullable if carousel object exists)|
| \--- header| String| O| Carousel intro header (Not Null if head object exists)|
| \--- content| String| O| Carousel intro content (Not Null if head object exists)|
| \--- imageUrl| String| O| Carousel intro image address (Not Null if header object exists)|
| \--- linkMo| String| X| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \-- list| Array| O| Carousel list (Not Null if carousel object exists)|
| \--- header| String| X| Carousel item header|
| \--- message| String| O| Carousel item message (Not Null if list item exists)|
| \--- additionalContent| String| X| Additional Info|
| \--- imageUrl| String| X| Image URL|
| \--- imageLink| String| X| Image link|
| \--- commerce| Object| X| Commerce (within carousel)|
| \---- title| String| O| Product title (Not Null if carousel.list.commerce exists)|
| \---- regularPrice| Integer| X| Regular price|
| \---- discountPrice| Integer| X| Discount price|
| \---- discountRate| Integer| X| Discount rate|
| \---- discountFixed| Integer| X| Fixed discount price|
| \--- buttons| Array| X| Button list (within carousel)|
| \---- name| String| O| Button title (Not Null if carousel.list.buttons item exists)|
| \---- type| String| O| Button type (Not Null if carousel.list.buttons item exists)|
| \---- linkMo| String| X| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \---- schemeAndroid| String| X| Android app link|
| \---- schemeIos| String| X| IOS app link|
| \---- chatExtra| String| X| Meta information to send when the button is BT type|
| \---- chatEvent| String| X| Bot event name to connect when the button is BT type|
| \---- bizFormKey| String| X| BizForm key when the button is BF type|
| \--- coupon| Object| X| Coupon (within carousel)|
| \---- title| String| O| Coupon title (Not Null if carousel.list.coupon exists)|
| \---- description| String| O| Coupon details (Not Null if carousel.list.coupon exists)|
| \---- linkMo| String| X| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \---- schemeAndroid| String| X| Android app link|
| \---- schemeIos| String| X| IOS app link|
| \-- tail| Object| X| More button information (Nullable if carousel object exists)|
| \--- linkMo| String| O| Mobile web link (Not Null if tail object exists)|
| \--- linkPc| String| X| PC web link|
| \--- schemeAndroid| String| X| Android app link|
| \--- schemeIos| String| X| IOS app link|
| \- templateParameter| String| X| Template parameter|
| \- pushAlarm| boolean| O| Push notification status (Not Null if message object exists)|
| \- messageStatus| String| O| Request status (COMPLETED: success, FAILED: failure) (Not Null if message object exists)|
| \- isAddedChannel| boolean| O| Channel friend status (Not Null if message object exists)|
| \- resultCode| String| X| Received result code (within message)|
| \- resultCodeName| String| X| Received result code name (within message)|
| \- resendStatusCode| String| X| Fallback status code|
| \- resendStatusName| String| X| Fallback status code name|
| \- resendResultCode| String| X| Fallback result code|
| \- resendRequestId| String| X| Fallback request ID|
| \- createUser| String| X| Registrant (stored as user UUID when sent from console)|
| \- senderGroupingKey| String| X| Outgoing grouping key|
| \- recipientGroupingKey| String| X| Recipient grouping key|

## Cancel message sending

#### Requested

\[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| requestId| String| Request ID|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Query parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| recipientSeq| String| X| Recipient sequence number<br>(If you do not enter any information, all requests for that request ID will be canceled)|

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|----------|----------|:----------:|----------|
| header| Object| X| Header zone|
| \- resultCode| Integer| X| Result code|
| \- resultMessage| String| X| Result message|
| \- isSuccessful| Boolean| X| Success status|

\[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

## Manage Templates

### View template list

#### Requested

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Query parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| templateCode| String| X| Template Code|
| templateName| String| X| Template Name|
| status| String| X| Template status code|
| pageNum| Integer| X| Page number (Default: 1\)|
| pageSize| Integer| X| Number of views (default: 15, Max: 1,000)|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| templateListResponse| Object| O| Body area|
| \- templates| Array| O| Template list|
| \-- plusFriendId| String| O| Outgoing profile ID|
| \-- plusFriendType| String| O| Outgoing profile type|
| \-- templateCode| String| O| Template Code|
| \-- templateName| String| O| Template name|
| \-- chatBubbleType| String| O| Message type|
| \-- content| String| X| Message Content|
| \-- header| String| X| Header|
| \-- additionalContent| String| X| (refer to the description table)|
| \-- adult| boolean| O| Message status for adults|
| \-- image| Object| X| Image Information|
| \--- imageUrl| String| O| Image URL|
| \--- imageLink| String| X| Image link|
| \-- buttons| Array| X| Button list|
| \--- name| String| O| Button title|
| \--- type| String| O| Button type|
| \--- linkMo| String| X| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \--- bizFormId| Integer| X| BizForm ID (JSON based)|
| \-- item| Object| X| Wide list elements|
| \--- list| Array| X| Wide list|
| \---- title| String| X| Item title|
| \---- imageUrl| String| O| Item image URL|
| \---- linkMo| String| O| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \---- schemeAndroid| String| X| Android app link|
| \---- schemeIos| String| X| IOS app link|
| \-- coupon| Object| X| Coupon elements|
| \--- title| String| O| Coupon title|
| \--- description| String| O| Coupon details|
| \--- linkMo| String| X| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeAndroid| String| X| Android app link|
| \--- schemeIos| String| X| IOS app link|
| \-- commerce| Object| X| Commerce elements|
| \--- title| String| O| Product title|
| \--- regularPrice| Integer| X| Regular price|
| \--- discountPrice| Integer| X| Discount price|
| \--- discountRate| Integer| X| Discount rate|
| \--- discountFixed| Integer| X| Fixed discount price|
| \-- video| Object| X| Video elements|
| \--- videoUrl| String| O| KakaoTV video URL|
| \--- thumbnailUrl| String| X| Image URL for video thumbnail|
| \-- carousel| Object| X| Carousel|
| \--- head| Object| X| Carousel intro|
| \---- header| String| O| Carousel intro header|
| \---- content| String| O| Carousel intro content|
| \---- imageUrl| String| O| Carousel intro image address|
| \---- linkMo| String| X| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \----- schemeAndroid| String| X| Android app link|
| \----- schemeIos| String| X| IOS app link|
| \---- list| Array| O| Carousel list|
| \----- header| String| O| Carousel item header|
| \----- message| String| O| Carousel item message (content mapping of Not Null list)|
| \----- additionalContent| String| X| Additional Info|
| \----- imageUrl| String| O| Image URL (within carousel items)|
| \----- imageLink| String| X| Image link (within carousel items)|
| \----- commerce| Object| O| Commerce (within carousel items)|
| \------ title| String| O| Product title|
| \------ regularPrice| Integer| X| Regular price|
| \------ discountPrice| Integer| X| Discount price|
| \------ discountRate| Integer| X| Discount rate|
| \------ discountFixed| Integer| X| Fixed discount price|
| \----- buttons| Array| O| Carousel list button list|
| \------ name| String| O| Button title|
| \------ type| String| O| Button type|
| \------ linkMo| String| X| Mobile web link|
| \------ linkPc| String| X| PC web link|
| \------ schemeIos| String| X| IOS app link|
| \------ schemeAndroid| String| X| Android app link|
| \------ bizFormId| Integer| X| BizForm ID (JSON based)|
| \----- coupon| Object| X| Coupon elements (within carousel items)|
| \------ title| String| X| Coupon title|
| \------ description| String| X| Coupon details|
| \------ linkMo| String| X| Mobile web link|
| \------ linkPc| String| X| PC web link|
| \------ schemeAndroid| String| X| Android app link|
| \------ schemeIos| String| X| IOS app link|
| \---- tail| Object| X| More button information|
| \----- linkMo| String| O| Mobile web link|
| \----- linkPc| String| X| PC web link|
| \----- schemeAndroid| String| X| Android app link|
| \----- schemeIos| String| X| IOS app link|
| \--- status| String| O| Template status (A: registered, S: blocked)|
| \--- createDate| String| O| Registered on|
| \--- updateDate| String| X| Modified on|
| \- totalCount| Integer| O| Total|

### View single template

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|
| templateCode| String| Template Code|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| template| Object| O| Template body area|
| \- plusFriendId| String| O| Outgoing profile ID|
| \- plusFriendType| String| O| Outgoing profile type|
| \- templateCode| String| O| Template Code|
| \- templateName| String| O| Template name|
| \- chatBubbleType| String| O| Message type|
| \- pushAlarm| boolean| X| Push notification status|
| \- content| String| X| Message Content|
| \- adult| boolean| O| Message status for adults|
| \- header| String| X| Header (within template)|
| \- additionalContent| String| X| Additional info (within template)|
| \- image| Object| X| Image Information|
| \-- imageUrl| String| O| Image URL|
| \-- imageLink| String| X| Image link|
| \- buttons| Array| X| Button list|
| \-- name| String| O| Button title|
| \-- type| String| O| Button type|
| \-- linkMo| String| X| Mobile web link|
| \-- linkPc| String| X| PC web link|
| \-- schemeIos| String| X| IOS app link|
| \-- schemeAndroid| String| X| Android app link|
| \-- bizFormId| Integer| X| BizForm ID (JSON based)|
| \- item| Object| X| Wide list elements|
| \-- list| Array| X| Wide list|
| \--- title| String| X| Item title|
| \--- imageUrl| String| O| Item image URL|
| \--- linkMo| String| O| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \- coupon| Object| X| Coupon elements|
| \-- title| String| O| Coupon title|
| \-- description| String| O| Coupon details|
| \-- linkMo| String| X| Mobile web link|
| \-- linkPc| String| X| PC web link|
| \-- schemeIos| String| X| IOS app link|
| \-- schemeAndroid| String| X| Android app link|
| \- commerce| Object| X| Commerce elements|
| \-- title| String| O| Product title|
| \-- regularPrice| Integer| X| Regular price|
| \-- discountPrice| Integer| X| Discount price|
| \-- discountRate| Integer| X| Discount rate|
| \-- discountFixed| Integer| X| Fixed discount price|
| \- video| Object| X| Video elements|
| \-- videoUrl| String| O| KakaoTV video URL|
| \-- thumbnailUrl| String| X| Image URL for video thumbnail|
| \- carousel| Object| X| Carousel|
| \-- head| Object| X| Carousel intro|
| \--- header| String| O| Carousel intro header|
| \--- content| String| O| Carousel intro content|
| \--- imageUrl| String| O| Carousel intro image address|
| \--- linkMo| String| X| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \-- list| Array| O| Carousel list|
| \--- header| String| O| Carousel item header|
| \--- message| String| O| Carousel item message|
| \--- additionalContent| String| X| Additional Info (within carousel items)|
| \--- imageUrl| String| O| Image URL (within carousel items)|
| \--- imageLink| String| X| Image link (within carousel items)|
| \--- commerce| Object| O| Commerce (within carousel items)|
| \---- title| String| O| Product title|
| \---- regularPrice| Integer| X| Regular price|
| \---- discountPrice| Integer| X| Discount price|
| \---- discountRate| Integer| X| Discount rate|
| \---- discountFixed| Integer| X| Fixed discount price|
| \--- buttons| Array| O| Carousel list button list|
| \---- name| String| O| Button title|
| \---- type| String| O| Button type|
| \---- linkMo| String| X| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \---- schemeIos| String| X| IOS app link|
| \---- schemeAndroid| String| X| Android app link|
| \---- bizFormId| Integer| X| BizForm ID (JSON based)|
| \--- coupon| Object| X| Coupon elements (within carousel items)|
| \---- title| String| X| Coupon title|
| \---- description| String| X| Coupon details|
| \---- linkMo| String| X| Mobile web link|
| \---- linkPc| String| X| PC web link|
| \---- schemeIos| String| X| IOS app link|
| \---- schemeAndroid| String| X| Android app link|
| \-- tail| Object| X| More button information|
| \--- linkMo| String| O| Mobile web link|
| \--- linkPc| String| X| PC web link|
| \--- schemeIos| String| X| IOS app link|
| \--- schemeAndroid| String| X| Android app link|
| \- status| String| O| Template status (A: registered, S: blocked)|
| \- createDate| String| O| Registered on|
| \- updateDate| String| X| Modified on|

### Register Template

#### Requested

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Note

* If applying placeholders to coupon titles, you must use the following fixed placeholders:

```
- #{discount price} won discount coupon (#{discount price} scope is 1 ~ 99,999,999)
- #{discount rate}% discount coupon (#{discount rate} scope is 1 ~ 100)
- Delivery fee discount coupon
- #{product name} free coupon (#{product name} is up to 7 characters)
- #{product name} UP coupon (#{product name} is up to 7 characters)
```

* In commerce, users cannot specify placeholders for the regularPrice, discountPrice, discountRate, and discountFixed fields, except for the product title.
  * If you leave all fields except product title blank, the fixed placeholders will be automatically filled in and saved.
  * regularPrice -> #{regular price}
  * discountPrice -> #{discount price}
  * discountRate -> #{discount rate}
  * discountFixed -> #{fixed discount price}

#### Request to register text type template

\[Request body]

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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| templateName| String| O| Template name (up 200 characters)|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| O| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally, 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters|
| \- schemeIos| String| X| IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X| BizForm key when the button is BF type<br>No placeholders available|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register image type template

\[Request body]

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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| templateName| String| O        | Template name (up 200 characters)|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X        | Message status for adults (defaults: false)|
| content| String| O        | \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally, 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| image| Object| O        | Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O        | Image URL, use an image URL uploaded as a general image<br>No placeholders available|
| \- imageLink| String| X        | URL to go to when the image is clicked, limited to 500 characters<br>If not set, use the image viewer in KakaoTalk<br>No placeholders available|
| buttons| List| X        | Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters|
| \- schemeIos| String| X        | IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X        | BizForm key when the button is BF type|
| coupon| Object| X        | Coupon elements|
| \- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X        | IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register wide image type template

\[Request body]

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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| templateName| String| O        | Template name (up 200 characters)|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X        | Message status for adults (defaults: false)|
| content| String| O        | \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally, 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| image| Object| O        | Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O        | Image URL, use an image URL uploaded as a wide image<br>No placeholders available|
| \- imageLink| String| X        | URL to go to when the image is clicked, limited to 500 characters<br>If not set, use the image viewer in KakaoTalk<br>No placeholders available|
| buttons| List| X        | Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters|
| \- schemeIos| String| X        | IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X        | BizForm key when the button is BF type|
| coupon| Object| X        | Coupon elements|
| \- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X        | IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register wide item list type template

\[Request body]

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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| templateName| String| O| Template name (up to 200 characters)|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X| Message status for adults (defaults: false)|
| header| String| O| Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)<br>- Optional field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)|
| item| Object| O| Wide list item (available only for WIDE_ITEM_LIST type)|
| \- list| List| O| Wide list (minimum: 3, maximum 4)|
| \-- title| String| O| Item title<br>- The first item is limited to up to 25 characters (linebreak: up to 1; for the first item, title is not required)<br>- For the first item, title field is not required<br>- 2 to 4th items are limited to up to 30 characters (linebreak: up 1 item)|
| \-- imageUrl| String| O| Item image URL<br>- The first item uses the image URL uploaded as the first wide item list image.<br>- 2 to 4th items use the image URL uploaded as a general wide item list image<br>Placeholders unavailable|
| \-- linkMo| String| O| Mobile web link, limited to 500 characters|
| \-- linkPc| String| X| PC web link, limited to 500 characters|
| \-- schemeAndroid| String| X| Android app link, limited to 500 characters|
| \-- schemeIos| String| X| IOS app link, limited to 500 characters|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters|
| \- schemeIos| String| X| IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register premium video type template

\[Request body]

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

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| templateName| String| O| Template name (up 200 characters)|
| chatBubbleType| String| O| Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X| Message status for adults (defaults: false)|
| content| String| X| \- For TEXT type, up to 1,300 characters (linebreak: up to 99, URL type available)<br>- For IMAGE type, up to 400 characters (linebreak: up to 29, URL type available)<br>- For WIDE type, up to 76 characters (linebreak: up to 1)<br>- For PREMIUM_VIDEO type, the field can be used optionally, 76 characters (linebreak: up to 1)<br>- For other types, the field is unavailable.|
| header| String| X| Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)<br>- Optional field for WIDE_ITEM_LIST type, up to 20 characters (linebreak: unavailable)|
| video| Object| O| Video elements (only PREMIUM_VIDEO type available)|
| \- videoUrl| String| O| KakaoTV video URL (only video addresses uploaded on KakaoTV available), limited to up to 500 characters<br>Placeholders unavailable|
| \- thumbnailUrl| String| X| Image URL for video thumbnail. Only URLs uploaded as general images available (if none is available, the default KakaoTV video thumbnail is used). Limited to up to 500 characters<br>Placeholders unavailable|
| buttons| List| X| Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O| Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O| Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters|
| \- schemeIos| String| X| IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X| BizForm key when the button is BF type|
| coupon| Object| X| Coupon elements|
| \- title| String| O| Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O| Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X| Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X| PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X| Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X| IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register commerce type template

\[Request body]

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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| templateName| String| O        | Template name (up to 200 characters)|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| adult| boolean| X        | Message status for adults (defaults: false)|
| additionalContent| String| X        | Additional information (up to 34 characters, linebreak: up to 1), available only for commerce type|
| image| Object| O        | Image elements<br>- Required fields for IMAGE, WIDE, COMMERCE types|
| \- imageUrl| String| O        | Image URL, use an image URL uploaded as a general image<br>No placeholders available|
| \- imageLink| String| X        | URL to go to when the image is clicked, limited to 500 characters<br>If not set, use the image viewer in KakaoTalk<br>No placeholders available|
| commerce| Object| O        | Commerce (available only for COMMERCE type)|
| title| String| O        | Product title (up to 30 characters, linebreak: unavailable)|
| regularPrice| Integer| O        | Regular price (0 to 99,999,999)<br>Cannot customize placeholders, stored as a fixed placeholder `#{regular price}` if the value is left blank|
| discountPrice| Integer| X        | Discount price (0 to 99,999,999)<br>Cannot customize placeholders, stored as a fixed placeholder `#{discount price}` if the value is left blank|
| discountRate| Integer| X        | Discount rate (0 to 100), discount rate when a discount price exists, one of the fixed discount prices required<br>Cannot customize placeholders, stored as a fixed placeholder `#{discount rate}` if the value is left blank|
| discountFixed| Integer| X        | Fixed discount rate (0 to 999,999), discount rate when a discount price exists, one of the fixed discount prices required<br>Cannot customize placeholders, stored as a fixed placeholder `#{fixed discount price}` if the value is left blank|
| buttons| List| O        | Button list<br>- For TEXT, IMAGE type, up to 4 when applying a coupon; for others, up to 5<br>- For WIDE, WIDE_ITEM_LIST type, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1 and up to 2|
| \- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 1,000 characters|
| \- schemeIos| String| X        | IOS app link (required for AL type), limited to 500 characters|
| \- bizFormKey| String| X        | BizForm key when the button is BF type|
| coupon| Object| X        | Coupon elements|
| \- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- schemeIos| String| X        | IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|

#### Request to register carousel feed type template

\[Request body]

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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| templateName| String| O        | Template name (up to 200 characters)|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X        | Sending status for message push notifications (defaults: true)|
| adult| boolean| X        | Message status for adults (defaults: false)|
| carousel| Object| O        | Carousel|
| \- list| List| O        | Carousel list (minimum 2, maximum 6)|
| \-- header| String| O        | Carousel item title (up to 20 characters), available only for carousel feed type|
| \-- message| String| O        | Carousel item title (up to 20 characters), carousel item message (up to 180 characters), available only for carousel feed type|
| \-- imageUrl| String| O        | Image URL (use an image uploaded as a carousel feed type image<br>No placeholders available|
| \-- imageLink| String| X        | Image link, limited to 500 characters<br>No placeholders available|
| \-- buttons| List| O        | Carousel list button list, minimum 1, maximum 2|
| \--- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \--- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters|
| \--- schemeIos| String| X        | IOS app link (required for AL type), limited to 500 characters|
| \--- bizFormKey| String| X        | BizForm key when the button is BF type|
| \-- coupon| Object| X        | Coupon elements|
| \--- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \--- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- schemeIos| String| X        | IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- tail| Object| X        | More button information|
| \-- linkMo| String| O        | Mobile web link, limited to 500 characters<br>No placeholders available|
| \-- linkPc| String| X        | PC web link, limited to 500 characters<br>No placeholders available|
| \-- schemeAndroid| String| X        | Android app link, limited to 500 characters<br>No placeholders available|
| \-- schemeIos| String| X        | IOS app link, limited to 500 characters<br>No placeholders available|
| recipientList| List| O        | Recipient list (up to 1,000)|
| \- recipientNo| String| O        | Incoming number|
| createUser| String| X        | Registrant (stored as user UUID when sent from console)|

#### Request to register carousel commerce type template

\[Request body]

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

| Name| Type| Required | Description|
|----------|----------|----------|----------|
| templateName| String| O        | Template name (up to 200 characters)|
| chatBubbleType| String| O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)|
| pushAlarm| boolean| X        | Sending status for message push notifications (defaults: true)|
| adult| boolean| X        | Message status for adults (defaults: false)|
| carousel| Object| O        | Carousel|
| \- head| Object| X        | Carousel intro|
| \-- header| String| O        | Carousel intro header (up to 20 characters)|
| \-- content| String| O        | Carousel intro content (up to 50 characters)|
| \-- imageUrl| String| O        | Carousel intro image address (use an image uploaded as a carousel commerce type image; the used image should have the same image and ratio of the carousel.)<br>No placeholders available|
| \-- linkMo| String| X        | Mobile web link (linkMo is required if one of the linkMo, linkPc, schemeAndroid, schemeIos will be used), limited to up to 500 characters|
| \-- linkPc| String| X        | PC web link, limited to 500 characters|
| \-- schemeAndroid| String| X        | Android app link, limited to 500 characters|
| \-- schemeIos| String| X        | IOS app link, limited to 500 characters|
| \- list| List| O        | Carousel list (If head exists, minimum 1, maximum 5 / otherwise minimum 2, maximum 6)|
| \-- additionalContent| String| X        | Additional info (up to 34 characters), available only for carousel commerce|
| \-- imageUrl| String| O        | Image URL(use an image uploaded as a carousel commerce type image<br>No placeholders available|
| \-- imageLink| String| X        | Image link, limited to 500 characters<br>No placeholders available|
| \-- commerce| Object| O        | Commerce (available only for CAROUSEL_COMMERCE type)|
| \--- title| String| O        | Product title (up to 30 characters, linebreak: unavailable)|
| \--- regularPrice| Integer| O        | Regular price (0 to 99,999,999)<br>Cannot customize placeholders, stored as a fixed placeholder `#{regular price}` if the value is left blank|
| \--- discountPrice| Integer| X        | Discount price (0 to 99,999,999)<br>Cannot customize placeholders, stored as a fixed placeholder `#{discount price}` if the value is left blank|
| \--- discountRate| Integer| X        | Discount rate (0 to 100), discount rate when a discount price exists, one of the fixed discount prices required<br>Cannot customize placeholders, stored as a fixed placeholder `#{discount rate}` if the value is left blank|
| \--- discountFixed| Integer| X        | Fixed discount rate (0 to 999,999), discount rate when a discount price exists, one of the fixed discount prices required<br>Cannot customize placeholders, stored as a fixed placeholder `#{fixed discount price}` if the value is left blank|
| \-- buttons| List| O        | Carousel list button list, minimum 1, maximum 2|
| \--- name| String| O        | Button title<br>- For TEXT, IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>No placeholders available|
| \--- type| String| O        | Button type (WL: web link, AL: app link, BK: bot keyword, MD: forward message, AC: add channel, BT: chatbot conversion, BF: business form)<br>- AC type must be registered as the first button for TEXT and IMAGE, and as the last button for other message types|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters|
| \--- schemeIos| String| X        | IOS app link (required for AL type), limited to 500 characters|
| \--- bizFormKey| String| X        | BizForm key when the button is BF type|
| \-- coupon| Object| X        | Coupon elements|
| \--- title| String| O        | Limited to 5 formats for titles<br>- "${number} won discount coupon" number is 1 or greater and 99,999,999 or less<br>- "${number}% discount coupon" number is 1 or greater and 100 or less<br>- "Shipping discount coupon"<br>- "${within 7 characters} free coupon"<br>- "${within 7 characters} UP coupon"|
| \--- description| String| O        | Coupon details<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO type, up to 18, linebreak: unavailable<br>- For other types, up to 12 characters, linebreak: unavailable|
| \--- linkMo| String| X        | Mobile web link (required for WL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- linkPc| String| X        | PC web link (optional for WL type), limited to 500 characters|
| \--- schemeAndroid| String| X        | Android app link (required for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \--- schemeIos| String| X        | IOS app link (required field for AL type), limited to 500 characters<br>If entering linkMo field in the coupon, the remaining fields become optional.<br>If entering the channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.|
| \- tail| Object| X        | More button information|
| \-- linkMo| String| O        | Mobile web link, limited to 500 characters<br>No placeholders available|
| \-- linkPc| String| X        | PC web link, limited to 500 characters<br>No placeholders available|
| \-- schemeAndroid| String| X        | Android app link, limited to 500 characters<br>No placeholders available|
| \-- schemeIos| String| X        | IOS app link, limited to 500 characters<br>No placeholders available|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| template| Object| X| Template Info|
| \- templateCode| String| O| Template Code|

### Modify Template

#### Requested

\[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|
| templateCode| String| Template Code|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request Body]

* Same specifications as template registration

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

### Delete Template

#### Requested

\[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|
| templateCode| String| Template Code|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

## Manage Image

### Upload image

#### Requested

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| image| File| O| Image|
| imageType| String| O| Image type <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE)|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| image| Object| X| Image area|
| \- imageSeq| Integer| O| Image sequence|
| \- imageUrl| String| O| Image URL|
| \- imageName| String| X| Image Name|

#### Upload Image Specifications
| Image Type                 | Usage                                                                   | Upload Image Specifications                                                                                                                                                      |
| :------------------------- | :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IMAGE                      | Image-type image, Commerce-type image, Premium video-type thumbnail     | **Recommended size:** 800 × 400 px (width ≥ 500 px)<br/>**Aspect ratio:** 0.5 ≤ (height ÷ width) ≤ 1.333<br/>**File format & size limit:** JPG, PNG / up to 5 MB                 |
| WIDE_IMAGE                 | Wide image-type image                                                   | **Recommended size:** 800 × 600 px (width ≥ 500 px)<br/>**Aspect ratio:** 0.5 ≤ (height ÷ width) ≤ 1<br/>**File format & size limit:** JPG, PNG / up to 5 MB                     |
| MAIN_WIDE_ITEMLIST_IMAGE   | First item image in a wide item list                                    | **Minimum size:** width ≥ 500 px<br/>**Aspect ratio:** (height ÷ width) = 0.5<br/>**File format & size limit:** JPG, PNG / up to 5 MB                                            |
| NORMAL_WIDE_ITEMLIST_IMAGE | Item images #2–#4 in a wide item list                                   | **Minimum size:** width ≥ 500 px<br/>**Aspect ratio:** (height ÷ width) = 1<br/>**File format & size limit:** JPG, PNG / up to 5 MB each                                         |
| CAROUSEL_FEED_IMAGE        | Per-cell image in a carousel feed                                       | **Recommended size:** 800 × 600 px or 800 × 400 px (width ≥ 500 px)<br/>**Aspect ratio:** 0.5 ≤ (height ÷ width) ≤ 1.333<br/>**File format & size limit:** JPG, PNG / up to 5 MB |
| CAROUSEL_COMMERCE_IMAGE    | Intro image for carousel commerce, per-cell image for carousel commerce | **Recommended size:** 800 × 600 px or 800 × 400 px (width ≥ 500 px)<br/>**Aspect ratio:** 0.5 ≤ (height ÷ width) ≤ 1.333<br/>**File format & size limit:** JPG, PNG / up to 5 MB |


### View image

#### Requested

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| imageTypes| List| O| Image type <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE)|
| pageNum| String| X| Page number (default: 1\)|
| pageSize| String| X| Number of views (default: 15\)|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| image| Object| X| Image area|
| \- imageSeq| Integer| O| Image sequence|
| \- imageUrl| String| O| Image URL|
| \- imageName| String| X| Image Name|

### Delete Image

#### Requested

\[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Query parameter]

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| imageSeq| String| O| Image number|

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

## Upload

### Upload Bizform key

#### Requested

\[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

## Manage Outgoing Profiles

### View outgoing profile

#### Requested

\[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

#### Response

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

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|
| sender| Object| X| Outgoing Profile|
| \- plusFriendId| String| O| PlusFriend ID|
| \- senderKey| String| O| Outgoing Key|
| \- categoryCode| String| X| Category code|
| \- unsubscribePhoneNumber| String| X| Toll-free opt-out phone number|
| \- unsubscribeAuthNumber| String| X| toll-free opt-out verification number|
| \- status| String| X| NHN Cloud PlusFriend status code <br>(YSC02: pending registration, YSC03: normal registration)|
| \- statusName| String| X| NHN Cloud PlusFriend status name (pending registration, normal registration)|
| \- kakaoStatus| String| X| Kakao PlusFriend status code <br>(A: normal, S: blocked)<br>if status is YSC02, have kakaoStatus null value.|
| \- kakaoStatusName| String| X| Kakao PlusFriend status name (normal, blocked)<br>if status is YSC02, have kakaoStatusName null value.|
| \- kakaoProfileStatus| String| X| Kakao PlusFriend profile status code <br>(A: enabled, B: blocked, C: disabled, D:deleted E:deletion in progress)<br>if status is YSC02, have kakaoProfileStatus null value.|
| \- kakaoProfileStatusName| String| X| Kakao PlusFriend profile status name (enabled, disabled, blocked, deletion in progress, deleted)<br>if status is YSC02, have kakaoProfileStatusName null value.|
| \- profileSpamLevel| String| X| KakaoTalk channel spam status name (permanent restriction, warning restriction, normal)<br>It may have a null value if the outgoing profile status is not normal.|
| \- profileMessageSpamLevel| String| X| KakaoTalk Message spam status name (activity restriction, warning restriction, normal)<br>It may have a null value if the outgoing profile status is not normal.|
| \- block| boolean| O| Outgoing profile blocked status|
| \- brandMessage| Object| X| Brand Message settings information|
| \-- resendAppKey| String| X| SMS service appkey to be set as fallback|
| \-- isResend| boolean| O| Fallback settings (resending) status|
| \-- resendSendNo| String| X| If resending, tc-sms outgoing number|
| \-- resendUnsubscribeNo| String| X| If resending, tc-sms 080 opt-out number|
| \- dormant| boolean| O| Outgoing profile inactive status|
| \- marketingAgreement| boolean| O| M/N-type use application status|
| \- createDate| String| X| Registered Date|
| \- initialUserRestriction| boolean| O| First-time user restriction status|

### Modify outgoing profile 080 opt-out number

#### Requested

\[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|
| senderKey| String| Outgoing Key|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| unsubscribeNo| String| O| 080 toll-free opt-out phone number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx|
| unsubscribeAuthNo| String| X| 080 toll-free opt-out verification number (If not entered, the message will be sent using the free opt-out information registered in the outgoing profile)<br>unsubscribe_auth_number cannot be entered without unsubscribe_phone_number<br>ex) 1234|

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

## Manage Fallback

### Register SMS AppKey

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request body]

```
{
    "resendAppKey": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| resendAppKey| String| O| SMS service appkey to be set as fallback|

\[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

### Register fallback settings

\[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

\[Path parameter]

| Name| Type| Description|
|----------|----------|----------|
| appkey| String| Unique appkey|

\[Header]

```
{
  "X-Secret-Key": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| X-Secret-Key| String| O| Available to create from the console.|

\[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| Name| Type| Required| Description|
|----------|----------|----------|----------|
| senderKey| String| O| Outgoing Key|
| isResend| Boolean| O| Fallback text status if sending fails<br>When setting up fallbacks in the console, fallbacks will be sent by default.|
| resendSendNo| String| X| Fallback outgoing number<br><span style="color:red">(if the outgoing number is not registered with the SMS product, the fallback may fail.)</span>|
| resendUnsubscribeNo| String| X| Fallback 080 opt-out number<br><span style="color:red">(if the 080 opt-out number is not registered with the SMS service the fallback may fail.)</span>|

\[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name| Type| Not Null| Description|
|:----------|:----------|:----------|:----------|
| header| Object| O| Header zone|
| \- resultCode| Integer| O| Result code|
| \- resultMessage| String| O| Result message|
| \- isSuccessful| boolean| O| Success status|

