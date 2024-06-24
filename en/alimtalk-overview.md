## Notification > KakaoTalk Bizmessage > AlimTalk > Overview

Alimtalk is a service based on mobile phones with which users can send informative messages such as delivery message, schedule notification, and others without adding the recipient as a friend.
The service provides RESTful APIs for easy integration.

## Characteristics
* You can send long messages up to 1,000 characters at a lower cost than SMS.
* Informational messages without commercial purposes, such as announcements, notifications, and guidance, can be sent regardless of adding channel friends or not.
* AlimTalk can be sent after completing the template-based inspection.
* Replacement tags can be used to send individual content to each recipient.
* Messages can be delivered more efficiently through various types of AlimTalk in text type, image type, highlight type and item list type.
* Kakao authentication marks can be sent together to increase the credibility of the brand.
* It shows a high message reach rate because it can be sent as an alternative text message to recipients who do not use KakaoTalk.

## Main Features
* NHN Cloud provides a differentiated features to modify AlimTalk template (no template code change used for API interworking)
* It provides RESTful APIs such as sending, inquiring and etc.
* It provides a Webhook for updating delivery results, updating template status.
* You can also send, inquire about delivery history, and manage templates from the console.

## AlimTalk Template Precautions
AlimTalk is an informational message that can be sent to users who have not added KakaoTalk channel.<br/> Among the exceptions to commercial information for commercial purposes in the Information and Communication Network Act guide to prevent illegal spam, only templates that have been judged to be suitable for the protection of KakaoTalk users can be sent. [[Guidance on compliance with informational messages (AlimTalk)]](https://cs.kakao.com/helps?articleId=1073203715&service=159&category=503&device=1871&locale=ko)

#### Alert Talk Message Maximum Character Guide
Please refer to the following two additional contents based on the maximum number of characters in Alim Talk message, which is largely basic and up to 1,000 characters in image/item list 700 characters for each message type.

* The maximum number of characters in the message is a standard that includes all of 'Body + Variables + Additional Information'.
* For the type of message that uses the Add Channel button, consider the number of guiding characters (about 40 characters) associated with adding a channel.
    * “Channel Addition type (AD)” and “Complex type (MI)” using the channel add button should be considered to be included in the AlimTalk maximum character guide because AlimTalk message (approximately 40 characters) related to channel addition is required by default.
    * In case of the message type using the Add Channel button, please consider the maximum number of characters in the 'Body + Variables + Additional Information' message except for the number of characters related to adding a channel (about 40 characters).

#### Number of variable limits in AlimTalk template
* Templates with up to 40 variables in one template cannot be registered.

#### Validating AlimTalk Template button
* Please be careful that the delivery will fail if there is a case where the normal link connection is not possible among the buttons in the AlimTalk sent to the customer. [[Related Notice Shortcut]](https://www.nhncloud.com/kr/support/notice/detail/5287)

#### AlimTalk Template Dormant Policy
* If it will remain the same for 1 year after template registration or if there is no additional delivery, it will switch to **Dormant** status. 
    * After a year in the dormant state, the template is deleted and the deleted template cannot be recovered.
    * If there is no history of sending notification messages for 30 days after the dormant state is released, it will be re-dormanted.
* In case of dormant release, please submit the request form in the path [[Technical Support 1:1 Inquiry]](https://www.nhncloud.com/kr/support/inquiry).
    * Template dormant release Request Form<br/>- Kacao template code to release dormant:<br/> - Sender Profile Name:

#### AlimTalk Template Review Time
* The inspection and review of AlimTalk template will be conducted directly by Kakao, and will be processed sequentially within two business days after the inspection request.

#### Request for Emergency Inspection of AlimTalk Template
* Emergency inspection only allows emergency notification messages related to failure/error and mis-delivery, and if not, requests for quick emergency cannot be accepted.
* If you meet the emergency inspection criteria, you can apply by submitting the request form through the path [[Technical Support 1:1 Inquiry]](https://www.nhncloud.com/kr/support/inquiry).
    * Emergency Inspection Request Form<br/>- Kakao Template Code:<br/> - Template name:<br/> - Sender Profile Name:<br/>- Reasons for emergency inspection:
    * How to check Kakao template code<br/>- NHN Cloud Console > KakaoTalk Bizmessage > AlimTalk > Template > Template to request emergency inspection > Check Kakao Template Code

## Template Inspection and Creation Guide
* [[AlimTalk Creation Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/content-guide)
* [[AlimTalk Inspection Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit)
* [[AlimTalk Image Creation Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/content-guide/image)
  


