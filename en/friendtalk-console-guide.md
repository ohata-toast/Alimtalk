## Notification > KakaoTalk Bizmessage > FriendTalk > Console Guide

## Send FriendTalk

### General Delivery

You can send a message in the form of a FriendTalk by setting the sender profile and entering the content. 
To send a FriendTalk, select **Notification > KakaoTalk Bizmessage > FriendTalk** from the console.

![friendtalk_01_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_01_20231227.png)

1. Select a sender profile. 
    * Sender profile can be checked on <b>Notification</b>><b>KakaoTalk Bizmessage</b>> the <b>Manage sender profile</b> tab.

2. Set event keys to collect statistical data by statistical event key.

3. If there is advertising information (special price, discount, event, promotion, PR, etc.) in the content to be sent, select <b>Advertising or not</b> as <b>Advertising</b> and if not, select <b>Not Advertising</b> and write the content.
    * Alternative sending may fail if 080 Unsubscription Incoming number is not set for alternative delivery of an advertising message.

4. If there is certain time you want to send, select Scheduled delivery and enter the date and time.
    * Scheduled delivery can be set up to one month.

5. Enter the content.

6. Select a message type. For a detailed guide to each type, see [Supported types for FriendTalk](https://docs.nhncloud.com/ko/Notification/KakaoTalk%20Bizmessage/ko/friendtalk-overview/#_3).
    * Basic (text/image/wide image)
        * Text: 1,000 characters of text, including spaces, regardless of Korean/English + up to 5 link buttons (vertically arranged)
        * Image: 400 characters of text, including spaces, regardless of Korean/English + 1 image + up to 5 link buttons (vertically arranged)
        * Wide image: 76 characters of text, including spaces, regardless of Korean/English + 1 image + 1 link button
    * Wide item list
        * It is an advertisement-type product that allows you to add three to four lists (image + item) to one title.
        * Text with spacing, regardless of Korean/English, first item title 25 characters, 2nd to 4th item title 30 characters + 3 to 4 image items + up to 2 link buttons (horizontal alignment)
    * Carousel feed
        * It is an advertisement-type product that can contain up to 10 images and various text information.
        * Up to 10 items consisting of 20 subject text + phrase text 180 characters + image + link buttons (horizontal alignment), including spaces, regardless of Korean/English
    * Premium video
        * The type of video you attached that will automatically play in the speech bubble.
        * The video link can only be used for videos uploaded to Kakao TV (e.g. https://tv.kakao.com/v/#{숫자} / https://tv.kakao.com/channel/#{숫자}/cliplink/#{숫자}).
        * 'Header' 20 characters of text + 'Copy' 76 characters of text + 1 video uploaded to Kakao TV + 1 link button
    * Commerce
        * A speech bubble that can be used to highlight pricing and discount information for a product.
        * 20 characters of text for "Title" + 34 characters of text for "Additional Info" + up to 2 link buttons (horizontally aligned) with no spaces, regardless of Korean/English
    * Carousel commerce
        * A speech bubble that allows you to organize information about different products into a catalog.
        * Up to 10 items consisting of 30 characters of "Title" text + 34 characters of "Additional Info" text + up to 2 link buttons (horizontally aligned), with no case-sensitive spacing
        * All images used in carousel commerce must have the same proportions.

7. Select an image, if any.
    * Before you can attach an image to a message, you must register the image on the <b>Image Management</b> tab.
    * Image Link: Enter the link to be linked when clicking on the image (URL containing http://,https://를)

8. You can insert web links, app links, bot keywords, message delivery, consultation talk switching, bot switching, and business form buttons.
    * Up to 5 basic types, up to 2 carousel/wide item lists

9. If you need to highlight a coupon in the message, you can use the button to click it to move to the attached coupon.

10. Select whether to set an alternative sending message.
* For alternative delivery, <b>Manage Alternative Delivery</b> tab should be enabled and SMS service should be used. 

11. Recipients can write in the form of mobile phone numbers.

12. Click <b>Send</b> to send after completing the entry.

### Mass Delivery

It is the feature to send AlimTalk to multiple recipients numbers through an Excel/CSV format template file. Select <b>Mass Delivery</b> from bottom tab.

![friendtalk_02_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_02_20231227.png)

* In the content, enter the template replacement in the form of '#{Name}', and then click **Download Template** button to download CSV, XLSX files containing template replacement.
* If you open CSV file as Excel and save it, Hangul may not be saved normally. It is recommended to ‘inspect and send’ it to check if it has been replaced normally.
* Files can be uploaded in CSV, XLSM format. The size is up to 30MB. The maximum number of recipients that can be sent is 1,000,000.
* If you select <b>Split Delivery</b>, you can send FriendTalk by specifying <b>Split Count</b> and <b>Send Interval<b>.

![friendtalk_03_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_03_20231227.png)

* For each recipient, you can enter the value of the template replacement value.
* If you enter content without a template replacement, it will be sent to all recipients with the same content.

If you click on <b>Send</b> button, you can select <b>Inspect and proceed</b> or <b>Immediate Delivery</b>.

* Inspect and proceed: <b>View Mass Delivery</b> tab, check the mail recipient within 7 days and send FriendTalk. (Not supported for scheduled delivery.)
* Immediate Delivery: Send immediately without checking FriendTalk recipient. Delivery status can be found on the <b> View Mass Delivery </b> tab.

### Alternative Delivery
It is the feature to send a text message alternatively if you fail to send FriendTalk. 
<b>Send with common content </b> or <b>Set different messages for each user </b> can be selected to send.

![friendtalk_04_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_04_20231227.png)

* For alternative delivery, <b>Manage Alternative Delivery</b> tab should be enabled and SMS service should be used. 
* Alternative delivery requires a sender number. If you do not set an alternative sending status or a sender number, the default settings specified on the <b> Manage Alternative Delivery</b> tab will apply.
* It will be sent alternatively after the KakaoTalk delivery result has been changed to a failure and it may take up to tens of seconds to receive a text.
* Depending on the message, some contents such as buttons and links may look different from KakaoTalk messages.
* Depending on the length of the message, it will be replaced by SMS/LMS. (A separate fee will be charged for sending text messages for each type.)

#### Send with common contents
* Select <b>Sender Profile</b> and click on <b>Send with common contents</b> to set up an alternative message.
* If you do not enter an alternative delivery message, it will be sent as [Message Body].

#### Set different messages for each user
* Select <b>Sender Profile</b> and click on <b>Set different messages for each user</b> to set alternative sending messages on <b>Add Recipients</b> tab at the bottom.
* Click on [Ballpoint Pen Icon] next to the added recipient number to view and modify the settings.

### Precautions for sending advertising messages
What is an advertising message? 
It corresponds to a message about information, goods, or services that the sender sends for the purpose of obtaining economic benefits.

<b>[Advertising Message Sending Guide]</b>
1) Advertising Display 
①(Advertising) must be displayed at the beginning of the message containing advertising content. 
② Criteria for judging advertising messages 
a. a special/discount product guide 
b. Promotions or events to promote goods and services 
c. Even if the most contents is about ‘information’, [a, b] above are mixed in the message. 
2) Sender's contact information mandatory item
① The sender name, sender contact information, must be included above the body of the message.
② For sender names and sender contacts, they are exposed at the top of the chat room. 
③ In the case of contact information, you must select either of the phone numbers or addresses and fill it in.
3) Mandatory requirements for indicating measures and methods to facilitate the expression of refuse to receive 
①Measures and methods to make it easier to refuse to receive and withdraw consent to receive messages should be specified in the body of the advertisement. 
② If the refusal of receipt or withdrawal of consent is not easily made or impossible by such measures and methods, it is considered not to be indicated. 
③ It is a violation of the law to have to take additional measures, such as having to log in to the website to refuse to receive e-mails, making it difficult to refuse to receive or withdraw consent to receive.

![[Figure 3] FriendTalk advertising message ](http://static.toastoven.net/prod_alimtalk/friendtalk_02.png)

* If you select <b>Advertising</b> from <b>Ad or not</b>, it is considered an advertising message, and the '(Advertising)' is displayed at the beginning of the message, and the method of refusing to receive (Home > Block Friend) is displayed. 
If an advertising message is sent after release it, you may be restricted to use of the Kakao Channel service.

## View Delivery 

### Retrieve send results 
You can view FriendTalk message in **Retrieve Send Results** tab.

![friendtalk_05_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_05_20231227.png)

* Details of each sending message can be found on the <b>View Details</b> screen by clicking on the search results item.
* <b>Registration Date and Time</b> and <b>Date and time of request</b> can be retrieved up to a month.
* Data displayed on full screen can be downloaded as an Excel file.
* You can check the status of the delivery request in <b>Request Status</b> column.
* In the <b>Send results</b> column, you can see whether the shipment was successful or not.

### Cancel Send
You can cancel scheduled delivery if the date and time of the sending request set to the future during normal delivery.

* You can check the check box on the left side of request ID by inquiring about scheduled delivery request.
* The check box appears only for uncancelled scheduled requests.
* Select the check box for the request you want to cancel and click the **Cancel Selected Schedule** button at the top to cancel the request.
* You can select and cancel the entire list through the check box in the query list header.

### View Mass Delivery
You can view the mass delivery of your FriendTalk.

![friendtalk_06_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_06_20231227.png)

* View: You can look up the mass delivery of FriendTalk in the inquiry form at the top. If you select the line of the inquiry list, you can check the receipt number and the sending information (contents, delivery results) in the inquiry form at the bottom.
* Send/Cancel: When sending mass delivery FriendTalk, select <b>Inspect and proceed</b> to send or cancel items that are <b> Ready to send</b> by clicking on <b>Send</b> or <b>Cancel</b>.
* We recommend that you click on an item in the search results and check the replacement value in View in Detail window.
* If <b>Progress Status</b> is <b>Delivering</b>, even if you click on <b>Cancel</b> and some messages that were already being sent may be forwarded to the recipient.

#### Mass Delivery Status

* <b>Waiting</b>: Template file data are yet to be read.
* <b>Preparing</b>: Loading template file data.
* <b>Ready</b>: The template file data is all loaded and ready for dispatch. When you select a request (a row in the list), you can see the number received and what was sent in the list at the bottom.
* <b>Waiting for Delivery</b>: Delivery is yet to be processed.
* <b>Delivering</b>: Delivery is in progress.
* <b>Delivery Completed</b>: Request for SMS delivery has been properly completed.
* <b>Delivery Failed</b>: Error occurred during delivery.
* <b>Delivery Canceled</b>: User has canceled delivery.

#### Query Sending by Recipient
If you select Mass FriendTalk delivery (row in the list), you can look up your FriendTalk delivery contents and sending result by recipients’ number from the list below.

![friendtalk_07_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_07_20231227.png)

On <b>View by receiver</b>, you can select the corresponding receiver to see if the delivery content has been successfully replaced.

![friendtalk_08_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_08_20231227.png)


## Image Management

### Register, delete, look up images
You can register or delete the images you want to use for FriendTalk and check the information of the registered images.

![friendtalk_09_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_09_20231227.png)

* You can select Images to register by type (basic/wide, wide item list type, carousel feed, carousel commerce, business form).
* You cannot use the image when the image is deleted.
* Make sure to comply with file specifications, recommended/restricted sizes.
* You can copy an image address URL, which is utilized when sending APIs.
* Business form is a business tool that supports event design such as schedule arrangements, surveys, and applications that KakaoTalk users can easily participate in and create and enter the corresponding ID. 
* Go to [Register Business Form](https://business.kakao.com/talkbizform/) 
* Go to [Business Form Registration Guide](https://kakaobusiness.gitbook.io/main/tool/bizform)

#### Range that allows Image upload 
* File format: JPG, PNG
* File size: General [5MB or less] / Wide [5MB or less] / Wide item list, carousel feed, carousel commerce [5MB or less]
* Size limit
- General [500px * 250px]
- Wide [800px * 600px]
- Wide item list [400px * 400px ~ 800px * 400px]
- Carousel feed, carousel commerce [width length 500px or more]
* Ratio: normal / carousel [width: length ratio is 2:1 or more and 3:4 or less]


## Manage Alternative Delivery

* It is the feature to send the same contents as a text message alternatively if you fail to send FriendTalk.
* The NHN Cloud SMS service must be in use and will be sent by SMS/LMS depending on the length of the message. (A separate fee will be charged for sending text messages for each type.)
* Modifying the SMS app key will reset the alternative sending settings for all search IDs.
* Since alternative delivery is made in the SMS service, field values must follow the API specifications for SMS (e.g. Sender number registered at the SMS service, or restriction in the field length). 
* Title or message of an alternative delivery may be cut in length, if the byte size exceeds restrictions (see [[Cautions for SMS](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])
* The content for alternative sending is sent in EUC-KR, and unsupported emoji will fail for alternative sending.
* You can modify <b>alternative sending</b> and <b>sender number</b> by clicking<b>modify</b> to modify the search ID's channel to modify the alternative sending settings. 
* If <b>alternative sending or not</b> is set to <b>Disabled </b>, the channel of the search ID cannot be sent alternatively when sending FriendTalk message.
* FriendTalk ad message can be replaced by Ad SMS API, so it must be registered at the 080 Unsubscription Service to enable alternative delivery.
* If you enter the resendContent field of FriendTalk ad message, <span style="color:red">Advertising phrase</span> of the SMS ad API is required to be replaced. ` (Advertising) Contents [Free Unsubscription] 080XXXXXX`
* If there is no resendContent field in the FriendTalk advertising message, the <span style="color:red">Advertising phrase </span> will be automatically generated and replaced with the registered 080 Unsubscription Number.

![friendtalk_10_20231227.png](https://static.toastoven.net/prod_alimtalk/friendtalk_10_20231227.png)

