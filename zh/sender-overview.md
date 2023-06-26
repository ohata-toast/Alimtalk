## Notification > KakaoTalk Bizmessage > Plus Friend > Overview

### Creating a PlusFriend
To send KakaoTalk Bizmessages, a Plus Friend must be registered first. Plus Friend referst to a business ID of KakaoTalk, which is available for free on its website. <a target="_blank" href="https://center-pf.kakao.com">[Apply for Plus Friend]</a>

<b>* As of May 24, 2017, Yellow ID is changed to Plus Friend.  </b>

![[Figure 1] Creating Plus Friend](http://static.toastoven.net/prod_alimtalk/plus_friend_overview_01.png)

<b> For authentication, settings must be changed to **Open Home to the Public**  after registration. </b>
![[그림 2] 홈 공개 변경](http://static.toastoven.net/prod_alimtalk/plus_friend_overview_02.png)

### Status of Plus Friend
#### Status of NHN Cloud Plus Friend
* Refers to the status of Plus Friend who is registered to NHN Cloud.
* Register your Plus Friend and authenticate tokens, in reference of the console guide, and the service is enabled.

#### Profile Status of KakaoTalk Plus Friend
* Refers to the status of sender key to send AlimTalk/FriendTalk.
* May be blocked if profile is not in use for a long time or business information is not consistent.
* If status is not normal, send request to NHN Cloud Customer Center, along with Plus Friend ID, to ask for the release of blockage.

#### Status of KakoTalk Plus Friend
* Refers to the status of KakaoTalk business ID.
* May be blocked if Plus Friend is not in use for a long time.
* If status is not normal, send request to NHN Cloud Customer Center, along with the Plus Friend ID, for the release of blockage.

### 카카오톡 스팸 상태
#### 카카오톡 채널 스팸 상태
* 카카오톡 발신 프로필의 스팸 상태를 의미합니다.
* 비즈메시지 운영정책을 위반할 경우 스팸 채널로 분류되어 해당 프로필을 이용한 활동이 제한될 수 있습니다.
* 스팸 상태가 정상이 아닌 경우 NHN Cloud 고객 센터로 플러스친구 ID와 함께 차단 해제를 요청해 주시기 바랍니다.

#### 카카오톡 메시지 스팸 상태
* 카카오톡 발신 프로필에서 발송하는 메시지의 스팸 상태를 의미합니다.
* 비즈메시지 운영정책을 위반할 경우 스팸 채널로 분류되어 해당 프로필을 이용한 활동이 제한될 수 있습니다.
* 스팸 상태가 정상이 아닌 경우 NHN Cloud 고객 센터로 플러스친구 ID와 함께 차단 해제를 요청해 주시기 바랍니다.

### 최초 사용자 제한
특정 기준을 충족하지 못한 발신 프로필은 어뷰징 방지를 위해 최초 사용자 제한이 적용되어 일부 기능에 제약을 받게 됩니다.

### Restrictions for first user
Sending Profiles that do not meet certain criteria are subject to first user restrictions to prevent abuse, which limits some features.
#### Restriction items for first user
1. Restriction on the daily amount of sending.
2. Unable to add as a member to Group Profile
3. When template variable is replaced and if the difference is greater than 14 characters, process it as message sending failure

#### Release Criteria for first User Restrictions
1. First user restriction is to be automatically lifted, when a newly registered Sending Profile and if Sending profile with the first user restriction lifted is existing in the same project.
2. If there are more than 10 charging normally Sending Templates within a month of restriction creation of the first user Sending Profile, the restriction will be automatically lifted the next day.
3. Other than that, if you need to release the first user restriction, please contact us separately.
