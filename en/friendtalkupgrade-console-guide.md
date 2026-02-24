## Notification > KakaoTalk Bizmessage > Brand Message > Console User Guide

## Brand Message sending

### Regular send

You can set up an outgoing profile and enter content to send messages in the form of brand message. To send a brand message, select Notification > KakaoTalk Bizmessage > Brand Message on the console.

### When using a template

![friendtalkupgrade_04_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_04_20250729.png)

1. Select an outgoing profile.
   * You can check the outgoing profile from the Notification > KakaoTalk Bizmessage > Manage Outgoing Profiles tab.
2. Select push notification status.
   * If you select “Disable”, the push notification for messages is not sent.
3. Select “Enable” for using template, then select the template you wish to request sending.
   * The template can be registered from the <strong>Notification > KakaoTalk Bizmessage > Brand Message > Manage Templates</strong> tab.
4. Select whether to set fallback messages.
   * To enable fallback, you must set the fallback option to “Enable” in the <b>Manage 080 Opt Out</b> tab and integrate the SMS service.
5. Select whether to set 080 opt-out number.
   * If you select “Not Set (send using outgoing profile settings)” for the 080 opt-out number setting, the 080 opt-out number set in the outgoing profile will be used.
   * If you select “Set as Common Content” or “Set by User”, the 080 opt-out number you entered will be used for sending.
   * When using the SMS service from the <b>Manage 080 Opt Out</b> tab, only 080 opt-out numbers registered in the SMS service are available.
6. Add a recipient.
   * A recipient can be written in phone number format.
7. Select a targeting type by recipient.
   * Brand Message is an advertising message that can be sent to users who have agreed to receive marketing messages from advertisers (hereinafter referred to as "marketing consent") and channel friend users.
   * Depending on the targeting type, whether the specified recipients receive your message may vary.
     * M: Advertisers’ users with marketing consent
       * Send advertising messages to Advertisers’ users with marketing consent (KakaoTalk message consent)
     * N: Advertisers’ users with marketing consent- channel friend
       * Send advertising messages to advertisers’ users with marketing consent (KakaoTalk message consent), except channel friends.
     * I: Target of advertiser sending request ∩ Channel friend
       * For advertiser sending requests, advertising messages are sent only to channel friends.
8. After completing the input, click Send to send.

### When not using a template

![friendtalkupgrade_05_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_05_20250729.png)

1. Select an outgoing profile.
   * You can check the outgoing profile from the Notification > KakaoTalk Bizmessage > Manage Outgoing Profiles tab.
2. Select push notification status.
   * If you select “Disable”, the push notification for messages is not sent.
3. Select whether the message is for adults or not.
   * For adult messages, chat room messages will be covered until age verification is completed, and only those over 20 years of age can view the content.
4. Select a message type.
   * Text
     * 1,300 characters of text including spaces, in both Korean and English + up to 5 link buttons (vertically arranged)
   * Image
     * 400 characters of text including spaces, in both Korean and English + one image + up to 5 link buttons (vertically arranged)
   * Wide image
     * 76 characters of text including spaces, in both Korean and English + one image + up to 2 link buttons
   * Wide item list
     * An ad-type product that allows you to add 3 to 4 lists (images + items) to one title.
     * First item title 25 characters, including spaces, regardless of Korean/English, 24 + up to 2 link buttons (horizontally aligned)
   * Carousel feed
     * An ad-type product that can contain up to 10 images and various text information.
     * Up to 6 items consisting of a title of 20 characters including spaces, a phrase of 180 characters, an image, and 2 link buttons (horizontally aligned), regardless of Korean/English
   * Premium video
     * This type of video is automatically played in a speech bubble.
     * Video links can only be used for videos uploaded to Kakao TV (e.g. [https://tv.kakao.com/v/#{number}](https://tv.kakao.com/v/#%7B%EC%88%AB%EC%9E%90%7D) / [https://tv.kakao.com/channel/#{number}/cliplink/#{number}](https://tv.kakao.com/channel/#%7B%EC%88%AB%EC%9E%90%7D/cliplink/#%7B%EC%88%AB%EC%9E%90%7D)).
     * 20-character header text including spaces, regardless of Korean/English, + 76-character text for the text + 1 video uploaded to Kakao TV + 1 link button
   * Commerce
     * A speech bubble that can highlight product prices and discount information.
     * 20-character title text including spaces, regardless of Korean/English, 34-character text for additional information + 2 link buttons (horizontally aligned)
   * Carousel commerce
     * A speech bubble that can organize information on various products in a catalog format.
     * Up to 6 items consisting of a title text of 30 characters including spaces, a phrase of 34 characters, and up to 2 link buttons (horizontally aligned), regardless of Korean/English
     * Every image used for carousel commerce must have the same ratio.
5. If you have an image, select it.
   * To attach an image to a message, first register the image in the Manage Image tab.
   * Image link: Enter the link you want to connect to when the image is clicked (URL including http://, https://).
   * Every image used for carousel commerce must have the same ratio.
6. You can insert web links, app links, bot keywords, message forwarding, add channel, chat conversion, bot conversion, and business form buttons.
   * Up to 5 basic items, up to 2 carousel/wide item lists
7. If you need to highlight a coupon in your message, you can use a button that takes users to the attached coupon when they click on it.
8. Select whether to set fallback messages.
   * To enable fallback, you must set the fallback option to “Enable” in the “080 Opt Out Management” tab and integrate the SMS service.
9. Add a recipient.
   * A recipient can be written in phone number format.
10. Select a targeting type by recipient.
    * Brand Message is an advertising message that can be sent to users who have agreed to receive marketing messages from advertisers (hereinafter referred to as "marketing consent"') and channel friend users.
    * Depending on the targeting type, whether the specified recipients receive your message may vary.
      * M: Advertisers’ users with marketing consent
        * Send advertising messages to Advertisers’ users with marketing consent (KakaoTalk message consent)
      * N: Advertisers’ users with marketing consent- channel friend
        * Send advertising messages to advertisers’ users with marketing consent (KakaoTalk message consent), except channel friends.
      * I: Target of advertiser sending request ∩ Channel friend
        * For advertiser sending requests, advertising messages are sent only to channel friends.
11. After completing the input, click Send to send.

### Mass Delivery

This feature allows you to send brand messages to multiple recipient numbers using a template file in Excel/CSV format. Select <b>Mass Delivery</b> from the bottom tab.

![friendtalkupgrade_mass01.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade_mass01.png)

* You can download a CSV or XLSX file containing the template placeholder by entering the template placeholder in the content in the format '#{name}' and clicking the **Download Template** button.
* When opening and saving CSV files with Excel, Korean characters may not be saved properly.
* Only CSV, XLSX files can be uploaded. The maximum size is 30MB, and the maximum number of recipients you can send to is 1,000,000.

![friendtalkupgrade_mass02.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade_mass02.png)

* You can enter individual targeting values and template placeholders for each recipient.
  * For free-form sending, be careful not to set M or N targeting values, as this will result in send failure.
* If you enter content without template placeholders, the same content will be sent to all recipients.

When clicking <b>Send</b> button, you can select <b>Proceed after Review</b> or <b>Immediate Send</b>.

* Proceed after Review: after confirming the recipient of the email within 7 days in the <b>View Mass Delivery</b> tab, a notification message will be sent. (unsupported for scheduled send.)
* Immediate Send: AlimTalks are sent immediately without confirming the recipient. You can check the progress of your shipment in the <b>View Mass Delivery</b> tab.

### Fallback

A feature that allows you to replace a brand message with a text message if it fails to send. You can send it by selecting <b>Set as Common Content</b> or <b>Set by Different Messages per User</b>.

![friendtalkupgrade_06_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_06_20250729.png)

* To enable fallback, you must set the fallback option to “Enable” in the “Manage 080 Opt Out” tab and integrate the SMS service.
* You must have an outgoing number to send a fallback. If you do not set a fallback option or a sending number, the default settings specified in the Manage Fallback tab will be applied.
* Fallback is performed after changing to Kakao’s send failure, and it may take up to tens of seconds to receive a text message.
* Depending on the message, some content, such as buttons and links, may appear differently from the KakaoTalk message.
* Depending on the message length, it will be sent as SMS/LMS (separate fees apply for sending each type of text message).

#### Set as Common Content

![friendtalkupgrade_resend01_20250729.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_resend01_20250729.png)

* Set a fallback message by selecting <b>Outgoing Profile</b> and clicking <b>Set as Common Content</b>.
* If you do not enter a fallback message, it will be sent as [Message Body].

#### Set by Different Messages per User

* After selecting an <b>outgoing profile</b>, click <b>Set by Different Messages per User</b> and set an alternative sending message in the <b>Add Recipient</b> tab at the bottom.
* You can check and modify the settings by clicking the [pen icon] next to the added recipient number.

What is !!! danger "Notes for sending advertising messages" advertising message? It refers to messages about information, goods, or services that the sender transmits for the purpose of obtaining economic gain.

    **[Guide for Sending Advertising Message]**
    1. Display ads
        ① Messages containing advertising content must be marked with "(Advertisement)" at the very beginning.
        ② Criteria for determining advertising messages
            a. Information on special offers/discounts
            b. Promotions or events to promote products and services
            c. Even if information is presented as "Note," if the contents of [a and b above] are mixed
    2. Mandatory display of sender's contact information
       ① The sender's name and contact information must be written above the message body.
       ② The sender's name and contact information will be displayed at the top of the chat room.
       ③ For contact information, you must enter either a phone number or an address.
    3. Mandatory display of measures and methods that facilitate the expression of intent to refuse receipt
       ① The measures and methods that facilitate opting out or withdrawing consent must be clearly stated in the body of the advertisement.
       ② If opting out or withdrawing consent is difficult or impossible due to these measures or methods, they will be deemed not to have been indicated.
       ③ Requiring additional steps, such as logging into a website, to opt out of receiving emails makes opting out or withdrawing consent difficult and constitutes a violation of the law.

![[Figure 3] FriendTalk advertising message](http://static.toastoven.net/prod_alimtalk/friendtalk_02.png)

## View Send

### View Send Result

![friendtalkupgrade_07_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_07_20250729.png)

* You can view detailed information about each sent message by clicking on the search results item in the **View Details** screen.
* **Request Date** can be searched for up to one month.
* You can check the status of the sending request in the **Request Status** column.
* You can check the sending process status in the **Send Result** column.

### Cancel send

Cancellation is possible for scheduled send with a sending request date set to the future during regular send.

* When you view a scheduled send request, you can see a checkbox to the left of the request ID.
* The checkbox only appears for reservation requests that have not been canceled.
* Select the checkbox for the request you wish to cancel and click the Cancel Selected Schedule button at the top to cancel the schedule.
* You can select or deselect the entire list by checking the checkboxes in the header of the list.

## View Mass Delivery

### View Send Result

![friendtalkupgrade_masssearch01_20250729.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_masssearch01_20250729.png)

* You can select each mass delivery request and check the recipient’s send results.
* You can view detailed information about each sent message by clicking on the search results item in the **View Details** screen.
* **Request Date** can be searched for up to one month.
* You can check the status of the sending request in the **Request Status** column.
* You can check the sending process status in the **Send Result** column.

## Manage Image

![friendtalkupgradfriendtalkupgrade_07_20250729.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_8_20250616.png)

You can register or delete images to use in the brand message and check information about registered images.

* You can register images by selecting them by type (basic/wide, wide item list type, carousel feed, carousel commerce, business form).
* When you delete an image, the image cannot be used.
* Be sure to adhere to file specifications, recommended/limited sizes.
* You can copy an image address URL. The URL is used when sending the API.
* Business Form is a business tool that supports the design of events such as reservations, surveys, and applications that KakaoTalk users can easily participate in. Create a business form and enter the corresponding ID.
* [\[Go to Business Form Registration\]](https://business.kakao.com/talkbizform/)
* [\[Go to Business Form Registration Guide\]](https://kakaobusiness.gitbook.io/main/tool/bizform)

#### Image upload allowance

* File format: JPG, PNG
* Please check each image upload API specification.

## Manage Templates

![friendtalkupgrade_09_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_09_20250729.png)

* You can manage templates and use them for sending in a similar way to existing AlimTalk messages.
* ******Brand messages are free to create, edit, and delete, with no review process.******
* Unlike AlimTalk, this is a method where users do not register a template code, but instead receive a random identifier from Kakao.

## 080 Opt-out management

* In brand messages, the “080 opt-out management” and “fallback” features are integrated into a single smsAppkey through NHN Cloud's SMS service integration.

### Register and manage 080 opt-out numbers

* When sending Brand Message marketing to users who have opted in, you must register a 080 number in your outgoing profile because messages can be sent to recipients who are not friends with your outgoing profile.
  * The 080 opt-out number for the outgoing profile is data that is applied to all outgoing profiles of other organizations, projects, and dealers in the same talk channel, so it must be changed carefully.
* If you need to register the 080 opt-out number on an outgoing profile or modify it, please contact the Customer Center.
* If you register a 080 opt-out number, the outgoing profile owner is responsible for ensuring that messages are not sent to recipients who have indicated they wish to opt out of receiving calls through the number.
* By linking with NHN Cloud SMS service, you can easily manage status of the 080 opt-out numbers. ![sms 080](https://static.toastoven.net/prod_sms/SMS_14_20230818.png)
* To manage the 080 opt-out number with the SMS service, you need to enable the SMS service first.
* Register the 080 opt-out number you wish to manage for the SMS service.
  * If the 080 opt-out number registered in the outgoing profile is not registered with the SMS service, all sending requests will fail.
* For more information, see [SMS User Guide](https://docs.nhncloud.com/ko/Notification/SMS/ko/console-guide/#080 "friendtalkupgrade_10_20250616.png"). ![](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_10_20250729.png)
* Enter the app key of the SMS service that registered the 080 opt-out number in the 080 Opt Out tab and save it.
* You can check whether recipient management is possible with the 080 opt-out number registered in the outgoing profile of the SMS service designated as the management target by checking the **SMS Project 080 Number Management Status** item of the outgoing profile in the **080 Opt-out Management** tab of the console.
* If you empty and save the SMS service app key
  * Customers must manage their own opt-out recipients, and there is no verification of recipients when sending brand messages.

### Manage Fallback

* If a brand message fails to be sent, you can set it to be sent as a fallback message.
* You must be using NHN Cloud SMS service, and messages will be sent via SMS/LMS depending on the message length (separate fees apply for sending each type of text message).
* If you modify the SMS app key, the fallback settings for all channels for search IDs will be reset.
* Since it is sent as a fallback through the SMS service, you must enter fields according to the SMS service's sending API specifications (such as outgoing number registered with the SMS service, various field length restrictions).
* Fallback titles or contents that exceed the byte limit of the specified fallback type may be truncated and replaced (see [SMS API Guide > Caution]).
* Fallback content is sent based on EUC-KR, and unsupported emoticons will fail to be sent as fallbacks.
* You can change the <b>Fallback Status</b> and <b>Outgoing Number</b> by clicking <b>Modify</b> on the channel ID for the search.
* If you set the <b>Fallback Status</b> to <b>Disable</b>, the channel for the search ID will not be able to send the brand message.
* Brand Message advertising messages are sent as fallback messages using the advertising SMS API, so you must register a 080 opt-out number to receive fallback messages.
* When entering the resendContent field of a brand message advertising message, you must enter the <span style="color:red">advertising text</span> from the SMS advertising API to send it as a replacement. `(ads) content [toll-free opt out]080XXXXXXX`
* If there is no resendContent field in the brand message advertising message, the <span style="color:red">advertising text</span> will be automatically generated and sent to the registered 080 opt-out number.

## Apply for using non-friend message sending (targeting M, N)

* If you wish to use non-friend message sending (targeting M, N), you must apply for use. If you do not apply for use, the M/N type will not be displayed during send.
* Applications for use will be approved if they meet the following conditions:
  * Business verification channel
  * Register a business registration number
  * Register a channel customer service center phone number
  * 50,000 or more channel friends
  * Successfully send notification messages within the past three months

### Cautions

* If your business verification is canceled, the permission to send non-friend messages (targeting M, N) will be canceled. You will need to reapply for use after your business verification has been re-reviewed.
* The consent evidence file for receiving advertising information is saved per Talk channel, so any changes will be applied to all profiles sent by other dealers in the same Talk channel.
* If a file uploaded by a dealer already exists, you can skip the file upload process and apply to send non-friend messages (targeting M, N).

![friendtalkupgrade_11_20250715.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_11_20250729.png)