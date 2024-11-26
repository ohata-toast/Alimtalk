## Notification > KakaoTalk Bizmessage > FriendTalk > API v2.4 Guide

## FriendTalk

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

## Overview of v2.4 API
1. Added FriendTalk commerce, carousel commerce, premium video, and adult messaging settings.
2. Added the Carousel Commerce image registration API.

## Send Messages

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | Request to send duplicate messages by key<br>If a request is made with the same key for 10 minutes, the request will be failed. |

* <b>The request date can be set from the time you make the call to 60 days later.</b>
* <b> Delivery restricted during night (20:50~08:00 on the following day)</b>
* <b> Delivery is to be replaced by SMS, and field input must follow delivery API specifications of the SMS service (e.g. sender number registered at SMS service, 080 unsubscription, and field length restrictions) </b>
* <b> Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>
* <b>FriendTalk ad messages are sent using the Ads SMS API instead, so you must register a 080 opt-out number.</b>
* <b>When entering the resendContent field of a FriendTalk ad message, you must enter the <span style="color:red">ad text</span>from the SMS ad API to send it. `(Ad) content[Free unsubscribe]080XXXXXXX`</b>
* <b> When the resendContent field of a FriendTalk ad message is missing, ad phrase is automatically created with registered 080 number for unsubscription to enable alternative delivery. </b>
* <b>Wide item list type and carousel feed type can only send ads.</b>
* <b>If you enter the linkMo field in the coupon, the remaining fields become optional, and if you enter the channel coupon URL (format: alimtalk=coupon://)) in the scheme_android or scheme_ios field, the remaining fields become optional.</b>

#### Text type sending request

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
            "isResend": boolean,
            "resendType": String,
            "resendTitle": String,
            "resendContent": String,
            "resendSendNo": String,
            "resendUnsubscribeNo": String
        },
        "isAd": Boolean,
        "recipientGroupingKey": String
    }],
    "statsId": String
}
```

| Name |	Type|	Required| 	Description                                                                                                                                                          |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
|senderKey|	String|	O | Sender key (40 characters)                                                                                                                                                    |
|requestDate|	String|	X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                                |
|senderGroupingKey| String | X| Sender's grouping key (up to 100 characters)                                                                                                                                            |
| createUser | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                                  |
|recipientList|	List|	O| 	Recipient lists (up to 1,000 recipients)                                                                                                                                            |
|- recipientNo|	String|	O| 	Recipient number                                                                                                                                                       |
|- content|	String|	O| Body message (up to 1,000 characters)<br>Up to 400 characters when sending images<br>Up to 76 characters when sending wide images                                                                                                    |
|- buttons|	List|	X| 	Buttons (up to 5, up to 4 if a coupon is included)<br>Up to 2 link buttons when sending wide images                                                                                                    |
|-- ordering|	Integer|	X | 	Button sequence (required, if there is a button)                                                                                                                                         |
|-- type| String |	X | 	Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)                                                                                                      |
|-- name| String |	X | 	Button name (required, if there is a button)                                                                                                                                         |
|-- linkMo| String |	X | 	Mobile web link (required for the WL type)                                                                                                                                   |
|-- linkPc | String |	X | PC web link (optional for the WL type)                                                                                                                                     |
|-- schemeIos | String | X | 	iOS app link (required for the AL type)                                                                                                                                   |
|-- schemeAndroid | String | X | 	Android app link (required field if AL type)                                                                                                                                 |
|-- chatExtra|	String|	X| Meta information to pass if it's a BC (Convert Chat) / BT (Convert Bot) type button                                                                                                                      |
|-- chatEvent|	String|	X| The name of the bot event to connect to if it's a BT (Bot Toggle) type button                                                                                                                                 |
|-- bizFormKey|	String|	X| The Bizform key for business form (BF) type buttons                                                                                                                                    |
|-- target|	String|	X | 	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                                   |
|- coupon | Object | X | Coupon                                                                                                                                                           | 
|-- title| String |	X | 	Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
|-- description| String |	X | 	Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
|-- linkMo| String |	X | 	Mobile web link (see conditions below)                                                                                                                                      |
|-- linkPc | String |	X | PC web link                                                                                                                                                      |
|-- schemeIos | String | X | 	iOS app link                                                                                                                                                    |
|-- schemeAndroid | String | X | 	Android app link                                                                                                                                                  |
|- resendParameter|	Object|	X| Alternative delivery information                                                                                                                                                     |
|-- isResend|	boolean|	X| 	Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                       |
|-- resendType|	String|	X| 	Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                     |
|-- resendTitle|	String|	X| 	Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                               |
|-- resendContent|	String|	X| 	Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                         |
|-- resendSendNo | String| X| Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                                |
|-- resendUnsubscribeNo | String| X| Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                  |
|- isAd | Boolean | X | 	Ad or not (default is true)                                                                                                                                             |
|- recipientGroupingKey|	String|	X| 	Recipient's grouping key (up to 100 characters)                                                                                                                                          |
| statsId | String |	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                           |

#### Image / Wide Image type sending request

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

| Name                     |	Type|	Required| 	Description                                                                                                                                                          |
|------------------------|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              |	String|	O | Sender key (40 characters)                                                                                                                                                    |
| requestDate            |	String|	X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                                |
| senderGroupingKey      | String | X| Sender's grouping key (up to 100 characters)                                                                                                                                            |
| createUser             | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                                  |
| recipientList          |	List|	O| 	Recipient lists (up to 1,000 recipients)                                                                                                                                            |
| - recipientNo          |	String|	O| 	Recipient number                                                                                                                                                       |
| - content              |	String|	O| Body message (up to 1,000 characters)<br>Up to 400, if image is included<br>Up to 76, if wide image is included                                                                                                    |
| - imageUrl             |	String|	O| 	Image URL                                                                                                                                                     |
| - imageLink            |	String|	X| 	Image link                                                                                                                                                      |
| - buttons              |	List|	X| 	Buttons (up to 5, up to 4 if a coupon is included)<br>Up to 2 link buttons when sending wide images                                                                                                                    |
| -- ordering            |	Integer|	X | 	Button sequence (required, if there is a button)                                                                                                                                         |
| -- type                | String |	X | 	Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)
| -- name                | String |	X | 	Button name (required, if there is a button)                                                                                                                                         |
| -- linkMo              | String |	X | 	Mobile web link (required for the WL type)                                                                                                                                   |
| -- linkPc              | String |	X | PC web link (optional for the WL type)                                                                                                                                     |
| -- schemeIos           | String | X | 	iOS app link (required for the AL type)                                                                                                                                   |
| -- schemeAndroid       | String | X | 	Android app link (required field if AL type)                                                                                                                                 |
| -- chatExtra           |	String|	X| Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons   
| -- chatEvent           |	String|	X| Bot event name to connect for BT (Bot Transfer) type button                                                                                                                                 |
| -- bizFormKey          |	String|	X| Bizform key for BF (Business Form) type buttons                                                                                                                                    |
| -- target              |	String|	X | 	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                                   |
| - coupon               | Object | X | Coupon                                                                                                                                                           | 
| -- title               | String |	X | 	Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| -- description         | String |	X | 	Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
| -- linkMo              | String |	X | 	Mobile web link (see conditions below)                                                                                                                                      |
| -- linkPc              | String |	X | PC web link                                                                                                                                                      |
| -- schemeIos           | String | X | 	iOS app link                                                                                                                                                    |
| -- schemeAndroid       | String | X | 	Android app link                                                                                                                                                  |
| - resendParameter      |	Object|	X| Alternative delivery information                                                                                                                                                     |
| -- isResend            |	boolean|	X| 	Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                       |
| -- resendType          |	String|	X| 	Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                     |
| -- resendTitle         |	String|	X| 	Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                               |
| -- resendContent       |	String|	X| 	Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                         |
| -- resendSendNo        | String| X| Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                                |
| -- resendUnsubscribeNo | String| X| Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                  |
| - isAd                 | Boolean | X | 	Ad or not (default is true)                                                                                                                                             |
| - adult                | Boolean | X | Whether messages are for adults (default is false)                                                                                                                           |
| - recipientGroupingKey |	String|	X| 	Recipient's grouping key (up to 100 characters)                                                                                                                                          |
| statsId                | String |	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                           |

#### Wide item list type sending request

[Request Body]
```
{
    "senderKey": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
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
| Name                     | 	Type  | 	Required | 	Description                                                                                                                                             |
|------------------------|------|---|-------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | Sender key (40 characters)                                                                                                                                       |
| requestDate            | String | X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                   |
| senderGroupingKey      | String | X | Sender's grouping key (up to 100 characters)                                                                                                                               |
| createUser             | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                     |
| recipientList          | List | 	O | Recipient lists (up to 1,000 recipients)                                                                                                                                |
| - recipientNo          | String | O | Recipient number                                                                                                                                           |
| - buttons              | List | 	X | Button<br>Up to 2 link buttons when sending wide images                                                                                                                 |
| -- ordering            | Integer | X | Button sequence (required, if there is a button)                                                                                                                             |
| -- type                | String | X | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)                                                                                          |
| -- name                | String | X | Button name (required, if there is a button)                                                                                                                             |
| -- linkMo              | String | X | Mobile web link (required for the WL type)                                                                                                                       |
| -- linkPc              | String | X | PC web link (optional for the WL type)                                                                                                                        |
| -- schemeIos           | String | X | iOS app link (required for the AL type)                                                                                                                       |
| -- schemeAndroid       | String | X | Android app link (required field if AL type)                                                                                                                     |
| -- chatExtra           | String | X | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                         |
| -- chatEvent           | String | X | Bot event name to connect for BT (Bot Transfer) type button                                                                                                                    |
| -- bizFormKey          | String | X | Bizform key for BF (Business Form) type buttons                                                                                                                       |
| -- target              | String | X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                       |
| - header               | String | O | Header (required when using the wide item list message type, up to 25 characters)                                                                                                          |
| - item                 | Object | O | Wide item                                                                                                                                         |
| -- list                | List | O | Wide item list (at lease 3, up to 4)                                                                                                                         |
| --- title              | String | O | Item title (up to 25 characters for the first item, up to 30 characters for items 2-4)                                                                                                |
| --- imageUrl           | String | O | Item image URL                                                                                                                                     |
| --- linkMo             | String | O | Mobile web link                                                                                                                                        |
| --- linkPc             | String | X | PC web link                                                                                                                                         |
| --- schemeIos          | String | X | iOS app link                                                                                                                                        |
| --- schemeAndroid      | String | X | Android app link                                                                                                                                      |
| - coupon               | Object | X | Coupon                                                                                                                                              | 
| -- title               | String | X | Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| -- description         | String | X | Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                          |
| -- linkMo              | String | X | Mobile web link (see conditions below)                                                                                                                          |
| -- linkPc              | String | X | PC web link                                                                                                                                         |
| -- schemeIos           | String | X | iOS app link                                                                                                                                        |
| -- schemeAndroid       | String | X | Android app link                                                                                                                                      |
| - resendParameter      | Object | X | Alternative delivery information                                                                                                                                        |
| -- isResend            | boolean | X | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                           |
| -- resendType          | String | X | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                         |
| -- resendTitle         | String | X | Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                   |
| -- resendContent       | String | X | Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                             |
| -- resendSendNo        | String | X | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                   |
| -- resendUnsubscribeNo | String | X | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                     |
| - isAd                 | Boolean | X | Ad or not (default is true)                                                                                                                                 |
| - adult                | Boolean | X | Whether messages are for adults (default is false)                                                                                                                           |
| - recipientGroupingKey | String | 	X | Recipient's grouping key (up to 100 characters)                                                                                                                              |
| statsId                | String | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                              |


#### Carousel feed type sending request

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

| Name                     | 	Type   | 	Required | 	Description                                                                                                                                                         |
|------------------------|-------|--|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | Sender key (40 characters)                                                                                                                                                   |
| requestDate            | String | X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                               |
| senderGroupingKey      | String | X | Sender's grouping key (up to 100 characters)                                                                                                                                           |
| createUser             | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                                 |
| recipientList          | List  | O | Recipient lists (up to 1,000 recipients)                                                                                                                                            |
| - recipientNo          | String | O | Recipient number                                                                                                                                                       |
| - carousel             | Object | O | Carousel                                                                                                                                                         
| -- list                | List  | O | Carousel lists (minimum 2, maximum 10)                                                                                                                                       | 
| --- header             | String | O | Carousel item title (up to 20 characters), only available for carousel feeds                                                                                                                        | 
| --- message            | String | O | Carousel item message (up to 180 characters), only available for carousel feeds                                                                                                                      | 
| --- attachment         | Object | O | Carousel item images, button information                                                                                                                                          | 
| ---- buttons           | List  | X | Button list (up to 2)                                                                                                                                               | 
| ----- name             | String | X | Button name (required if you have a button, up to 8 characters)                                                                                                                                  |
| ----- type             | String | X | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)
| ----- linkMo           | String | X | Mobile web link (required for the WL type)                                                                                                                                   |
| ----- linkPc           | String | X | PC web link (optional for the WL type)                                                                                                                                    |
| ----- schemeIos        | String | X | iOS app link (required for the AL type)                                                                                                                                   |
| ----- schemeAndroid    | String | X | Android app link (required field if AL type)                                                                                                                                 |
| ---- image             | Object | O |                                                                                                                                                             | 
| ----- imageUrl         | String | O | Image URL (only available for carousel-feed images, not carousel commerce images)                                                                                                          |
| ----- imageLink        | String | X | Image link                                                                                                                                                      |
| ---- coupon            | Object | X | Coupon                                                                                                                                                          | 
| ----- title            | String | X | Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| ----- description      | String | X | Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
| ----- linkMo           | String | X | Mobile web link (see conditions below)                                                                                                                                      |
| ----- linkPc           | String | X | PC web link                                                                                                                                                     |
| ----- schemeIos        | String | X | iOS app link                                                                                                                                                    |
| ----- schemeAndroid    | String | X | Android app link                                                                                                                                                  |
| -- tail                | Object | X | Learn more button information                                                                                                                                                   | 
| --- linkMo             | String | O | Mobile web link                                                                                                                                                    |
| --- linkPc             | String | X | PC web link                                                                                                                                                     |
| --- schemeIos          | String | X | iOS app link                                                                                                                                                    |
| --- schemeAndroid      | String | X | Android app link                                                                                                                                                  |
| - resendParameter      | Object | X | Alternative delivery information                                                                                                                                                    |
| -- isResend            | boolean |X | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                       |
| -- resendType          | String | X | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                     |
| -- resendTitle         | String | X | Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                               |
| -- resendContent       | String | X | Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                         |
| -- resendSendNo        | String | X | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                               |
| -- resendUnsubscribeNo | String | X | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                 |
| - isAd                 | Boolean | O | Ads or not (carousel feeds can only send ad types)                                                                                                                                |
| - adult                | Boolean | X | Whether messages are for adults (default is false)                                                                                                                                       |
| - recipientGroupingKey | String | X | Recipient's grouping key (up to 100 characters)                                                                                                                                          |
| statsId                | String | X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                          |

#### Carousel commerce type sending request

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

| Name     | 	Type   | 	Required | 	Description                                                                                                                                                         |
|--------|-------|--|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | Sender key (40 characters)                                                                                                                                                   |
| requestDate | String | X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                               |
| senderGroupingKey | String | X | Sender's grouping key (up to 100 characters)                                                                                                                                           |
| createUser | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                                 |
| recipientList | List  | O | Recipient lists (up to 1,000 recipients)                                                                                                                                            |
| - recipientNo | String | 	O | Recipient number                                                                                                                                                       |
| - carousel | Object | O | Carousel                                                                                                                                                         
| -- head | String | X | Carousel intro info (available only for carousel commerce)                                                                                                                              | 
| --- header | String | O | Carousel intro header (up to 20 characters)                                                                                                                                          |  
| --- content | String | O | Carousel intro content (up to 50 characters)                                                                                                                                          |
| --- imageUrl | String | O | The address of the carousel intro image (the image used must have the same proportions as the image in the carousel).                                                                                                           |
| --- linkMo | String | X | Web link to go to on intro click in mobile                                                                                                                                   |
| --- linkPc | String | X | Web link to go to on intro click in PC                                                                                                                                    |
| --- schemeIos | String | X | 	App link to go to on intro click in iOS                                                                                                                                  |
| --- schemeAndroid | String | X | 	App link to go to on intro click in Android                                                                                                                                |
| -- list | List  | O | Carousel lists (minimum 2, maximum 10)                                                                                                                                       | 
| --- additionalContent | String | X | Additional information (up to 34 characters), only available for carousel commerce types                                                                                                                            |  
| --- attachment | Object | O | Carousel item images, button information                                                                                                                                          | 
| ---- buttons | List  | X | Button list (up to 2)                                                                                                                                               | 
| ----- name | String | X | Button name (required if you have a button, up to 8 characters)                                                                                                                                  |
| ----- type | String | X | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)
| ----- linkMo | String |X | Mobile web link (required for the WL type)                                                                                                                                   |
| ----- linkPc | String | X | PC web link (optional for the WL type)                                                                                                                                    |
| ----- schemeIos | String | X | iOS app link (required for the AL type)                                                                                                                                   |
| ----- schemeAndroid | String | X | Android app link (required field if AL type)                                                                                                                                 |
| ---- image | Object | O |                                                                                                                                                             | 
| ----- imageUrl | String | O | 	Image URL (Only carousel commerce images; the image used must have the same proportions as the other carousel commerce images and the image in the intro).                                                                        |
| ----- imageLink | String | X | 	Image link                                                                                                                                                     |
| ---- coupon | Object | X | Coupon                                                                                                                                                          | 
| ----- title | String | X | Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| ----- description | String |X | Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
| ----- linkMo | String | X | Mobile web link (see conditions below)                                                                                                                                      |
| ----- linkPc | String | X | PC web link                                                                                                                                                     |
| ----- schemeIos | String | X | iOS app link                                                                                                                                                    |
| ----- schemeAndroid | String | X | Android app link                                                                                                                                                  |
| ---- commerce | Object | O | Commerce (available only for Carousel Commerce)                                                                                                                                      | 
| ----- title | String | O | Product title (up to 30 characters)                                                                                                                                               |
| ----- regularPrice | Integer | O | Regular price (0 to 99,999,999)                                                                                                                                        |
| ----- discountPrice | Integer | X | Discount Price (0 to 99,999,999)                                                                                                                                        |
| ----- discountRate | Integer | X | A discount rate (0 to 100), discount rate if a discount price exists, or flat discount price required                                                                                                                |
| ----- discountFixed | Integer | X | 	a flat discount price (0 to 999,999), a discount percentage if a discount price exists, or a flat discount price required                                                                                                        |
| -- tail | Object | X | Learn more button information                                                                                                                                                   | 
| --- linkMo | String | O | Mobile web link                                                                                                                                                    |
| --- linkPc | String | X | PC web link                                                                                                                                                     |
| --- schemeIos | String | X | iOS app link                                                                                                                                                    |
| --- schemeAndroid | String | X | Android app link                                                                                                                                                  |
| - resendParameter | Object | X | Alternative delivery information                                                                                                                                                    |
| -- isResend | boolean | X | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                       |
| -- resendType | String | X | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                     |
| -- resendTitle | String | X | Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                               |
| -- resendContent | String | X | Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                         |
| -- resendSendNo | String | X | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                               |
| -- resendUnsubscribeNo | String | X | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                 |
| - isAd | Boolean | O | Ads or not (carousel commerce types can only send ad types)                                                                                                                               |
| - adult | Boolean | X | Whether messages are for adults (default is false)                                                                                                                                       |
| - recipientGroupingKey | 	String | X | 	Recipient's grouping key (up to 100 characters)                                                                                                                                         |
| statsId | String | X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                          |

* All images used in carousel commerce must have the same proportions.

#### Premium video type sending request

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

| Name                     | 	Type  | 	Required | 	Description                                                                                                                                                          |
|------------------------|------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String | O | Sender key (40 characters)                                                                                                                                                    |
| requestDate            | String | X | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                                 |
| senderGroupingKey      | String | X | Sender's grouping key (up to 100 characters)                                                                                                                                            |
| createUser             | String | X | Registrant (saved as user UUID when sending from console)                                                                                                                                  |
| recipientList          | List | O | Recipient lists (up to 1,000 recipients)                                                                                                                                            |
| - recipientNo          | String | O | Recipient number                                                                                                                                                        |
| - content              | String | O | Body message (up to 1,000 characters)<br>Up to 400, if image is included<br>Up to 76, if wide image is included                                                                                                   |
| - header               | String | O | Header (optional, up to 25 characters when using premium video types)                                                                                                                             |
| - buttons              | List | X | Buttons (up to 5, up to 4 if a coupon is included)<br>Up to 2 link buttons when sending wide images                                                                                                     |
| -- ordering            | Integer | X | Button sequence (required, if there is a button)                                                                                                                                          |
| -- type                | String | X | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)                                                                                                  
| -- name                | String | X | Button name (required, if there is a button)                                                                                                                                          |
| -- linkMo              | String | X | Mobile web link (required for the WL type)                                                                                                                                    |
| -- linkPc              | String | X | PC web link (optional for the WL type)                                                                                                                                     |
| -- schemeIos           | String | X | iOS app link (required for the AL type)                                                                                                                                    |
| -- schemeAndroid       | String | X | Android app link (required field if AL type)                                                                                                                                  |
| -- chatExtra           | String | X | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                                       |
| -- chatEvent           | String | X | Bot event name to connect for BT (Bot Transfer) type button                                                                                                                                  |
| -- bizFormKey          | String | X | Bizform key for BF (Business Form) type buttons                                                                                                                                     |
| -- target              | String | X | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                                     |
| - coupon               | Object | X | Coupon                                                                                                                                                           | 
| -- title               | String | X | Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| -- description         | String | X | Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
| -- linkMo              | String | X | Mobile web link (see conditions below)                                                                                                                                        |
| -- linkPc              | String | X | PC web link                                                                                                                                                      |
| -- schemeIos           | String | X | iOS app link                                                                                                                                                     |
| -- schemeAndroid       | String | X | Android app link                                                                                                                                                   |
| - video                | Object | O | Video                                                                                                                                                          | 
| -- videoUrl            | String | O | Kakao TV video URL (only available for video addresses uploaded to Kakao TV)                                                                                                                    |
| -- thumbnailUrl        | String | X | Image URL for video thumbnail, only available if uploaded as a regular image, otherwise use KakaoTV video default thumbnail                                                                                         |
| - resendParameter      | Object | X | Alternative delivery information                                                                                                                                                     |
| -- isResend            | boolean | X | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                          |
| -- resendType          | String | X | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                      |
| -- resendTitle         | String | X | Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                 |
| -- resendContent       | String | X | Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                           |
| -- resendSendNo        | String | X | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                                 |
| -- resendUnsubscribeNo | String | X | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                   |
| - isAd                 | Boolean | X | Ad or not (default is true)                                                                                                                                              |
| - adult                | Boolean | X | Whether messages are for adults (default is false)                                                                                                                                        |
| - recipientGroupingKey | 	String | X | Recipient's grouping key (up to 100 characters)                                                                                                                                           |
| statsId                | String | X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                           |

#### Commerce type sending request

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

| Name                   | 	Type  | 	Required | 	Description                                                                                                                                                         |
|----------------------|------|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey            | String | O   | Sender key (40 characters)                                                                                                                                                   |
| requestDate          | String | X   | Date and time of request (yyyy-MM-dd HH:mm), to be sent immediately if field is not sent                                                                                                               |
| senderGroupingKey    | String | X   | Sender's grouping key (up to 100 characters)                                                                                                                                           |
| createUser           | String | X   | Registrant (saved as user UUID when sending from console)                                                                                                                                 |
| recipientList        | List | 	O  | Recipient lists (up to 1,000 recipients)                                                                                                                                            |
| - recipientNo        | String | O   | Recipient number                                                                                                                                                       |
| - additionalContent | String | X   | Additional information (up to 34 characters), available for commerce only                                                                                                                            |
| - buttons            | List | X   | 	Buttons (up to 5, up to 4 if a coupon is included)<br>Up to 2 link buttons when sending wide images                                                                                                   |
| -- ordering          | Integer | X   | Button sequence (required, if there is a button)                                                                                                                                         |
| -- type              | String | X   | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)
| -- name              | String | X   | Button name (required, if there is a button)                                                                                                                                         |
| -- linkMo            | String | X   | Mobile web link (required for the WL type)                                                                                                                                   |
| -- linkPc            | String | X   | PC web link (optional for the WL type)                                                                                                                                    |
| -- schemeIos         | String | X   | iOS app link (required for the AL type)                                                                                                                                   |
| -- schemeAndroid     | String | X   | Android app link (required field if AL type)                                                                                                                                 |
| -- chatExtra         | String | X   | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                                     |
| -- chatEvent         | String | X   | Bot event name to connect for BT (Bot Transfer) type button                                                                                                                                |
| -- bizFormKey        | String | X   | Bizform key for BF (Business Form) type buttons                                                                                                                                   |
| -- target            | String | X   | In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link                                                                                                   |
| - coupon             | Object | X   | Coupon                                                                                                                                                          | 
| -- title             | String | X   | Limited to 5 formats for title<br>"${number} won off coupon" number should be 1 or greater than or equal to 99,999,999<br>"${number}% off coupon" number is at least 1 and no more than 100<br>"Shipping discount coupon"<br><br>"${7 characters or less} free coupon"<br>"${7 characters or less} UP coupon" |
| -- description       | String | X   | Coupon description (up to 12 characters for plain text, image, carousel feed / up to 18 characters for wide image, wide item list)                                                                                      |
| -- linkMo            | String | X   | Mobile web link (see conditions below)                                                                                                                                      |
| -- linkPc            | String | X   | PC web link                                                                                                                                                     |
| -- schemeIos         | String | X   | iOS app link                                                                                                                                                    |
| -- schemeAndroid     | String | X   | Android app link                                                                                                                                                  |
| - commerce           | Object | O   | Commerce (available only for Commerce)                                                                                                                                          | 
| -- title             | String | O   | Product title (up to 30 characters)                                                                                                                                                |
| -- regularPrice      | Integer | O   | Regular price (0 to 99,999,999)                                                                                                                                        |
| -- discountPrice     | Integer | X   | Discount Price (0 to 99,999,999)                                                                                                                                        |
| -- discountRate      | Integer | X   | A discount rate (0 to 100), discount rate if a discount price exists, or flat discount price required                                                                                                                 |
| -- discountFixed     | Integer | X   | a flat discount price (0 to 999,999), a discount percentage if a discount price exists, or a flat discount price required                                                                                                          |
| - resendParameter    | Object | X   | Alternative delivery information                                                                                                                                                    |
| -- isResend          | boolean | X   | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console.                                                                                                       |
| -- resendType        | String | X   | Alternative delivery type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                     |
| -- resendTitle       | String | X   | Title of alternative delivery for LMS<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                               |
| -- resendContent     | String | X   | Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                         |
| -- resendSendNo      | String | X   | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span>                                                               |
| -- resendUnsubscribeNo | String | X   | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span>                                                 |
| - isAd               | Boolean | X   | 	Ad or not (default is true)                                                                                                                                            |
| - adult              | Boolean | X   | Whether messages are for adults (default is false)                                                                                                                                       |
| - recipientGroupingKey | String | 	X  | 	Recipient's grouping key (up to 100 characters)                                                                                                                                         |
| statsId              | String | 	X  | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                          |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages -d '{"senderKey":"9e0afe2c12aaaaaaaaaa7520052880b555f1a60a","requestDate":"yyyy-MM-dd HH:mm","recipientList":[{"recipientNo":"010-0000-0000","imageSeq":1,"imageLink":"https://toast.com","content":"Content","buttons":[{"ordering":1,"type":"WL","name":"Button1","linkMo":"https://toast.com","linkPc":"https://toast.com"}]}]}]}'
```

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|message|	Object|	Body area|
|- requestId | String |	Request ID |
|- senderGroupingKey | String |	Sender's grouping key |
|- sendResults | Object | Result of delivery request |
|-- recipientSeq | Integer | Recipient's sequence number |
|-- recipientNo | String | Recipient number |
|-- resultCode | Integer | Result code of delivery request |
|-- resultMessage | String | Result message of delivery request |
|-- recipientGroupingKey | String | Recipient's grouping key |

## List Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter] No.1 or (2, 3) is conditionally required

| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|requestId|	String|	Conditionally required (no.1) | Request ID |
|startRequestDate|	String|	Conditionally required (no.2) | Start date of delivery request (yyyy-MM-dd HH:mm)|
|endRequestDate|	String| Conditionally required (no.2) |	End date of delivery request (yyyy-MM-dd HH:mm) |
|startCreateDate| String| Conditionally required (no.3) | Registration date start value (yyyy-MM-dd HH:mm)|
|endCreateDate|  String| Conditionally required (no.3) |  Registration date end value (yyyy-MM-dd HH:mm) |
|recipientNo|	String|	X |	Recipient number |
|senderKey|	String|	X |	Sender Key |
|senderGroupingKey| String | X| Sender's grouping key |
|recipientGroupingKey|	String|	X|	Recipient's grouping key |
|messageStatus| String |	X | Request status (COMPLETED: successful, FAILED: failed)	|
|resultCode| String |	X | Delivery result (MRC01: successful, MRC02: failed)	|
|createUser| String | X | Registrant (saved as user UUID when sending from console) |
|pageNum|	Integer|	X|	Page number (Default: 1)|
|pageSize|	Integer|	X|	Number of queries (default: 15, max: 1000)|

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|messageSearchResultResponse|	Object|	Body area|
|- messages | List |	List of messages |
|-- requestId | String |	Request ID |
|-- recipientSeq | Integer |	Recipient's sequence number |
|-- plusFriendId | String |	Plus Friend ID |
|-- senderKey   | String | Sender Key   |
|-- recipientNo | String |	Recipient number |
|-- requestDate | String |	Date and time of request |
|-- createDate | String | Registered date and time |
|-- receiveDate | String |	Date and time of receiving |
|-- content | String |	Body |
|-- messageStatus | String |	Request status (COMPLETED: successful, FAILED: failed) |
|-- resendStatus | String |	Status code of resending |
|-- resendStatusName | String |	Status code name of resending |
|-- resultCode | String |	Result code of receiving |
|-- resultCodeName | String |	Result code name of receiving |
|-- createUser | String | Registrant (saved as user UUID when sending from console) |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String |	Recipient's grouping key |
|- totalCount | Integer | Total count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages?startRequestDate=2018-05-01%2000:00&endRequestDate=2018-05-30%2023:59"
```

#### Status of Resending
| Name |	Description|
|---|---|
|RSC01|	No target of resending|
|RSC02|	Target of resending (If sending fails, resending is performed.)|
|RSC03|	Resending in progress|
|RSC04|	Resending successful|
|RSC05|	Resending failed|

## Get Deliveries

#### Request

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|requestId|	String|	O | Request ID |
|recipientSeq|	Integer |	O | Recipient's sequence number |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

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
      "recipientNo": String,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "content": String,
      "messageStatus": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
      "imageSeq": Integer,
      "imageName": String,
      "imageUrl": String,
      "imageLink": String,
      "wide": Boolean,
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

| Name |	Type| 	Descriptions                                                       |
|---|---|-----------------------------------------------------------|
|header|	Object| 	Header area                                                    |
|- resultCode|	Integer| 	Result code                                                    |
|- resultMessage|	String| Result message                                                    |
|- isSuccessful|	Boolean| Successful or not                                                     |
|message|	Object| 	Message                                                      |
|- requestId | String | 	Request ID                                                    |
|- recipientSeq | Integer | 	Recipient's sequence number                                               |
|- plusFriendId | String | 	Plus Friend ID                                                 |
|- senderKey | String | Sender Key                                                      |
|- recipientNo | String | 	Recipient number                                                    |
|- requestDate | String | 	Date and time of request                                                    |
|- createDate | String | Registered date and time                                                     |
|- receiveDate | String | 	Date and time of receiving                                                    |
|- content | String | 	Body                                                       |
|- messageStatus | String | 	Request status (COMPLETED: successful, FAILED: failed)                        |
|- resendStatus | String | 	Status code of resending                                                |
|- resendStatusName | String | 	Status code name of resending                                               |
|- resendResultCode | String | Alternative delivery result code SMS result code                                       |
|- resendRequestId | String | Resending SMS request ID                                             |
|- resultCode | String | 	Result code of receiving                                                 |
|- resultCodeName | String | 	Result code name of receiving                                                |
|- createUser | String | Registrant (saved as user UUID when sending from console)                               |
|- imageSeq|	Integer| Image number                                                    |
|- imageName|	String| Image name (name of uploaded file)                                            |
|- imageUrl|	String| Image URL                                                   |
|- imageLink|	String| Image link                                                    |
|- wide  | boolean | Image is wide or not                                                |
|- buttons | List | 	List of buttons                                                   |
|-- ordering | Integer | 	Button sequence                                                    |
|-- type | String | 	Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery)         |
|-- name | String | 	Button name (up to 28 characters, 9 characters for wide item list)                        |
|-- linkMo | String | 	Mobile web link (required for the WL type)                                |
|-- linkPc | String | 	PC web link (optional for the WL type)                                 |
|-- schemeIos | String | 	iOS app link (required for the AL type)                                |
|-- schemeAndroid | String | 	Android app link (required field if AL type)                              |
|-- chatExtra|	String| 	Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                  |
|-- chatEvent|	String| Bot event name to connect for BT (Bot Transfer) type button                              |
|-- bizFormKey|	String| 	Bizform key for BF (Business Form) type buttons                                |
|-- target|	String| 	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
|- header | String | Header (required when using the wide item list message type, up to 25 characters)                    |
|- additionalContent | String | Additional information (up to 34 characters), available for commerce only                                                                                                                            |
|- item | Object | Wide item                                                   |
|-- list | List | Wide item list (at lease 3, up to 4)                                   |
|--- title | String | Item title (up to 25 characters for the first item, up to 30 characters for items 2-4)          |
|--- imageUrl | String | Item image URL                                               |
|--- linkMo | String | Mobile web link                                                  |
|--- linkPc | String | PC web link                                                   |
|--- schemeIos | String | iOS app link                                                  |
|--- schemeAndroid | String | Android app link                                                |
|- carousel | Object | Carousel                                                       | 
|-- head | String | Carousel intro info                                                | 
|--- header | String | Carousel intro header (up to 20 characters)                                        |  
|--- content | String | Carousel intro content (up to 50 characters)                                        |
|--- imageUrl | String | Carousel intro image address                                            |
|--- linkMo | String | Web link to go to on intro click in mobile                                 |
|--- linkPc | String | Web link to go to on intro click in PC                                  |
|--- schemeIos | String | App link to go to on intro click in iOS                                 |
|--- schemeAndroid | String | App link to go to on intro click in Android                               |
|-- list | List  | Carousel lists (minimum 2, maximum 10)                                     |
|--- header | String | Carousel item title (up to 20 characters)                                        | 
|--- message | String | Carousel item message (up to 180 characters)                                      | 
|--- additionalContent | String | Additional information (up to 34 characters)                                             | 
|--- attachment | Object | Carousel item images, button information                                        | 
|---- buttons | List | Button list (up to 2)                                             | 
|----- name| String | 	Button name (required if you have a button, up to 8 characters)                               |
|----- type| String | 	Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)   |
|----- linkMo| String | 	Mobile web link (required for the WL type)                                |
|----- linkPc | String | PC web link (optional for the WL type)                                  |
|----- schemeIos | String | iOS app link (required for the AL type)                                 |
|----- schemeAndroid | String | Android app link (required field if AL type)                               |
|---- image | Object | Image                                                       | 
|----- imageUrl|	String| 	Image URL                                                  |
|----- imageLink|	String| 	Image link                                                   |
|---- coupon | Object | Coupon                                                        | 
|----- title| String | 	Coupon title                                                 |
|----- description| String | 	Coupon details                                                 |
|----- linkMo| String | Mobile web link                                                  |
|----- linkPc | String | 	PC web link                                                  |
|----- schemeIos | String | iOS app link                                                  |
|----- schemeAndroid | String | Android app link                                                |
|---- commerce | Object | Commerce                                                       | 
|----- title | String | Product title (up to 30 characters)                                             |
|----- regularPrice | Integer | Regular price (0 to 99,999,999)                                      |
|----- discountPrice | Integer | Discount Price (0 to 99,999,999)                                      |
|----- discountRate | Integer | Discount rate (0 to 100), discount rate if discounted price exists                                |
|----- discountFixed | Integer | Flat rate pricing (0 to 999,999)                                       |
|-- tail | Object | Learn more button information                                                 | 
|--- linkMo| String | 	Mobile web link                                                 |
|--- linkPc | String | 	PC web link                                                  |
|--- schemeIos | String | iOS app link                                                  |
|--- schemeAndroid | String | Android app link                                               |
|- coupon | Object | Coupon                                                        | 
|-- title| String | 	Coupon title                                                 |
|-- description| String | 	Coupon details                                                 |
|-- linkMo| String | Mobile web link                                                  |
|-- linkPc | String | 	PC web link                                                  |
|-- schemeIos | String | iOS app link                                                  |
|-- schemeAndroid | String | Android app link                                                |
|- video | Object | Video                                                       | 
|-- videoUrl | String | Kakao TV video URL                  |
|-- thumbnailUrl | String | Image URLs for video thumbnails               |
|- commerce | Object | Commerce                                                       | 
|-- title | String | Product title (up to 30 characters)                                             |
|-- regularPrice | Integer | Regular price (0 to 99,999,999)                                      |
|-- discountPrice | Integer | Discount Price (0 to 99,999,999)                                      |
|-- discountRate | Integer | Discount rate (0 to 100), discount rate if discounted price exists                                |
|-- discountFixed | Integer | Flat rate pricing (0 to 999,999)                                       |
|- isAd | Boolean | Ad or not                                                     |
|- adult | Boolean | Whether the message is for adults                                                |
|- senderGroupingKey | String | Sender's grouping key                                                  |
|- recipientGroupingKey | String | 	Recipient's grouping key                                                |

## Message
### Cancel Sending Messages

#### Request

[URL]

```
DELETE  /friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|requestId| String| Request ID|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|recipientSeq|	String|	X | Recipient's sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

* Both general and authentication messages can be canceled by the same API.

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

### Query Updated Message Results

#### Request

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|startUpdateDate|	String|	O | Start time of querying result updates (yyyy-MM-dd HH:mm)|
|endUpdateDate|	String| O |	End time of querying result updates (yyyy-MM-dd HH:mm) |
|pageNum|	Integer|	X|	Page number (default: 1)|
|pageSize|	Integer|	X|	Number of queries (default: 15)|

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|messageSearchResultResponse|	Object|	Body area|
|- messages | List |	List of messages |
|-- requestId | String |	Request ID |
|-- recipientSeq | Integer |	Recipient's sequence number |
|-- plusFriendId | String |	Plus Friend ID |
|-- senderKey | String |	Sender Key |
|-- recipientNo | String |	Recipient number |
|-- requestDate | String |	Date and time of request |
|-- receiveDate | String |	Date and time of receiving |
|-- content | String |	Body |
|-- messageStatus | String |	Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
|-- resendStatus | String |	Status code of resending |
|-- resendStatusName | String |	Status code name of resending |
|-- resultCode | String |	Result code of receiving |
|-- resultCodeName | String |	Result code name of receiving |
|-- senderGroupingKey | String | Sender's grouping key |
|-- recipientGroupingKey | String |	Recipient's grouping key |
|- totalCount | Integer | Total count |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

### List Mass Delivery Requests

#### Request
[URL]
```
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Descriptions|
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
| X-Secret-Key | String | Unique secret key |

[Query parameter]
* One of the following is required: requestId, startRequestDate + endRequestDate, or startCreateDate + endCreateDate.

| Name |	Type| Max. Length |	Required|	Descriptions|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | O | Start date of delivery |
| endRequestDate | String | - | O | End date of delivery |
| startCreateDate |	String| - |	O |	Start date of registration |
| endCreateDate |	String| - |	O |	End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
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

| Name |	Type|	Descriptions|
|---|---|---|
| header | Object |	Header area |
| - resultCode |	Integer |	Result code |
| - resultMessage |	String | Result message |
| - isSuccessful |	Boolean | Successful or not |
| body | Object | Body area |
| - messages | Object | List of messages |
| -- requestId | String | Request ID |
| -- requestDate | String | Date of request |
| -- plusFriendId | String | PlusFriend ID |
| -- senderKey | String | Sender ID |
| -- masterStatusCode | String | Mass delivery status code (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content | String | Content |
| -- isAd | Boolean | Ad or not |
| -- imageSeq | Integer | Image sequence |
| -- imageLink | Boolean | Image URL |
| -- fileId | String | Attachment ID |
| -- autoSendYn | String | Auto sending or not |
| -- statsId | String | Statistics ID |
| -- createDate | String | Date of creation |
| -- createUser | String | User who created the request (saved as user UUID when sending from console) |
| - totalCount | Integer | Total count |


### List Mass Delivery Recipients

#### Request
[URL]
```
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Descriptions|
|---|---|---|
| appKey |	String |	Unique appkey |
| requestId |	String |	Request ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
| X-Secret-Key | String | Unique secret key |


| Name |	Type| Max. Length |	Required|	Descriptions|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate |	String| - |	X |	Start date of registration |
| endCreateDate |	String| - |	X |	End date of registration |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Search count |

#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
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

| Name | Type| Descriptions |
|---|---|---|
| header | Object |	Header area |
| - resultCode |	Integer |	Result code |
| - resultMessage |	String | Result message |
| - isSuccessful |	Boolean| Successful or not |
| body | Object | Body area |
| - recipients | List | List of recipients |
| -- requestId | String | Request ID |
| -- recipientSeq | Integer | Recipient's sequence number |
| -- recipientNo | String | Recipient number |
| -- requestDate | String | Date of request |
| -- receiveDate | String | Date of receiving |
| -- messageStatus | String | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| -- resultCode | String | Result code of receiving |
| -- resultCodeName | String | Result code name of receiving |
| - totalCount | Integer | Total count |

### Get a Mass Delivery Recipient

#### Request
[URL]
```
GET /friendtalk/v2.4/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Descriptions|
|---|---|---|
| appKey |	String | Unique appkey |
| requestId |	String | Request ID |
| recipientSeq | String | Recipient sequence |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name |	Type|	Description|
|---|---|---|
|X-Secret-Key|	String|	Unique secret key |


| Name |	Type| Max. Length |	Required|	Descriptions|
|---|---|---|---|---|
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | X | Start date of delivery |
| endRequestDate | String | - | X | End date of delivery |
| startCreateDate |	String| - |	X |	Start date of registration |
| endCreateDate |	String| - |	X |	End date of registration |


#### cURL
```
curl -X GET \
'https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

#### Response
```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
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
        "createDate": String
    }
}
```

| Name |	Type|	Descriptions|
|---|---|---|
| header | Object |	Header area |
| - resultCode |	Integer |	Result code |
| - resultMessage |	String | Result message |
| - isSuccessful |	Boolean| Successful or not |
| body | Object | Body area |
| - requestId | String | Request ID |
| - recipientSeq | Integer | Recipient's sequence number |
| - plusFriendId | String | PlusFriend ID |
| - senderKey | String | Sender key (40 characters)|
| - recipientNo | String | Recipient number |
| - requestDate | String | Date of request |
| - receiveDate | String | Date of receiving |
| - content | String | Body |
| - messageStatus | String | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| - resendStatus | String |	Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)<br>([[Refer to Status code of resending table](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] below) |
| - resendStatusName | String |	Status code name of resending |
| - resendRequestId | String | Resending SMS request ID |
| - resendResultCode | String | Result code of resending [Result code of SMS sending](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resultCode | String |	Result code of receiving |
| - resultCodeName | String |	Result code name of receiving |
| - imageSeq | Integer | Image sequence |
| - imageLink | Integer | Image URL |
| - buttons | List | Button sequence |
| -- ordering | String | Button sequence |
| -- type | String | Button type<br/> - WL: Web link<br/> - AL: App link<br/> - DS: Delivery Search<br/> - BK: Bot Keyword<br/> - MD: Message Delivery<br/> - BC: Bot for Consultation<br/> - BT: Bot Transfer<br/> - AC: Channel Added [only for Ad Included/Mixed Purposes Type] |
| -- name | String | Button name |
| -- linkMo | String | Mobile web link (required for the WL type) |
| -- linkPc | String | PC web link (optional for the WL type)|
| -- schemeIos | String | iOS app link (required for the AL type) |
| -- schemeAndroid | String | Android app link (required field if AL type) |
| -- chatExtra | String | BC: Meta information to be delivered when switching to consultation talk<br/> BT: Meta information to be delivered when switching to bot |
| -- chatEvent | String | BT: Bot event name to connect when switching to bot |
| -- bizFormKey|	String|	Bizform key for BF (Business Form) type buttons |
| -- target|	String|	In the case of a web link button, out link used when adding "target":"out" attribute<br>Send with the default in-app link |
| - header | String | Header (required when using the wide item list message type, up to 25 characters) |
| - additionalContent | String | Additional information (up to 34 characters), available for commerce only                                                                                                                            |
| - item | Object | Wide item |
| -- list | List | Wide item list (at lease 3, up to 4) |
| --- title | String | Item title (up to 25 characters for the first item, up to 30 characters for items 2-4) |
| --- imageUrl | String | Item image URL |
| --- linkMo | String | Mobile web link |
| --- linkPc | String | PC web link |
| --- schemeIos | String | iOS app link |
| --- schemeAndroid | String | Android app link |
| - carousel | Object | Carousel | 
|-- head | String | Carousel intro info                                                | 
|--- header | String | Carousel intro header (up to 20 characters)                                        |  
|--- content | String | Carousel intro content (up to 50 characters)                                        |
|--- imageUrl | String | Carousel intro image address                                            |
|--- linkMo | String | Web link to go to on intro click in mobile                                 |
|--- linkPc | String | Web link to go to on intro click in PC                                  |
|--- schemeIos | String | App link to go to on intro click in iOS                                 |
|--- schemeAndroid | String | App link to go to on intro click in Android                               |
|-- list | List  | Carousel lists (minimum 2, maximum 10)                                     |
|--- header | String | Carousel item title (up to 20 characters)                                        | 
|--- message | String | Carousel item message (up to 180 characters)                                      | 
|--- additionalContent | String | Additional information (up to 34 characters)                                             | 
|--- attachment | Object | Carousel item images, button information                                        | 
|---- buttons | List | Button list (up to 2)                                             | 
|----- name| String | 	Button name (required if you have a button, up to 8 characters)                               |
|----- type| String | 	Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, BF: Business Form)   |
|----- linkMo| String | 	Mobile web link (required for the WL type)                                |
|----- linkPc | String | PC web link (optional for the WL type)                                  |
|----- schemeIos | String | iOS app link (required for the AL type)                                 |
|----- schemeAndroid | String | Android app link (required field if AL type)                               |
|---- image | Object | Image                                                       | 
|----- imageUrl|	String| 	Image URL                                                  |
|----- imageLink|	String| 	Image link                                                   |
|---- coupon | Object | Coupon                                                        | 
|----- title| String | 	Coupon title                                                 |
|----- description| String | 	Coupon details                                                 |
|----- linkMo| String | Mobile web link                                                  |
|----- linkPc | String | 	PC web link                                                  |
|----- schemeIos | String | iOS app link                                                  |
|----- schemeAndroid | String | Android app link                                                |
|---- commerce | Object | Commerce                                                       | 
|----- title | String | Product title (up to 30 characters)                                             |
|----- regularPrice | Integer | Regular price (0 to 99,999,999)                                      |
|----- discountPrice | Integer | Discount Price (0 to 99,999,999)                                      |
|----- discountRate | Integer | Discount rate (0 to 100), discount rate if discounted price exists                                |
|----- discountFixed | Integer | Flat rate pricing (0 to 999,999)                                       |
|-- tail | Object | Learn more button information                                                 | 
|--- linkMo| String | 	Mobile web link                                                 |
|--- linkPc | String | 	PC web link                                                  |
|--- schemeIos | String | iOS app link                                                  |
|--- schemeAndroid | String | Android app link                                               |
|- coupon | Object | Coupon                                                        | 
|-- title| String | 	Coupon title                                                 |
|-- description| String | 	Coupon details                                                 |
|-- linkMo| String | Mobile web link                                                  |
|-- linkPc | String | 	PC web link                                                  |
|-- schemeIos | String | iOS app link                                                  |
|-- schemeAndroid | String | Android app link                                                |
|- video | Object | Video                                                       | 
|-- videoUrl | String | Kakao TV video URL                  |
|-- thumbnailUrl | String | Image URLs for video thumbnails               |
|- commerce | Object | Commerce                                                       | 
|-- title | String | Product title (up to 30 characters)                                             |
|-- regularPrice | Integer | Regular price (0 to 99,999,999)                                      |
|-- discountPrice | Integer | Discount Price (0 to 99,999,999)                                      |
|-- discountRate | Integer | Discount rate (0 to 100), discount rate if discounted price exists                                |
|-- discountFixed | Integer | Flat rate pricing (0 to 999,999)                                       |
|- isAd | Boolean | Ad or not                                                     |
|- adult | Boolean | Whether the message is for adults                                                |
|- createDate | String | Date of creation |


## Image Management

### Register Images
#### Request

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|image|	File|	O |	Image |
|wide| boolean | X | Image is wide or not (Default: false) |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images" -F "image=@friend-ricecake02.jpeg"
```

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|image|	Object|	Body area|
|- imageSeq | Integer |	Image number (used when sending a FriendTalk message)|
|- imageUrl | String |	Image URL |
|- imageName | String |	Image name (name of uploaded file) |

### Register Wide Item List Images
#### Request

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/wide-itemlist/images
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|image|	File|	O |	Image |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/wide-itemlist/images" -F "image=@friend-ricecake02.jpeg"
```

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|image|	Object|	Body area|
|- imageSeq | Integer |	Image number (used when sending a FriendTalk message)|
|- imageUrl | String |	Image URL |
|- imageName | String |	Image name (name of uploaded file) |

### Register Carousel Image
#### Request

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/carousel/images
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|image|	File|	O |	Image |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/carousel/images" -F "image=@friend-ricecake02.jpeg"
```

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|image|	Object|	Body area|
|- imageSeq | Integer |	Image number (used when sending a FriendTalk message)|
|- imageUrl | String |	Image URL |
|- imageName | String |	Image name (name of uploaded file) |

### Register carousel commerce images
#### Request

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/carousel/commerce-images
Content-Type: multipart/form-data
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Request parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|image|	File|	O |	Image |

[Example]
```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/carousel/images/intro" -F "image=@friend-ricecake02.jpeg"
```

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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|image|	Object|	Body area|
|- imageSeq | Integer |	Image number (used when sending a FriendTalk message)|
|- imageUrl | String |	Image URL |
|- imageName | String |	Image name (name of uploaded file) |

### Query Images
#### Request

[URL]

```
GET  /friendtalk/v2.4/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|imageTypes|	String|	X| - IMAGE: General image<br/> - WIDE_IMAGE: Wide image<br/> - WIDE_ITEMLIST_IMAGE: Wide item list image<br/> - CAROUSEL_IMAGE: Carousel image<br/> IMAGE, WIDE_IMAGE (default) |
|pageNum|	Integer|	X|	Page number (default: 1)|
|pageSize|	Integer|	X|	Number of queries (default: 15)|

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images?pageNum=1&pageSize=15"
```

#### Response
```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "imagesResponse": {
    "images": [
        {
            "imageSeq": Integer,
            "imageUrl": String,
            "imageName": String,
            "imageType": String,
            "createUser": String
        }
    ],
    "totalCount": Integer
  }

}
```

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|imagesResponse| Object| Body area|
|- image|	Object|	Body area|
|-- imageSeq | Integer |	Image number (used when sending a FriendTalk message)|
|-- imageUrl | String |	Image URL |
|-- imageName | String |	Image name (name of uploaded file) |
|-- createUser | String |	Creator |
|-- imageType | String |	- IMAGE: General image<br/> - WIDE_IMAGE: Wide image<br/> - WIDE_ITEMLIST_IMAGE: Wide item list image<br/> - CAROUSEL_IMAGE: Carousel image<br/> |
|- totalCount | Integer | Total count |

* Response is sent in the order of latest registration.

### Delete Images
#### Request

[URL]

```
DELETE  /friendtalk/v2.4/appkeys/{appkey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name |	Type|	Required|	Description|
|---|---|---|---|
|imageSeq|	String|	O|	Image number |

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/images?imageSeq=1,2,3"
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

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|


## Upload
### Register a business form
[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|
|senderKey|	String|	Sender Key |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |


[Request body]

```
{
    "bizFormId": Integer
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|bizFormId|	Integer|	O | Business Form ID |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/senders/{senderKey}/biz-form -d '{"bizFormId": 1}
```

#### Response
```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "bizFormKey": String
}
```

| Name |	Type|	Descriptions|
|---|---|---|
|header|	Object|	Header area|
|- resultCode|	Integer|	Result code|
|- resultMessage|	String| Result message|
|- isSuccessful|	Boolean| Successful or not|
|bizFormKey | String | Business form key |


## Manage Alternative Delivery
### SMS app key registration

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |


[Request body]

```
{
    "resendAppKey": String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|resendAppKey|	String|	O | SMS service appkey to set for alternative delivery |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
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

### Register Alternative Delivery Settings

[URL]

```
POST  /friendtalk/v2.4/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name |	Type|	Description|
|---|---|---|
|appkey|	String|	Unique appkey|

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |


[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| Name |	Type|	Required|	Description|
|---|---|---|---|
|senderKey|	String|	O | Sender Key |
|isResend|	Boolean|	O | Whether to resend text, if delivery fails<br>Resent by default, if alternative delivery is set on console. |
|resendSendNo|	String|	O | Sender number for alternative delivery (up to 13 characters)<br><span style="color:red"> (Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |
|resendUnsubscribeNo|	String|	X | Alternative delivery 080 blocked number<br><span style="color:red"> (If it is not the 080 blocked number registered in the SMS service, alternative delivery may fail.) </span> |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/friendtalk/v2.4/appkeys/{appkey}/failback/appkey -d '{"senderKey": "9e0afe2c12aaaaaaaaaa7520052880b555f1a60a","isResend": true,"resendSendNo": "01012341234", "resendUnsubscribeNo": "0801234567" }
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
