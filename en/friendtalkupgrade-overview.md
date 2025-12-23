## Notification > KakaoTalk Bizmessage > Brand Message > Overview

Brand Message is a message product that allows advertisers (clients) to send advertising messages to members who have agreed to receive marketing messages (hereinafter referred to as “marketing consent”), regardless of whether they are KakaoTalk channel friends.

The service provides RESTful API for easy integration.

## Features

* Allow you to send various promotional messages including advertising messages for users who have become friends.
* Even for users who have not become friends, allow you to send advertising messages to users with marketing consent.
* To send a marketing consent, need to 080 opt-out information on the send channel.
* Sending a marketing consent is possible only in the basic type (template).
* Since Brand Message is an advertising messages, it is restricted from being sent between 8:50 PM and 08:00 AM the next day in accordance with the Information and Communications Network Act.
* Allow you to send text and images at a lower cost than LMS/MMS.
* Template and free-form sending are possible.
* If a brand message fails to be sent, it can be replaced with a text message.

## Main features

* Provide RESTful API for sending messages, viewing, and managing images.
* Allow you to view the history of sending messages, viewing, and managing images on the console.
* Allow you to manage 080 opt out by linking with SMS service.
* Provide fallback feature by linking with SMS service.

## Brand Message targeting

![friendtalkupgrade_1_20250508.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_1_20250508.png)

### M: Advertisers’ users with marketing consent

* Send advertising messages to Advertisers’ users with marketing consent (KakaoTalk message consent)
* If the recipient is not a friend of the send channel, the message will be sent as an N-type channel message containing free opt-out information.
* The free opt-out information can be registered and managed from a sender profile of the send channel.

![friendtalkupgrade_02_20250508.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_2_20250508.png)

### N: Advertisers’ users with marketing consent- channel friend

* Send advertising messages to advertisers’ users with marketing consent (KakaoTalk message consent), except channel friends.
* As the recipient is not a friend of the send channel, the message will be sent as an N-type channel message containing free opt-out information.
* The free opt-out information can be registered and managed from a sender profile of the send channel.

![friendtalkupgrade_03_20250508.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_3_20250508.png)

### I: Target of advertiser sending request ∩ Channel friend

* For advertiser sending requests, advertising messages are sent only to channel friends.

## Brand message sending support type

- **When registering as a basic type (template), you can use targeting options M and N to target users with “marketing consent” for all sending support types.**<br>
  - If you want to send to channel friends, you must use the targeting option I.
- Free form sending cannot use targeting options M and N for users with “marketing consent”.

| Category| Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Kakao image upload specifications                                                                                                                                                                                                                                                                          |
|:----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Text**| \- Text types consist of “message + button + coupon”.<br>- Message field is required and can contain up to 1,300 characters. The button and coupon fields are optional.<br>-`chat_bubble_type: "TEXT"` is applicable for this.                                                                                                                                                                                                                                                                                                                                                     | No image                                                                                                                                                                                                                                                                                                   |
| **Image**| \- Image types consist of “image + message + button + coupon”.<br>- Image and message fields are required, and messages can contain up to 400 characters. The button and coupon fields are optional.<br>- For images, you must use images uploaded using the general image upload API.<br>-`chat_bubble_type: "IMAGE"` is applicable for this.                                                                                                                                                                                                                                     | Recommended size: 800 X 400px (500px or more in width)<br>Aspect ratio: 2:1 or more, 3:4 or less<br>file format and capacity limit: JPG, PNG / up to 5MB                                                                                                                                                   |
| **Wide image**| \- Wide image types consist of “wide image + message + button + coupon”.<br>- Wide image and message fields are required, and messages can contain up to 76 characters. The button and coupon fields are optional.<br>- For wide images, you must use images uploaded using the wide image upload API.<br>-`chat_bubble_type: "WIDE"` is applicable for this.                                                                                                                                                                                                                      | Recommended size: 800 X 600px (500px or more in width)<br>Aspect ratio: 2:1 or more, 1:1 or less<br>file format and capacity limit: JPG, PNG / up to 5MB                                                                                                                                                   |
| **Wide item list**| \- Wide list type consists of "header + 1,2,3,4 item list + button + coupon".<br>- Header field and item list (minimum: 3, maximum: 4\) are required, and the button and coupon fields are optional.<br>- For item images, you must use images uploaded using the first wide item image upload API or the general wide item list image upload API..<br>- `chat_bubble_type: "WIDE_ITEM_LIST"` is applicable for this.                                                                                                                                                              | **First image**<br>  - Limited size: 500px or more in width<br>  - Aspect ratio: 2:1<br>  - File format and capacity limit: JPG, PNG / up to 5MB<br> **Other images**<br>  - Limited size: 500px or more in width<br>  - Aspect ratio: 1:1<br>  - File format and size: JPG, PNG / up to 5MB for each file |
| **Carousel feed**| \- Carousel feed consists of “1 to 6 carousel list + more”.<br>- Carousel list (minimum: 2, maximum:6) is required, and More is optional.<br>- For carousel list images, you must use images uploaded using the carousel feed image upload API.<br>- `chat_bubble_type: "CAROUSEL_FEED"` is applicable for this.                                                                                                                                                                                                                                                                   | Recommended size: 800 X 600px or 800 X 400px (500px or more in width)<br>Aspect ratio: 2:1 or more, 3:4 or less<br>file format and capacity limit: JPG, PNG / up to 5MB                                                                                                                                    |
| **Premium video**| \- Premium video consists of “video + header + message + button + coupon”.<br>- Video field is required, and header, message, button, coupon fields are optional.<br>- Video links can only be used for videos uploaded to Kakao TV. (`https://tv.kakao.com/v/#{숫자}` / `https://tv.kakao.com/channel/#{숫자}/cliplink/#{숫자}`)<br>- You can use an image uploaded as a standard image as a thumbnail.<br>- `chat_bubble_type: "PREMIUM_VIDEO"` is applicable for this.                                                                                                                                                                                 | (Optional) Recommended size: 800 X 400px (500px or more in width)<br>Aspect ratio: 2:1 or more, 3:4 or less<br>file format and capacity limit: JPG, PNG / up to 5MB                                                                                                                                        |
| **Commerce**| \- Commerce type consists of "commerce image + commerce element + additional information + button + coupon".<br>- Commerce images and commerce elements are required, while additional information, buttons, and coupon fields are optional.<br>- In commerce element, product title (`title`) and regular price (`regular_price`) values are required.<br>- For commerce images, you must use images uploaded using the general image upload API.<br>- `chat_bubble_type: "COMMERCE"` is applicable for this.                                                                     | Recommended size: 800 X 400px (500px or more in width)<br>Aspect ratio: 2:1 or more, 3:4 or less<br>file format and capacity limit: JPG, PNG / up to 5MB                                                                                                                                                   |
| **Carousel commerce**| \- Carousel commerce type consists of "carousel intro + 1 to 6 carousel list + more".<br>- Carousel list is required, and carousel intro and more are optional.<br>- When using a carousel intro, the carousel list must consist of at least 1 and no more than 5 items.<br>- If you do not use a carousel intro, your carousel list must consist of at least 2 and no more than 6 items.<br>- For carousel intro and list images, you must use images uploaded using the carousel commerce image upload API.<br>- `chat_bubble_type: "CAROUSEL_COMMERCE"` is applicable for this. | Recommended size: 800 X 600px or 800 X 400px (500px or more in width)<br>Aspect ratio: 2:1 or more, 3:4 or less (same aspect ratio for the entire image) <br>File format and capacity limit: JPG, PNG / up to 5MB                                                                                          |

## Precautions before sending

Non-friend message sending (targeting M, N) can be sent if all the conditions below are met:

- Business verification channel
- Register a business registration number
- Register a channel customer service center phone number
- 50,000 or more channel friends
- Successfully send notification messages within the past three months

Resellers must additionally register a reseller code to be transmitted to KISA when reporting messages.

- If you need to add a reseller code, please request one through [Customer Center > 1:1 Inquiry].

## Notice of personal information consignee

When a customer uses NHN Cloud > Notification > KakaoTalk Bizmessage service, a business entrustment relationship regarding the processing of personal information occurs between the customer and our company. Therefore, in accordance with the Information and Communications Network Act and the Personal Information Protection Act, the customer, as the consignor, must disclose the status of the personal information entrusted to us (the trustee and the details of the work) through the personal information processing policy.

Accordingly, our company can guide our customer companies as follows to comply with relevant laws and regulations when using NHN Cloud's KakaoTalk Bizmessage service and to avoid penalties and other disadvantages due to failure to disclose the status of the entrustment.

(Example)

\[Notice of personal information consignee]

When using the KakaoTalk Bizmessage service, please indicate the following in the “Personal Information Processing Policy > Consignment Status” operated by the client company.

Trustee: NHN Cloud Co., Ltd.

Task description: KakaoTalk Bizmessage send service