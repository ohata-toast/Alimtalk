## Notification > KakaoTalk Bizmessage > AlimTalk > Console Guide

## Send AlimTalk
### General Delivery
This is general delivery screen of AlimTalk

![KTB_09_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_09_20230926.png)

1. Select sender profile. 
    * Sender profile can be checked on <b>Notification</b>><b>KakaoTalk Bizmessage</b>> <b>Manage Sender Profile</b> tab.

2. Set event keys to collect statistical data by statistical event key.

3. If there is certain time you want to send, select Scheduled delivery and enter the date and time.

4. Get the template that inspection is completed.
    * Templates can be managed from <b>Notification</b>><b>KakaoTalk Bizmessage</b>><b>AlimTalk</b>><b>Manage Templates</b> tab.

5. Select whether to set an alternative sending message.

6. On the bottom <b>General Delivery</b> tab, click on <b>Add Recipient</b> to enter the replacement value that exists in the template and enter the recipient number.

7. After completing the entry, click on <b>Send</b> to send it immediately.

### Mass Delivery
It is the feature to send AlimTalk to multiple recipient numbers through an Excel/CSV format template file. Select <b>Mass Delivery </b> from bottom tab. 
![KTB_10_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_10_20230926.png)
* Select a template and click on <b>Download Template</b> to download the CSV, XLSX files containing the template replacement. If the template replacement is not included, the delivery will be processed as fail.
* If you open CSV file as Excel and save it, Hangul may not be saved normally. It is recommended to inspect and send it to check if it has been replaced normally.
* Files can be uploaded up to 30MB in CSV, XLSM format. The maximum number of recipients that can be sent is 1,000,000.
* If you select <b>Split Delivery</b>, you can send AlimTalk by specifying <b>Split Count</b> and <b>Delivery interval</b>.

![KTB_11_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_11_20230926.png)
* In the recipient file, enter and upload the required information for template delivery, such as the recipient number, variable (if any).

If you click on <b>Send</b> button, you can select <b>Inspect and proceed</b> or <b>Immediate Delivery</b>.

* Inspect and proceed: <b>View Mass Delivery</b> tab, check the mail recipient within 7 days and send AlimTalk. (Not supported for scheduled delivery.)
* Immediate Delivery: Send immediately without checking AlimTalk recipient. Delivery status can be found on the <b> View Mass Delivery </b> tab.


### Alternative Delivery
It is the feature to send a text message alternatively if you fail to send AlimTalk. 
<b>Send with common content </b> or <b>Set different messages for each user </b> can be selected to send. 
![KTB_12_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_12_20230926.png)
* For alternative delivery, <b>Manage Alternative Delivery</b> tab should be enabled and SMS service should be used. 
* Alternative sending requires a sender number. If you do not set an alternative sending status or a sender number, the default settings specified on the <b> Manage Alternative Delivery</b> tab apply.
* It will be sent alternatively after the KakaoTalk delivery result has been changed to a failure and it may take up to tens of seconds to receive a text.
* Depending on the message, some contents such as buttons and links may look different from KakaoTalk messages.
* If you delete or block the selected sender number, or stop using the SMS service, the text will not be replaced for sending and will need to be set up again.
* Depending on the length of the message, it will be replaced by SMS/LMS. (A separate fee will be charged for sending text messages for each type.)

#### Send with common contents
* Select <b>Sender Profile/Template</b> and click <b>Send with common contents</b> to set up an alternative sending message.
* If you do not enter an alternative delivery message, it will be sent as [Message Body].

#### Set different messages for each user
* Select <b>Sender Profile/Template</b> and click on <b>Set different messages for each user</b> to set alternative sending messages on <b>Add Recipients</b> tab at the bottom.
* Click on [Ballpoint Pen Icon] next to the added recipient number to view and modify the settings.

## View Delivery 
### Retrieve send results 
You can view based on the message type. 
(<b>Request ID</b> and <b>Request Date and Time</b> are required values.) 
![KTB_13_20250403.jpg](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_alimtalk/KTB_13_20250403.jpg)
* Details of each sending message can be found on the <b>View Details</b> screen by clicking on the search results item.
* <b>Request Date and Time </b> can be retrieved up to a month.
* Data displayed on full screen can be downloaded as an Excel file.
* You can check the status of the delivery request in <b>Request Status</b> column.
* In the <b>send results</b> column, you can see whether the shipment was successful or not.

### Cancel Send
During normal delivery, scheduled delivery with sending request date and time set to the future can be canceled. 
![KTB_14_20250403.jpg](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_alimtalk/KTB_14_20250403.jpg)
* You can see the check box on the left side of the request ID by querying the scheduled delivery request.
* The check box appears only for uncancelled scheduled requests.
* If you select the check box of the request you want to cancel and click on <b>Cancel Selected Schedule</b> at the top, the request will be canceled.
* By selecting the check box in the lookup list header, you can select or deselect the list entirely.


### View Mass delivery
You can View Mass delivery of AlimTalk. 
![KTB_15_20250403.jpg](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_alimtalk/KTB_15_20250403.png)
* View: You can look up mass AlimTalk messages sent in the inquiry form at the top. If you select the line of the inquiry list, you can check the receipt number and the sending information (sending contents, sending result) in the inquiry form at the bottom.
* Send/Cancel: When sending mass AlimTalk, select <b> Inspect and proceed</b> to send or cancel items that are <b>ready to send</b> by clicking on <b>Send</b> or <b>Cancel</b>.
* We recommend that you click on an item in the search results and check the replacement value in View in Detail window.
* If <b>Progress Status</b> is <b>Delivering</b>, even if you click on <b>Cancel</b> and some messages that were already being sent may be forwarded to the recipient.

#### Mass Delivery Progress Status 

* <b>Waiting</b>: Template file data are yet to be read.
* <b>Preparing</b>: Loading template file data.
* <b>Ready</b>: The template file data is all loaded and ready for dispatch. When you select a request (a row in the list), you can see the number received and what was sent in the list at the bottom.
* <b>Waiting for Delivery</b>: Delivery is yet to be processed.
* <b>Delivering</b>: Delivery is in progress.
* <b>Delivery Completed</b>: Request for SMS delivery has been properly completed.
* <b>Delivery Failed</b>: Error occurred during delivery.
* <b>Delivery Canceled</b>: User has canceled delivery.

#### Query Sending by Recipient
If you select Mass AlimTalk Delivery (row in the list), you can view the AlimTalk delivery contents and the results of the delivery by receiving number from the list below. 
![KTB_16_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_16_20230926.png)

You can select the recipient from <b>View by Recipient</b> to see if the delivery has been successfully replaced. 
![KTB_17_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_17_20230926.png)

## Template Management
You can register a template by clicking the <b>Register Template</b> button.

### Template type
* Registrable message types include **Channel Add type, Basic type, Additional information type, and Complex type**.
* Registrable highlights types include **Highlight type, Image type and Item list type **.
* You can select the type you want to send and create a template.

### Register Templates
* Kakao AlimTalk Guide
    * [[ AlimTalk Creation Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/content-guide), [[ AlimTalk Review Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit), [[ AlimTalk Whitelist]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit/white-list), [[ AlimTalk Blacklist]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit/black-list)
* Sender profile/Group
    * Select sender profile or sender profile group to which you want to register the template. If you register for a group, the template is available to all sender profiles in the group.
    * If the same template code exists in sender profile/group, the template registered in the sender profile is sent first.
    * For sender profile groups, templates cannot be registered as "Channel add type, complex type" among message types.
* Template Code / Template Name
    * Cannot register the same template code and template name in one sender profile/group.
* Template Content
    * AlimTalk can be written up to 1,000 characters including variables, URLs, spacing and button names, regardless of Korean/English. If you enter variables to register, please create a template by considering the content to be replaced.<br/> Refer to [AlimTalk Template Precautions ](https://docs.nhncloud.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-overview/#_3) for a detailed guide to the number of characters.
    * Create a variable in the form of #{variable}. (Example: #{HongGildong}'s package will be delivered today (#{09:50})
    * When registering a button, the button name cannot be entered as a variable, and the button url can be entered as a variable. (Example http://kakao.com/#{변수})
    * When registering a button url, url_mobile, url_pc links must include 'http://' and 'https://'가 and the 'scheme_ios, 'scheme_android links must be registered according to the scheme type. Otherwise, template registration is not possible.
* Whether it is a security template
    * When securing the template, the message content is not exposed on devices other than mobile (expose the phrase 'Please check on mobile')
    * In case of general message, the setting value may change during Inspection and please check OTP, authentication number, password, credit information/grade change guide template.

#### Template button
* You can register **up to 5 buttons** on a template.
* Quick Reply
    * Tracking and plug-in types are not available and other types can be used the same as buttons.
    * You can register up to 10 buttons on a template, and the number of buttons is limited to two when using Quick Reply.

| Button type | Description |
| --- | --- |
| View delivery  | - Go to Courier’s Delivery Inquiry page.<br/> - Check the invoice number pattern for each courier and courier that supports the tracking button. [[AlimTalk Inquiry Invoice Number Pattern Guide]](https://www.nhncloud.com/kr/support/notice/detail/1455)|
| Web link | - Go to your mobile or PC webpage.<br/> - You can set the URL link as a variable. |
| App link | - Run the app on custom scheme.<br/> - You must set each custom scheme to run on Android and iOS. |
| Bot Keyword | - The name of the button is forwarded to consultation agent.<br/> - If a channel that does not support consultation talk adds the corresponding button, AlimTalk cannot be sent. |
| Message forwarding | - The name of the button, the body of the message, is forwarded to consultation agent.<br/> - If a channel that does not support consultation talk adds the corresponding button, AlimTalk cannot be sent. |
| Switch to a consultation talk | -Connect to consultation talk<br/> - If a channel that does not support consultation talk adds the corresponding button, AlimTalk cannot be sent. |
| Switching to Bot | -Chatbot is called<br/> - If a channel that does not support consultation talk adds the corresponding button, AlimTalk cannot be sent. |
| Add channel | - Add the channel that sent AlimTalk. The exposure location is only available on the first button.<br/> - If a channel has already been added, it is not visible to the recipient. |
| Image Secure Transfer Plugin | - If image contains sensitive information, it encrypts and transfers within the chat window.<br/> - You need to create Bizplugin. [[Bizplugin Guide]](https://business.kakao.com/info/talkbizplugin/) |
| Personal Information Use Plug-in | - In the chat window, consent is obtained to collect personal information necessary to provide services without signing up for membership.<br/> - You need to create Bizplugin. [[Bizplugin Guide]](https://business.kakao.com/info/talkbizplugin/) |
| One-Click Payment Plugin | - Users can pay for the product without changing screens within the chat window.<br/> - As payment plug-in is not yet registered directly on the platform, please contact [Kakao Customer Center](https://cs.kakao.com/helps?service=127&category=572&locale=ko)  | 
| Business Form | - If you created a business form and connected it to the current channel, the business form you set is called when you click the button.<br/> - Business Form Creation is required: [[Business Form Guide]](https://business.kakao.com/info/talkbizform/)

#### Template inspection
The inspection and review of AlimTalk template will be conducted directly by Kakao, and will be processed sequentially within two business days after the inspection request.

* Register a template inquiry
    * If you have any comments you would like to deliver to the Kakao inspection representative before requesting an inspection, type in the Enquiry Registration box. Templates in the Request/Approval status cannot be inquired. (Only templates in the status of <b><span style="color:red">Inquiry/Return</span></b> can be registered.)
    * Enquiries registered will be added to the inspection results, which will be confirmed by the Kakao inspection representative.
    * Inquiries about the purpose of the template and reasons for return will be added to the inspection results.
    * If you reject the template, you can re-examine it by clicking **Register** and **Modify**.

#### Template status
* When registering a template, it is updated in the order of **Request > Under Inspection > Approval/Return** status.
* After registering the template, it will remain the same for a year or will switch to **Dormant** status, if there is no additional sending. Please check [AlimTalk Template Precautions](https://docs.nhncloud.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-overview/#_3) for related guides.

#### Modify Templates
* You can modify only templates in ** Approval/Return ** state.
* When the re-inspection is complete after modifying the approved template, the existing template contents are replaced with the modified one.
* Sender profiles/groups and template codes cannot be modified.
* Modified templates will be inspected again from ** Under Inspection** status.

#### Delete Templates
* You can delete only templates with <b><span style="color:red">Request/Return</span></b> status.
* The returned template can be re-registered after **Delete**.
* Deleted template code can be reused.

## Manage Alternative Delivery

* If AlimTalk fails to send, you can set it to be sent alternatively as a text message.
* The NHN Cloud SMS service must be in use and will be sent by SMS/LMS depending on the length of the message. (A separate fee will be charged for sending text messages for each type.)
* Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). 
* Subjects or contents that exceed the byte limit for the specified alternative sending type may be cut short (see [SMS Precautions](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1))
* Modifying the SMS app key will reset the alternative sending settings for all search IDs.
* The content for alternative sending is sent in EUC-KR, and unsupported emoji will fail for alternative sending.
* You can modify <b>alternative sending</b> and <b>sender number</b> by clicking<b>modify</b> to modify the search ID's channel to modify the alternative sending settings. 
* If <b> Alternative sending or not</b> is set to <b>Disabled </b>, the channel of the search ID cannot be sent alternatively when sending AlimTalk message.

![KTB_19_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_19_20230926.png)

## Manage sender profile group
* You can group two or more sender profiles to register templates at once.
* One sender profile can belong to multiple groups in duplicate.
* Sender profile group ID can be up to 10 characters. (Korean/English)
* It takes 3 to 4 business days to add a sender profile group.

![KTB_20_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_20_20230926.png)
