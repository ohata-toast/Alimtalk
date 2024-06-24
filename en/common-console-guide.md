## Notification > KakaoTalk Bizmessage > Plus Friend> Console Guide

## Identity Verification
* As a KakaoTalk policy to prevent abusing, the enhanced Identity verification of pre-registration system was applied to the KakaoTalk Bizmessage service.
    * Only for members who joined after December 15, 2023
* Identity verification basically requires mobile phone identification and additional document screening according to the member type.
    * Without identity verification, all features other than the <b>Identity verification tab</b> will be disabled.
* The name and mobile phone number entered at the time of membership must match the information entered at the time of identity verification to be approved for identity verification.
* Non-business individual members are not allowed to use the service.
* The certificate of employment is valid <b><span style="color:red">only documents with seal and marked with the date of issuance</span></b>. Make sure to mask (hide) the 6 digits after the resident number in the certificate of employment. Example) 000000-0\*\*\*\*\**

### Required Documents for Identity verification
| Member type   | Verification Method     | Required Documents         |
|---------|-----------|---------------|
| Business Representative  | Mobile Phone Identification | Business Registration Certificate, Certificate of Employment |
| Business Executives and Employees | Mobile Phone Identification | Business Registration Certificate, Certificate of Employment |

### Authentication representative
![KTB_01_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_01_20230926.png)
1. Select the <b>Identity Verification</b> tab.
2. Click on <b>identifying with your mobile phone and attaching the required documents</b> to begin the process
3. Agree after checking the consent to collect and use personal information.
4. Proceed with the identity verification process through mobile phone text authentication or simple identity verification.
5. If you need any necessary documents, register them as attached.
6. Wait for the operator inspection and approval process.
7. Once the identity verification process is completed, the approval results will be sent to the mail registered with your account.

### Identity Verification Status Settings
* Reviewing: The administrator is reviewing the authentication documents for registered identity verification.
* Rejected: A state in which identity verification has been rejected and documents must be re-registered.
* Approved: Identity verification approval completed

## Manage Sender Profiles

### Add Sender Profiles
Register your sender profile after opening KakaoTalk channel. Detailed guide to creating KakaoTalk channels can be found in [Sender Profile](https://docs.nhncloud.com/ko/Notification/KakaoTalk%20Bizmessage/ko/sender-overview/ ). 
Once the sender profile registration is complete, a KakaoTalk token message will be sent to the administrator's phone. 
![KTB_02_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_02_20230926.png)
1. Enter the ID for the search that starts with @.
2. Enter the mobile phone number you registered when you joined the KakaoTalk Channel Administrator Center.
    * You can check/modify your mobile phone number in [KakaoTalk Channel Administrator Center Login > Select E-mail at the top right > My Account Information].
3. Sets the category of the channel. It can be different from the category you set when opening the channel.
4. Click <b> Request Token</b> to request an authentication token.

### Register Token
If you enter the token message you received on your administrator's phone, your sender profile registration is complete.

![KTB_03_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_03_20230926.png)

* Enter the authentication token that you received as AlimTalk (Number of 6 digits)
* <b><span style="color:red">The initial daily maximum count of delivery limited to 1,000 when registering for a sender profile. </span></b>
* If you want to change the maximum daily delivery volume, you will need to request it separately to customer center.

### KakaoTalk Channel Status
#### NHN Cloud Sender Profile Status
* It means the status of sender profile registered with NHN Cloud.
* If you register a KakaoTalk channel and complete token authentication by referring to KakaoTalk channel creation and sender profile console usage guide, you can use it normally.

#### KakaoTalk Channel Profile Status
* It indicates the status of sender key for sending AlimTalk/FriendTalk.
* It may be blocked in the case of long-term non-use of profiles, inconsistency of business information, etc.
* If the condition is not normal, please request NHN Cloud Customer Center to release the block along with KakaoTalk Channel ID.

#### Kakao Channel Status
* This refers to the status of your KakaoTalk business ID.
* Sender profiles may be blocked for long-term non-use, etc.
* If the condition is not normal, please request NHN Cloud Customer Center to release the block along with KakaoTalk Channel ID.

### Sender profile status
#### Sender profile dormant status
* This means that the sender profile is dormant because there is no history of sending Kakao AlimTalk messages for one year.
* If you do not release the dormant status for 1 year after becoming dormant, the sender profile will be deleted.
* If the condition is not normal, please request NHN Cloud Customer Center to release the block along with KakaoTalk Channel ID.

#### Sender Profile Block Status
* This means that the sender profile is deleted, as the dormant status was not released for 1 year after becoming dormant. 
* Deleted sender profiles cannot be recovered and must be newly registered.
* Please refer to the sender profile Console User Guide to register additional sender profiles.

### KakaoTalk channel spam status
#### KakaoTalk channel spam status
* It indicates the spam status of KakaoTalk sender profile.
* If violated the BizMessage operational policy, it leads to be classified as a spam channel and restrict activity using such profile.
* If the spam status is not normal, please request NHN Cloud Customer Center to release the block along with the KakaoTalk channel ID.

#### KakaoTalk message spam status
* This refers to the spam status of messages sent by the KakaoTalk sender profile.
* If violated the BizMessage operational policy, it leads to be classified as a spam channel and restrict activity using such profile.
* If the spam status is not normal, please request NHN Cloud Customer Center to release the block along with the KakaoTalk channel ID.

### First-time user restrictions
Sender profiles that do not meet certain criteria are subject to first-time user restrictions to prevent them from being abused, limiting some features.

#### First-time user restriction items
* Daily delivery Limit
* Not allowed to add as a member to a group profile
* When a template variable is replaced, if there is a part where the difference exceeds 14 characters, the message is to be processed as delivery failure. 

#### Release criteria for First-time user restrictions
* The restriction is automatically lifted the following day if the sender profile of first-time user has more than 10 normal billing sending templates within a month of creation of such limit.
* In addition, if you need to lift the user restriction for the first time, please contact NHN Cloud Customer Center separately along with the KakaoTalk channel ID.


## Sending Settings
* Depending on the message retention period policy, you can back up sending history data that is older than 90 days.
* If you enter information about whether to back up AlimTalk, the file extension, and the storage to upload the file to, a file containing the backup date will be created in that storage.

### Data Retention Period
* Retains the sending history for the last 90 days in accordance with the data retention policy.
* Statistical data stores information from the last 90 days.
* The images uploaded to the Kakao service will be permanently stored on Kakao CDN server.

## Webhook Management
You can receive webhook events by specifying a URL when the specified event occurs. 
![KTB_04_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_04_20230926.png)
1. Select the event type to register.
2. Enter URL address where you can receive the data to be sent to webhook.
3. Enter the webhook signature to register (not required).
4. Get authenticated and click on <b>Add</b> to register webhook.

Registered webhooks can be checked in the <b>webhook registration list</b>.

## Statistical Event Key Settings
When registering an event key and sending with that key, you can collect statistical data by statistical event key.
Please refer to the <b>note</b> for the meaning of statistical event key terms.
![KTB_05_20230926.png](https://static.toastoven.net/prod_alimtalk/KTB_05_20230926.png)
1. <b>Set the data collection period </b>.
2. Enter a name and <b>detailed description</b> for the <b>statistical event key</b>.
3. Click <b>Save<b> and the event key will be registered.

When the data collection period ends, it is disabled and will no longer accumulate data. 
<b>The ending point of time of data collection period can be modified if it is active.</b>

## Statistics
### Query Statistics
* You can inquire statistics by type, such as the sending request period, statistical event key, template and etc.
* You can check the status of delivery with graphs and tables, such as sending requests, successes, failures and etc.

#### Categorize Statistics
* Message (event time): Statistics collected based on the time the event occurred.
* Statistics are collected as of the following times
    * Request: Time of successful request
    * Delivery: Point of time when send as a vendor
    * Delivery Failed: Delivery request failed
    * Resending Successful: Point of time when request alternative sending
    * Resending Failed: Point of time when fail to request alternative sending
    * Received: Delivery Result Successful (Received Time)


## [Note]
### Statistics Event Keys and Statistics

| Term       | Description                                    |
|----------|---------------------------------------|
| Statistical Event Key | This is an event key used when you want to view statistics grouped into specific units. |
| statsId  | Unique ID of the statistical event key. This value is mainly used when calling the API. |

If you want to extract statistics in a specific unit when sending AlimTalk messages

1. Register statistical event keys in the <b>statistical event key management<b> tab. If sending using the API, you must obtain the statistics ID (statsId) from this screen.

2. When sending a message from the console or to the API, you must also send the statistics event key.

    1. When sending from the console
        * On the <b>Send AlimTalk</b> tab, select a statistical event key when sending a text
        * After you complete the message information, click <b>Send</b>.
        * <b>Statistics</b> tab allows you to view statistics after a certain period of time.

    2. 2-2. When sending via API
        * Enter the statsId obtained from the <b>statistics event key management<b> tab into the message transmission parameters.
        * You can check statistical information after a certain period of time in the <b>Statistics<b> tab.
