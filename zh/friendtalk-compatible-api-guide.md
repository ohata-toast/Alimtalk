## Notification > KakaoTalk Bizmessage > FriendTalk > Brand Message Migration Guide

## Overview

* Kakao’s **FriendTalk service** will end on **Wednesday, December 31, 2025**.*
* Accordingly, **NHN Cloud** will discontinue FriendTalk on **Tuesday, December 30, 2025**.*

|  | Details |
| :--- | :--- |
| **Service End Date** | After the regular **Notification** maintenance on Tuesday, December 30, 2025 |
| **Impact Scope** | *All FriendTalk features, including message sending, will be discontinued.* |
| **Exception (Temporary Support)** | Only a **FriendTalk Compatibility Send** will be provided temporarily<br>(Send API, Send Query API, and result query via Console) |

To support a smooth migration to Brand Message, we provide two options:

1. **(Recommended) Migrate to the Brand Message Free-form Send API**
2. (Temporary) Use the FriendTalk Compatibility Send via the existing FriendTalk API

## 1. (Recommended) **Migrate to the Brand Message Free-form Send API**

* For more stable, long-term sending, we recommend **migrating directly to the Brand Message–specific API**.

### Benefits of Migrating to Brand Message

* With **I-Targeting**, the Brand Message Free-form Send offers usage identical to the existing FriendTalk Send API, so migration requires minimal effort.
* Brand Message is a superset of FriendTalk and supports all features previously available in FriendTalk.
* Brand Message allows advertising to recipients **who are not friends of your sending channel** by specifying targeting.
* For details, see the [Brand Message Guide](https://docs.nhncloud.com/zh/Notification/KakaoTalk%20Bizmessage/zh/friendtalkupgrade-overview/).

## 2. (Temporary) FriendTalk Compatibility Send

* The FriendTalk Compatibility Send converts a FriendTalk payload into the Brand Message format and delivers it.

### Items Provided by the FriendTalk Compatibility Send

| Item | Details |
| :--- | :--- |
| **Support Period** | About 1 year (planned: 2025-12-30 ~ 2026-12-31) |
| **Scope** | Existing FriendTalk **Send API / Query API / Console** |
| **Behavior** | You can continue to use the **same FriendTalk URI and JSON** format; responses also mirror FriendTalk |
| **Internal Flow** | FriendTalk payloads are forwarded to Kakao Brand Message’s **FriendTalk Compatibility Send API** |
| **Validation Rules** | The original FriendTalk validation rules are kept.<br>*However, sends may fail at delivery time due to Brand Message spec limits.* |

* The FriendTalk Compatibility Send has been available since **October 28, 2025**, and you can test sending now.
* If you plan to use compatibility send immediately after the fade-out, prior testing may be required.
* See the comparison below for spec differences.

### How to Use the FriendTalk Compatibility Send

* Add the header `X-Convert-To-Brand-Message: true` to your existing FriendTalk API requests to enable the compatibility send.
* Note: We do **not** provide a separate, explicit method to confirm that the compatibility path was used.
* The compatibility send is provided **regardless of API version**.

[Header]

    {
      "X-Convert-To-Brand-Message": Boolean
    }

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| X-Convert-To-Brand-Message | Boolean | X | Request header for FriendTalk compatibility send. *(From **December 30, 2025**, requests **without** `X-Convert-To-Brand-Message: true` will be rejected at the request stage.)* |

### Kakao-Provided Spec Change Comparison

![friendtalk_compatible_spec_01.png](https://static.toastoven.net/prod_alimtalk/friendtalk_compatible_spec_01.png)

### Other Confirmed Spec Differences

* **Carousel Feed**
    * Per-carousel button: **Optional → Required**
* **Carousel Commerce**
    * **Min 1 (including intro) ~ Max 10 → Min 2 ~ Max 6 (including intro)**
* Please note there may be additional, not-yet-verified differences.*