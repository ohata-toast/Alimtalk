## Notification > KakaoTalk Bizmessage > Plus Friend> Console Guide

## Register/Authenticate Sender Profiles
* To send a KakaoTalk Biz message, you must first register your sender profile.
* KakaoTalk channel can be created free of charge in KakaoTalk homepage (https://center-pf.kakao.com).
* Only business-certified KakaoTalk channels can be added to NHN Cloud Kakaotalk Bizmessage service (Refer to [PlusFriend Business Authentication](https://static.toastoven.net/prod_alimtalk/plusfriend_business_certify_guide_20190311.pdf))

## Add a Sender Profile

Once the sender profile registration is complete, a KakaoTalk token message will be sent to administrator's phone. 
KakaoTalk token messages are delivered only to mobile phones registered as administrators.

![Add a Sender Profile](https://static.toastoven.net/prod_alimtalk/plusfriend_01_201904.png)

* PlusFriend ID requires you to enter the search ID you registered when opening PlusFriend.
* KakaoTalk Biz message that the customer receives will be indicated by the name of Plus friend registered on KakaoTalk.

## Token Registration

If you enter the token message you received on your administrator's phone, your registration will be complete.

![Token Registration](https://static.toastoven.net/prod_alimtalk/plusfriend_02_201904.png)

<b><span style="color:red">When registering a sender profile, the initial daily maximum volume of delivery is limited to 1,000.</span></b> 
If you want to change the maximum daily delivery, you will need to request it separately from customer center (support@toast.com).

## Manage a Fallback Delivery

You can set up an **fallback delivery** for each sender profile.

* Only messages from sender profiles with delivery failure settings are replaced by LMS or SMS.
* When you modify the SMS app key, the sender failure setting for all sender profiles is initialized.

![Manage a Fallback Delivery](https://static.toastoven.net/prod_alimtalk/plusfriend_03_201812.png)

## View Kakao Statistics

Click **Go to Kakao Statistics** in Sender Profile Management to view Kakao statistics in a new window. Statistics criteria include delivery statistics and template statistics, and the query conditions vary depending on the message channel. You can view the results in charts and tables.

* Real-time statistics are not provided. Data collected the previous day is provided daily at around 7:00 AM.
* AlimTalk statistics are first provided on D+1 and finalized on D+2.
* Valid opens are not counted more than once for the same message.
* Clicks are counted multiple times for the same message.
* If the number of successful deliveries is 10 or fewer, valid opens and clicks are not provided.

### Delivery Statistics

Retrieves the daily delivery count, valid opens, and clicks by sender profile. You can query by setting conditions such as period, delivery identifier, and message type.

### Template Statistics

Retrieves the daily delivery count, valid opens, and clicks by template and group tag. You can query by setting conditions such as period and message type.

* Brand message free format is provided only when a group tag is used.

## Group Tag Management

Group tags are identification tags used when querying template statistics for brand messages. Click the **Group Tag Management** tab in the new **Go to Kakao Statistics** window to manage group tags.

<b><span style="color:red">Group tags can only be used for brand messages. AlimTalk is not applicable.</span></b>

* Click **+ Register Group Tag** to enter a group tag name and register it.
* Select the checkbox of the group tag to modify or delete, and click **Modify Group Tag** or **Delete Group Tag**.

## Notice of Personal Information Consignee
When a ‘customer’ uses the NHN Cloud > Notification > KakaoTalk Bizmessage service, there is a consignment relationship between the ‘customer’ and the ‘company’ regarding the processing of personal information. Therefor, in line with the Information and Communication Network Act and the Personal Information Protection Act, the ‘customer’ who is the consignor must disclose the status of entrusting personal information to the ‘company’ (trustee and details of work) through the personal information processing policy.

The ‘Company’ guides ‘Customer’ to comply with the relevant laws and regulations when using NHN Cloud's KakaoTalk Bizmessage service and not to be penalized due to non-disclosure of consignment status as follows.

Example<br>
[Notice of Personal Information Consignee]<br>
When using the KakaoTalk Bizmessage service, please indicate the following information in the 'Personal Information Processing Policy > Consignment Status' operated by the customer.<br>
Consignee: NHN<br>
Business details: KakaoTalk Biz messaging agent<br>