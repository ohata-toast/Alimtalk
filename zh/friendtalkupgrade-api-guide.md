## Notification > KakaoTalk Bizmessage > 브랜드 메시지 > API v1.0 Guide

## 브랜드 메시지

#### [API 도메인]

| 도메인                                                                          |
|------------------------------------------------------------------------------|
| [https://api-alimtalk.cloud.toast.com](https://api-alimtalk.cloud.toast.com) |

## v1.0 API 소개

## 비친구 메시지 발송(타겟팅 M, N) 관리

비친구 메시지 발송(타겟팅 M, N)은 아래 조건을 모두 만족할 경우 발송할 수 있습니다.

- 비즈니스 인증 채널
- 사업자번호 등록
- 채널 고객센터 전화번호 등록
- 채널 친구 수 5만 이상
- 3개월 내 알림톡 발송 성공 이력 보유

### 마케팅 수신동의 증적 자료 업로드

#### 요청

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Request parameter]

| 이름   | 타입   | 필수 | 설명            |
|------|------|----|---------------|
| file | File | O  | 마케팅 수신동의 증적자료 |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

### 비친구 메시지 발송(타겟팅 M, N) 사용 신청

#### 요청

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

## 메시지 자유형 발송 요청

* 마수동 발송을 사용할 수 있습니다.
    * targeting 필드를 지정해 메시지 대상의 타입을 지정할 수 있습니다.
        * M: 고객사의 광고성 정보 수신동의 유저(카카오톡 수신 동의)
        * N: 고객사의 광고성 정보 수신동의 유저(카카오톡 수신 동의) - 채널 친구
        * I: 고객사의 발송 요청 대상 ∩ 채널 친구
* 기존 친구톡의 8가지 메시지 유형을 전부 사용할 수 있습니다.
* BC, BT 버튼 타입을 사용할 수 있습니다.
* AC(채널 추가)버튼은 사용할 수 없습니다.
* BF 버튼 사용 시 카카오에서 발급받은 비즈니스폼 ID를 넣어서 사용할 수 있습니다.
* 대체 발송은 수신자별 resendParameter를 통해 설정할 수 있습니다.
    * 대체 발송을 이용하실 경우 대체 발송 관리 API를 통해 SMS Appkey 등록 및 발송 설정이 필요합니다.
* <b>야간 발송 제한(20:50~다음 날 08:00)</b>

#### 요청

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 텍스트형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "TEXT",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String, 
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음. 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음                           |
| buttons                | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개, 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 이미지형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "IMAGE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음. 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음                           |
| image                  | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 이미지 URL. 일반 이미지로 업로드된 이미지 URL 사용                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | 이미지 클릭 시 이동할 URL. 1000자 제한<br>미 설정 시 카카오톡 내 이미지 뷰어 사용                                                                                                                                                                                                                            |
| buttons                | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개, 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 와이드 이미지형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음. 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음                           |
| image                  | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 이미지 URL, 와이드 이미지로 업로드된 이미지 URL 사용                                                                                                                                                                                                                                             |
| - imageLink            | String  | X  | 이미지 클릭 시 이동할 URL. 1000자 제한<br>미 설정 시 카카오톡 내 이미지 뷰어 사용                                                                                                                                                                                                                            |
| buttons                | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개, 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 와이드 아이템 리스트형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| header                 | String  | O  | 헤더<br>- WIDE_ITEM_LIST 타입일 경우 필수 필드이고 최대 20자(줄바꿈: 불가)<br>- PREMIUM_VIDEO 타입일 경우 선택 필드이고 최대 20자(줄바꿈: 불가)                                                                                                                                                                     |
| item                   | Object  | O  | 와이드 리스트 요소(WIDE_ITEM_LIST 타입에서만 사용 가능)                                                                                                                                                                                                                                       |
| - list                 | List    | O  | 와이드 리스트(최소: 3, 최대 4)                                                                                                                                                                                                                                                         |
| -- title               | String  | O  | 아이템 제목<br>- 1번째 아이템은 최대 25자 제한(줄바꿈: 최대 1개, 1번째 아이템의 경우 title이 필수 값이 아님)<br>- 2~4번째 아이템 최대 30자 제한(줄바꿈: 최대 1개)                                                                                                                                                                |
| -- imageUrl            | String  | O  | 아이템 이미지 URL<br>- 1번째 아이템에는 첫번째 와이드 아이템리스트 이미지로 업로드된 이미지 URL 사용<br>- 2~4번째 아이템은 일반 와이드 아이템리스트 이미지로 업로드된 이미지 URL 사용                                                                                                                                                             |
| -- linkMo              | String  | O  | 모바일 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | 안드로이드 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | IOS 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| buttons                | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개, 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 프리미엄 동영상형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| content                | String  | X  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈 : 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음, 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음                           |
| header                 | String  | X  | 헤더<br>- WIDE_ITEM_LIST 타입일 경우 필수 필드이고 최대 20자(줄바꿈: 불가)<br>- PREMIUM_VIDEO 타입일 경우 선택 필드이고 최대 20자(줄바꿈: 불가)                                                                                                                                                                     |
| video                  | Object  | O  | 동영상 요소(PREMIUM_VIDEO 타입만 사용 가능)                                                                                                                                                                                                                                              |
| - videoUrl             | String  | O  | 카카오TV 동영상 URL(카카오TV에 업로드된 동영상 주소만 사용 가능), 최대 500자 제한                                                                                                                                                                                                                         |
| - thumbnailUrl         | String  | X  | 동영상 섬네일용 이미지 URL, 일반 이미지로 업로드된 url만 사용 가능(없는 경우 카카오TV 동영상 기본 섬네일 사용) , 최대 500자 제한                                                                                                                                                                                            |
| buttons                | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개, 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 커머스형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| additionalContent      | String  | X  | 부가 정보(최대 34자, 줄바꿈: 최대 1개), 커머스형에서만 사용 가능                                                                                                                                                                                                                                      |
| image                  | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 이미지 URL. 일반 이미지로 업로드된 이미지 URL 사용                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | 이미지 클릭 시 이동할 URL. 1000자 제한<br>미 설정 시 카카오톡 내 이미지 뷰어 사용                                                                                                                                                                                                                            |
| commerce               | Object  | O  | 커머스(COMMERCE 타입에서만 사용 가능)                                                                                                                                                                                                                                                    |
| title                  | String  | O  | 상품 제목(최대 30자, 줄바꿈: 불가)                                                                                                                                                                                                                                                       |
| regularPrice           | Integer | O  | 정상 가격(0~99,999,999)                                                                                                                                                                                                                                                        |
| discountPrice          | Integer | X  | 할인가격(0~99,999,999)                                                                                                                                                                                                                                                          |
| discountRate           | Integer | X  | 할인율(0~100), 할인가격 존재 시 할인율. 정액할인가격 중 하나는 필수                                                                                                                                                                                                                                   |
| discountFixed          | Integer | X  | 정액할인가격(0 ~ 999,999), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수                                                                                                                                                                                                                            |
| buttons                | List    | O  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                                                                 |
| - name                 | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| - type                 | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BC/BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| - description          | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자. 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자. 줄바꿈: 불가                                                                                                                                                                      |
| - linkMo               | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - linkPc               | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| - schemeIos            | String  | X  | iOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 캐러셀 피드형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
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
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자). 그룹 발신 키는 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                  | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| carousel               | Object  | O  | 캐러셀                                                                                                                                                                                                                                                                           |
| - list                 | List    | O  | 캐러셀 리스트(최소 2개, 최대 6개)                                                                                                                                                                                                                                                        |
| -- header              | String  | O  | 캐러셀 아이템 제목(최대 20자). 캐러셀 피드형에서만 사용 가능                                                                                                                                                                                                                                          |
| -- message             | String  | O  | 캐러셀 아이템 제목(최대 20자), 캐러셀 아이템 메시지(최대 180자). 캐러셀 피드형에서만 사용 가능                                                                                                                                                                                                                    |
| -- imageUrl            | String  | O  | 이미지 URL(캐러셀 피드형 이미지로 업로드된 이미지만 사용 가능)                                                                                                                                                                                                                                        |
| -- imageLink           | String  | X  | 이미지 링크, 1000자 제한                                                                                                                                                                                                                                                              |
| -- buttons             | List    | O  | 캐러셀 리스트 버튼 목록 최소 1개, 최대 2개                                                                                                                                                                                                                                                    |
| --- name               | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| --- type               | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| --- linkMo             | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| --- linkPc             | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| --- schemeIos          | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| --- chatExtra          | String  | X  | BC / BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| --- chatEvent          | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| --- bizFormKey         | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| -- coupon              | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| --- title              | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| --- description        | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                                                      |
| --- linkMo             | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| --- linkPc             | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| --- schemeIos          | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - tail                 | Object  | X  | 더보기 버튼 정보                                                                                                                                                                                                                                                                     |
| -- linkMo              | String  | O  | 모바일 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | 안드로이드 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | IOS 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS, LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 대체 발송 080 수신거부번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 캐러셀 커머스형 발송 요청

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
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
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
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
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                   | 타입      | 필수 | 설명                                                                                                                                                                                                                                                                            |
|----------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey            | String  | O  | 발신 키(40자), 그룹 발신 키 사용 불가                                                                                                                                                                                                                                                       |
| chatBubbleType       | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm            | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                                                                   |
| requestDate          | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| adult                | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                                                       |
| carousel             | Object  | O  | 캐러셀                                                                                                                                                                                                                                                                           |
| - head               | Object  | X  | 캐러셀 인트로                                                                                                                                                                                                                                                                       |
| -- header            | String  | O  | 캐러셀 인트로 헤더(최대 20자)                                                                                                                                                                                                                                                            |
| -- content           | String  | O  | 캐러셀 인트로 내용(최대 50자)                                                                                                                                                                                                                                                            |
| -- imageUrl          | String  | O  | 캐러셀 인트로 이미지 주소(캐러셀 커머스형 이미지로 업로드된 이미지 사용, 사용되는 이미지는 캐러셀의 이미지와 비율이 동일해야 함)                                                                                                                                                                                                    |
| -- linkMo            | String  | X  | 모바일 웹 링크(linkMo, linkPc, schemeAndroid, schemeIos 중 하나라도 사용하려는 경우 linkMo은 필수 값), 1,000자 제한                                                                                                                                                                                    |
| -- linkPc            | String  | X  | PC 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| -- schemeAndroid     | String  | X  | 안드로이드 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | IOS 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| - list               | List    | O  | 캐러셀 리스트(head가 존재할 경우 최소 1개, 최대 5개 / 그 외에는 최소 2개, 최대 6개)                                                                                                                                                                                                                      |
| -- additionalContent | String  | X  | 부가 정보(최대 34자), 캐러셀 커머스형에서만 사용 가능                                                                                                                                                                                                                                              |
| -- imageUrl          | String  | O  | 이미지 URL(캐러셀 커머스형 이미지로 업로드된 이미지 사용)                                                                                                                                                                                                                                           |
| -- imageLink         | String  | X  | 이미지 링크, 1000자 제한                                                                                                                                                                                                                                                              |
| -- commerce          | Object  | O  | 커머스(CAROUSEL_COMMERCE 타입에서만 사용 가능)                                                                                                                                                                                                                                           |
| --- title            | String  | O  | 상품 제목(최대 30자, 줄바꿈: 불가)                                                                                                                                                                                                                                                       |
| --- regularPrice     | Integer | O  | 정상 가격(0~99,999,999)                                                                                                                                                                                                                                                        |
| --- discountPrice    | Integer | X  | 할인가격(0 ~ 99,999,999)                                                                                                                                                                                                                                                          |
| --- discountRate     | Integer | X  | 할인율(0 ~ 100), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수                                                                                                                                                                                                                                   |
| --- discountFixed    | Integer | X  | 정액할인가격(0 ~ 999,999), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수                                                                                                                                                                                                                            |
| -- buttons           | List    | O  | 캐러셀 리스트 버튼 목록 최소 1개, 최대 2개                                                                                                                                                                                                                                                    |
| --- name             | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자                                                                                                                                                                                                                    |
| --- type             | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- BC 타입은 상담톡을 이용하는 카카오톡 채널만 이용 가능<br>- BT 타입은 카카오 오픈 빌더의 챗봇을 사용하는 채널만 이용 가능<br>- BF 타입은 첫번째 버튼으로만 사용할 수 있으며, name에는 다음 3가지 문구만 사용 가능<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| --- linkMo           | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| --- linkPc           | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                       |
| --- schemeIos        | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                                                         |
| --- chatExtra        | String  | X  | BC / BT 타입 버튼일 경우 전달할 메타 정보                                                                                                                                                                                                                                                   |
| --- chatEvent        | String  | X  | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                                                                                                                                                                                                       |
| --- bizFormKey       | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                                            |
| -- coupon            | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                                                         |
| --- title            | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                                            |
| --- description      | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                                                      |
| --- linkMo           | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| --- linkPc           | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 1,000자 제한                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                 |
| --- schemeIos        | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                                                   |
| - tail               | Object  | X  | 더보기 버튼 정보                                                                                                                                                                                                                                                                     |
| -- linkMo            | String  | O  | 모바일 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| -- linkPc            | String  | X  | PC 웹 링크, 1,000자 제한                                                                                                                                                                                                                                                            |
| -- schemeAndroid     | String  | X  | 안드로이드 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | IOS 앱 링크, 1,000자 제한                                                                                                                                                                                                                                                           |
| recipientList        | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                                                             |
| - recipientNo        | String  | O  | 수신 번호                                                                                                                                                                                                                                                                         |
| - resendParameter    | Object  | X  | 대체 발송 정보                                                                                                                                                                                                                                                                      |
| -- isResend          | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                                                                                                                                                                       |
| -- resendType        | String  | X  | 대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                                                                                                                                                                       |
| -- resendTitle       | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                                                                                                                                                                               |
| -- resendContent     | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                                                                                                                                                                  |
| -- resendSendNo      | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                                                                                                                                                                 |
| - targeting         | String  | X  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                                |
| - unsubscribeNo       | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234        |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                                                                                                                                                                       |
| createUser           | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                                                                   |
| statsId              | String  | 	X | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                                                                                                                                                                            |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| 이름               | 타입      | Not Null | 설명                                                           |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | 응답 헤더 정보                                                     |
| - isSuccessful   | boolean | O        | API 호출 성공 여부                                                 |
| - resultCode     | Integer | O        | API 호출 결과 코드(성공: 0, 실패 시 오류 코드)                           |
| - resultMessage  | String  | O        | API 호출 결과 메시지(성공 시 "success" 또는 관련 성공 메시지, 실패 시 실패 원인 상세 메시지)  |
| message          | Object  | X        | 메시지 발송 결과 정보(발송 요청이 있는 경우에만 존재)                             |
| - requestId      | String  | X        | 발송 요청 ID(각 발송 요청을 고유하게 식별하는 ID)                             |
| - sendResults    | Array   | O        | 수신자별 발송 결과 목록                                                |
| -- recipientSeq  | Integer | O        | 수신자 목록의 순번                                                   |
| -- recipientNo   | String  | X        | 수신자 전화번호                                                     |
| -- resultCode    | Integer | O        | 수신자별 발송 결과 코드(성공 및 다양한 실패 코드가 존재할 수 있음)                     |
| -- resultMessage | String  | O        | 수신자별 발송 결과 메시지(성공 시 "success" 또는 관련 메시지, 실패 시 실패 원인 상세 메시지) |

## 메시지 기본형 발송 요청

* 템플릿을 이용한 발송입니다.
* 마수동 발송을 사용할 수 있습니다.
    * targeting 필드를 지정해 메시지 대상의 타입을 지정할 수 있습니다.
        * M: 고객사의 광고성 정보 수신동의 유저(카카오톡 수신 동의)
        * N: 고객사의 광고성 정보 수신동의 유저(카카오톡 수신 동의) - 채널 친구
        * I: 고객사의 발송 요청 대상 ∩ 채널 친구
* 기존 친구톡의 8가지 메시지 유형을 전부 사용할 수 있습니다.
* BC, BT 버튼 타입을 사용할 수 없습니다.
* AC(채널 추가)버튼을 사용할 수 있습니다.
* BF 버튼을 사용시 카카오에서 발급받은 비즈니스폼 ID를 업로드하여 비즈폼키를 발급받아 사용할 수 있습니다.
* 대체 발송은 수신자별 resendParameter를 통해 설정할 수 있습니다.
    * 대체 발송을 이용하실 경우 대체 발송 관리 API를 통해 SMS Appkey 등록 및 발송 설정이 필요합니다.
* <b>야간 발송 제한(20:50~다음 날 08:00)</b>

### 사용시 주의사항

- unsubscribeNo, unsubscribeAuthNo는 080 무료수신거부 전화번호와 인증번호로, 둘 중 하나라도 입력하지 않으면 발신 프로필에 등록된 무료수신거부 정보로 발송됩니다.
- 발송 간 unsubscribeNo, unsubscribeAuthNo를 입력할 경우 발신 프로필에 등록된 무료수신거부 정보가 아닌 입력한 값으로 발송됩니다.
- 발송 간 unsubscribeNo, unsubscribeAuthNo를 입력하지 않을 경우 발신 프로필에 등록된 무료수신거부 정보로 발송됩니다.
- unsubscribeNo, unsubscribeAuthNo는 수신자별로 입력할 수 있으며, 공통 필드와 수신자별 필드 둘 다 입력 시 공통 필드가 우선 적용됩니다.

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 발송 요청

```
{
  "senderKey": String,
  "templateCode": String,
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "recipientList": [
    {
      "recipientNo": String,
      "targeting": String,
      "templateParameter": Object,
      "imageParameters": List,
      "videoParameter": Object,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| 이름                     | 타입      | 필수 | 설명                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 발신 키(40자), 그룹 발신 키 사용 불가                                                                                                      |
| templateCode           | String  | O  | 사용하려는 템플릿 코드                                                                                                                  |
| pushAlarm              | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                    |
| requestDate            | String  | X  | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                            |
| unsubscribeNo          | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo      | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234 |
| recipientList          | List    | O  | 수신자 목록(최대 1,000명)                                                                                                             |
| - recipientNo          | String  | O  | 수신 번호                                                                                                                         |
| - targeting            | String  | O  | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)                                                             |
| - templateParameter    | Object  | X  | 템플릿 파라미터(템플릿에 치환할 변수 포함 시, 필수)                                                                                                |
| - imageParameters      | List    | X  | 템플릿 이미지 필드 값을 변경할 수 있는 동적 파라미터(템플릿에 존재하는 이미지 개수와 동일한 크기의 JSON 목록만 사용할 수 있음, 사용할 경우 변경하지 않을 이미지는 빈 JSON 객체를 입력해야 함)            |
| -- imageUrl            | String  | O  | 이미지 URL                                                                                                     |
| -- imageLink           | String  | X  | 이미지 링크                                                                                                                        |
| - videoParameter       | Object  | X  | 템플릿 비디오 필드 값을 변경할 수 있는 동적 파라미터                                                                                                |
| -- videoUrl            | String  | O  | 카카오TV 동영상 URL                                                                                                                 |
| -- thumbnailUrl        | String  | X  | 동영상 섬네일용 이미지 URL                                                                                                              |
| - resendParameter      | Object  | X  | 대체 발송 정보                                                                                                                      |
| -- isResend            | boolean | X  | 발송 실패 시, 문자 대체 발송 여부<br>콘솔에서 대체 발송 설정 시, 기본으로 대체 발송됩니다.                                                                       |
| -- resendType          | String  | X  | 대체 발송 타입(SMS,LMS)<br>값이 없을 경우, 템플릿 본문 길이에 따라 타입이 구분됩니다.                                                                       |
| -- resendTitle         | String  | X  | LMS 대체 발송 제목<br>(값이 없을 경우, 플러스친구 ID로 대체 발송됩니다.)                                                                               |
| -- resendContent       | String  | X  | 대체 발송 내용<br>(값이 없을 경우, [메시지 본문]으로 대체 발송됩니다.)                                                                                  |
| -- resendSendNo        | String  | X  | 대체 발송 발신 번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                                 |
| - unsubscribeNo        | String  | X  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| - unsubscribeAuthNo    | String  | X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234 |
| - recipientGroupingKey | String  | X  | 수신자 그룹핑 키(수신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                   |
| senderGroupingKey      | String  | X  | 발신자 그룹핑 키(발신자별로 그룹핑 키를 지정할 수 있습니다. 최대 100자)                                                                                   |
| resellerCode           | String  | X  | 리셀러 코드(리셀러가 발송 시 사용)                                                                                                          |
| createUser             | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                   |
| statsId                | String  | X  | 통계 ID(발신 검색 조건에는 포함되지 않습니다. 최대 8자)                                                                                            |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| 이름               | 타입      | Not Null | 설명                                                           |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | 응답 헤더 정보                                                     |
| - isSuccessful   | boolean | O        | API 호출 성공 여부                                                 |
| - resultCode     | Integer | O        | API 호출 결과 코드(성공: 0, 실패 시 오류 코드)                           |
| - resultMessage  | String  | O        | API 호출 결과 메시지(성공 시 "success" 또는 관련 성공 메시지, 실패 시 실패 원인 상세 메시지)  |
| message          | Object  | X        | 메시지 발송 결과 정보(발송 요청이 있는 경우에만 존재)                             |
| - requestId      | String  | X        | 발송 요청 ID(각 발송 요청을 고유하게 식별하는 ID)                             |
| - sendResults    | Array   | O        | 수신자별 발송 결과 목록                                                |
| -- recipientSeq  | Integer | O        | 수신자 목록의 순번                                                   |
| -- recipientNo   | String  | X        | 수신자 전화번호                                                     |
| -- resultCode    | Integer | O        | 수신자별 발송 결과 코드(성공 및 다양한 실패 코드가 존재할 수 있음)                     |
| -- resultMessage | String  | O        | 수신자별 발송 결과 메시지(성공 시 "success" 또는 관련 메시지, 실패 시 실패 원인 상세 메시지) |

## 발송 목록 조회

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Query parameter] 1번 or 2번 조건 필수

| 이름               | 타입     | 필수        | 설명                               |
|------------------|--------|-----------|----------------------------------|
| requestId        | String | 조건 필수(1번) | 요청 ID                            |
| startRequestDate | String | 조건 필수(2번) | 발송 요청 날짜 시작 값(yyyy-MM-dd HH:mm)  |
| endRequestDate   | String | 조건 필수(2번) | 발송 요청 날짜 끝 값(yyyy-MM-dd HH:mm)   |
| senderKey        | String | X         | 발신 키                             |
| templateCode     | String | X         | 템플릿 코드                           |
| recipientNo      | String | X         | 수신 번호                            |
| messageStatus    | String | X         | 요청 상태(COMPLETED: 성공, FAILED: 실패) |
| resultCode       | String | X         | 발송 결과(MRC01: 성공 MRC02: 실패 )      |
| senderGroupingKey    | String   | X          | 발신 그룹핑 키                          |
| recipientGroupingKey | 	String  | 	X         | 수신자 그룹핑 키                        |
| pageNum          | String | X         | 페이지 번호(Default: 1)               |
| pageSize         | String | X         | 조회 건수(Default: 15, Max: 1000)    |

#### 응답

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
        "templateCode": String,
        "recipientNo": String,
        "targeting": String,
        "requestDate": String,
        "createDate": String,
        "receiveDate": String,
        "chatBubbleType": String,
        "pushAlarm": boolean,
        "messageStatus": String,
        "resendStatusCode": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "isAddedChannel": boolean,
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

| 이름                          | 타입      | Not Null | 설명                                                                    |
|:----------------------------|:--------|:---------|:----------------------------------------------------------------------|
| header                      | Object  | O        | 헤더 영역                                                                 |
| - resultCode                | Integer | O        | 결과 코드                                                                 |
| - resultMessage             | String  | O        | 결과 메시지                                                                |
| - isSuccessful              | boolean | O        | 성공 여부                                                                 |
| messageSearchResultResponse | Object  | X        | 본문 영역                                                                 |
| - messages                  | Array   | O        | 메시지 목록                                                               |
| -- requestId                | String  | O        | 요청 ID                                                                 |
| -- recipientSeq             | Integer | O        | 수신자 시퀀스 번호                                                            |
| -- plusFriendId             | String  | O        | 발신 프로필 ID                                                             |
| -- senderKey                | String  | O        | 발신 키                                                                  |
| -- templateCode             | String  | X        | 템플릿 코드                                                                |
| -- recipientNo              | String  | O        | 수신 번호                                                                 |
| -- targeting                | String  | O        | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저) |
| -- requestDate              | String  | O        | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능                                                                 |
| -- createDate               | String  | O        | 등록 일시                                                                 |
| -- receiveDate              | String  | X        | 수신 일시                                                                 |
| -- chatBubbleType           | String  | O        | 메시지 타입                                                                |
| -- pushAlarm                | boolean | O        | 푸시 알람 여부                                                              |
| -- messageStatus            | String  | O        | 요청 상태(COMPLETED: 성공, FAILED: 실패)                                      |
| -- resendStatusCode         | String  | X        | 대체 발송 상태 코드                                                           |
| -- resendStatusName         | String  | X        | 대체 발송 상태 이름                                                           |
| -- resendResultCode         | String  | X        | 대체 발송 결과 코드                                                           |
| -- resendRequestId          | String  | X        | 대체 발송 요청 ID                                                           |
| -- isAddedChannel           | boolean | O        | 채널 친구 여부                                                              |
| -- resultCode               | String  | X        | 수신 결과 코드                                                              |
| -- resultCodeName           | String  | X        | 수신 결과 코드명                                                             |
| -- createUser               | String  | X        | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                           |
| -- senderGroupingKey        | String  | X        | 발신 그룹핑 키                                                 |
| -- recipientGroupingKey     | String  | X        | 수신자 그룹핑 키                                           |
| - totalCount                | Integer | O        | 총 개수                                                                  |

## 발송 단건 조회

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 타입      | 설명         |
|--------------|---------|------------|
| appkey       | String  | 고유의 앱키     |
| requestId    | String  | 요청 ID      |
| recipientSeq | Integer | 수신자 시퀀스 번호 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

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
    "templateCode": String,
    "recipientNo": String,
    "targeting": String,
    "requestDate": String,
    "createDate": String,
    "receiveDate": String,
    "chatBubbleType": String,
    "content": String,
    "adult": boolean,
    "header": String,
    "additionalContent": String,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "chatExtra": String,
        "chatEvent": String,
        "bizFormKey": String
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String
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
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
    "carousel": {
      "head": {
        "header": String,
        "content": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String
      },
      "list": [
        {
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String,
              "chatExtra": String,
              "chatEvent": String,
              "bizFormKey": String
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
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
    "templateParameter": String,
    "pushAlarm": boolean,
    "messageStatus": String,
    "isAddedChannel": boolean,
    "resultCode": String,
    "resultCodeName": String,
    "resendStatusCode": String,
    "resendStatusName": String,
    "resendResultCode": String,
    "resendRequestId": String,
    "createUser": String,
    "senderGroupingKey": String,
    "recipientGroupingKey": String
  }
}
```

| 이름                    | 타입      | Not Null | 설명                                                                                               | 
 |:----------------------|:--------|:---------|:-------------------------------------------------------------------------------------------------| 
| header                | Object  | O        | 헤더 영역                                                                                            | 
| - resultCode          | Integer | O        | 결과 코드                                                                                            | 
| - resultMessage       | String  | O        | 결과 메시지                                                                                           | 
| - isSuccessful        | boolean | O        | 성공 여부                                                                                            | 
| message               | Object  | X        | 메시지 본문 영역(메시지 실패 시 없을 수 있음)                                                                     | 
| - requestId           | String  | O        | 요청 ID(message 객체 존재 시 Not Null)                                                                 | 
| - recipientSeq        | Integer | O        | 수신자 시퀀스 번호(message 객체 존재 시 Not Null)                                                            | 
| - plusFriendId        | String  | O        | 발신 프로필 ID(message 객체 존재 시 Not Null)                                                             | 
| - senderKey           | String  | O        | 발신 키(message 객체 존재 시 Not Null)                                                                  | 
| - templateCode        | String  | X        | 템플릿 코드                                                                                           | 
| - recipientNo         | String  | O        | 수신 번호(message 객체 존재 시 Not Null)                                                                 | 
| - targeting           | String  | O        | 메시지 대상의 타입(M: 마케팅 수신 동의 유저, N: 친구가 아닌 마케팅 수신 동의 유저에게만, I: 친구인 유저)(message 객체 존재 시 Not Null) | 
| - requestDate         | String  | O        | 요청 일시(yyyy-MM-dd HH:mm)<br>(입력하지 않을 경우 즉시 발송)<br>최대 60일 이후까지 예약 가능(message 객체 존재 시 Not Null)                                                                 | 
| - createDate          | String  | O        | 등록 일시(message 객체 존재 시 Not Null)                                                                 | 
| - receiveDate         | String  | X        | 수신 일시                                                                                            | 
| - chatBubbleType      | String  | O        | 메시지 타입(message 객체 존재 시 Not Null)                                                                | 
| - content             | String  | X        | 메시지 내용                                                                                           | 
| - adult               | boolean | O        | 성인용 메시지 여부(message 객체 존재 시 Not Null)                                                            | 
| - header              | String  | X        | 헤더(메시지 내)                                                                                       | 
| - additionalContent   | String  | X        | 부가 정보(메시지 내)                                                                                    |
| - image               | Object  | X        | 이미지 요소                                                                                           | 
| -- imageUrl           | String  | O        | 이미지 URL(image 객체 존재 시 Not Null)                                                                 | 
| -- imageLink          | String  | X        | 이미지 링크                                                                                           | 
| - buttons             | Array   | X        | 버튼 목록                                                                                            | 
| -- name               | String  | O        | 버튼 제목(buttons 배열 항목 존재 시 Not Null)                                                              | 
| -- type               | String  | O        | 버튼 타입(buttons 배열 항목 존재 시 Not Null)                                                              | 
| -- linkMo             | String  | X        | 모바일 웹 링크                                                                                         | 
| -- linkPc             | String  | X        | PC 웹 링크                                                                                          | 
| -- schemeIos          | String  | X        | IOS 앱 링크                                                                                         | 
| -- schemeAndroid      | String  | X        | 안드로이드 앱 링크                                                                                       | 
| -- chatExtra          | String  | X        | BC / BT 타입 버튼일 경우 전달할 메타 정보                                                                      | 
| -- chatEvent          | String  | X        | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                          | 
| -- bizFormKey         | String  | X        | BF 타입 버튼일 경우 비즈폼 키                                                                               | 
| - item                | Object  | X        | 와이드 리스트 요소                                                                                       | 
| -- list               | Array   | X        | 와이드 리스트(item 객체 존재 시 Nullable)                                                                  | 
| --- title             | String  | X        | 아이템 제목                                                                                           | 
| --- imageUrl          | String  | O        | 아이템 이미지 URL(item.list 항목 존재 시 Not Null)                                                         | 
| --- linkMo            | String  | O        | 모바일 웹 링크(item.list 항목 존재 시 Not Null)                                                            | 
| --- linkPc            | String  | X        | PC 웹 링크                                                                                          | 
| --- schemeIos         | String  | X        | IOS 앱 링크                                                                                         | 
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크                                                                                       | 
| - coupon              | Object  | X        | 쿠폰 요소                                                                                            | 
| -- title              | String  | O        | 쿠폰 제목(coupon 객체 존재 시 Not Null)                                                                  | 
| -- description        | String  | O        | 쿠폰 상세 설명(coupon 객체 존재 시 Not Null)                                                               | 
| -- linkMo             | String  | X        | 모바일 웹 링크                                                                                         | 
| -- linkPc             | String  | X        | PC 웹 링크                                                                                          | 
| -- schemeAndroid      | String  | X        | 안드로이드 앱 링크                                                                                       | 
| -- schemeIos          | String  | X        | IOS 앱 링크                                                                                         | 
| - commerce            | Object  | X        | 커머스 요소                                                                                           | 
| -- title              | String  | O        | 상품 제목(commerce 객체 존재 시 Not Null)                                                                | 
| -- regularPrice       | Integer | X        | 정상 가격                                                                                            | 
| -- discountPrice      | Integer | X        | 할인가격                                                                                             | 
| -- discountRate       | Integer | X        | 할인율                                                                                              | 
| -- discountFixed      | Integer | X        | 정액할인가격                                                                                           | 
| - video               | Object  | X        | 동영상 요소                                                                                           | 
| -- videoUrl           | String  | O        | 카카오TV 동영상 URL(video 객체 존재 시 Not Null)                                                           | 
| -- thumbnailUrl       | String  | X        | 동영상 섬네일용 이미지 URL                                                                                 | 
| - carousel            | Object  | X        | 캐러셀                                                                                              | 
| -- head               | Object  | X        | 캐러셀 인트로(carousel 객체 존재 시 Nullable)                                                              | 
| --- header            | String  | O        | 캐러셀 인트로 헤더(head 객체 존재 시 Not Null)                                                               | 
| --- content           | String  | O        | 캐러셀 인트로 내용(head 객체 존재 시 Not Null)                                                               | 
| --- imageUrl          | String  | O        | 캐러셀 인트로 이미지 주소(head 객체 존재 시 Not Null)                                                           | 
| --- linkMo            | String  | X        | 모바일 웹 링크                                                                                         | 
| --- linkPc            | String  | X        | PC 웹 링크                                                                                          | 
| --- schemeIos         | String  | X        | IOS 앱 링크                                                                                         | 
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크                                                                                       | 
| -- list               | Array   | O        | 캐러셀 리스트(carousel 객체 존재 시 Not Null)                                                              | 
| --- header            | String  | X        | 캐러셀 아이템 헤더                                                                                       | 
| --- message           | String  | O        | 캐러셀 아이템 메시지(list 항목 존재 시 Not Null)                                                              | 
| --- additionalContent | String  | X        | 부가 정보                                                                                            | 
| --- imageUrl          | String  | X        | 이미지 URL                                                                                          | 
| --- imageLink         | String  | X        | 이미지 링크                                                                                           | 
| --- commerce          | Object  | X        | 커머스(캐러셀 내)                                                                                      | 
| ---- title            | String  | O        | 상품 제목(carousel.list.commerce 존재 시 Not Null)                                                     | 
| ---- regularPrice     | Integer | X        | 정상 가격                                                                                            | 
| ---- discountPrice    | Integer | X        | 할인가격                                                                                             | 
| ---- discountRate     | Integer | X        | 할인율                                                                                              | 
| ---- discountFixed    | Integer | X        | 정액할인가격                                                                                           | 
| --- buttons           | Array   | X        | 버튼 목록(캐러셀 내)                                                                                    | 
| ---- name             | String  | O        | 버튼 제목(carousel.list.buttons 항목 존재 시 Not Null)                                                   | 
| ---- type             | String  | O        | 버튼 타입(carousel.list.buttons 항목 존재 시 Not Null)                                                   | 
| ---- linkMo           | String  | X        | 모바일 웹 링크                                                                                         | 
| ---- linkPc           | String  | X        | PC 웹 링크                                                                                          | 
| ---- schemeAndroid    | String  | X        | 안드로이드 앱 링크                                                                                       | 
| ---- schemeIos        | String  | X        | IOS 앱 링크                                                                                         | 
| ---- chatExtra        | String  | X        | BC / BT 타입 버튼일 경우 전달할 메타 정보                                                                      | 
| ---- chatEvent        | String  | X        | BT 타입 버튼일 경우 연결할 봇 이벤트명                                                                          | 
| ---- bizFormKey       | String  | X        | BF 타입 버튼일 경우 비즈폼 키                                                                               | 
| --- coupon            | Object  | X        | 쿠폰(캐러셀 내)                                                                                       | 
| ---- title            | String  | O        | 쿠폰 제목(carousel.list.coupon 존재 시 Not Null)                                                       | 
| ---- description      | String  | O        | 쿠폰 상세 설명(carousel.list.coupon 존재 시 Not Null)                                                    | 
| ---- linkMo           | String  | X        | 모바일 웹 링크                                                                                         | 
| ---- linkPc           | String  | X        | PC 웹 링크                                                                                          | 
| ---- schemeAndroid    | String  | X        | 안드로이드 앱 링크                                                                                       | 
| ---- schemeIos        | String  | X        | IOS 앱 링크                                                                                         | 
| -- tail               | Object  | X        | 더보기 버튼 정보(carousel 객체 존재 시 Nullable)                                                            | 
| --- linkMo            | String  | O        | 모바일 웹 링크(tail 객체 존재 시 Not Null)                                                                 | 
| --- linkPc            | String  | X        | PC 웹 링크                                                                                          | 
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크                                                                                       | 
| --- schemeIos         | String  | X        | IOS 앱 링크                                                                                         | 
| - templateParameter   | String  | X        | 템플릿 파라미터                                                                                         | 
| - pushAlarm           | boolean | O        | 푸시 알림 여부(message 객체 존재 시 Not Null)                                                              | 
| - messageStatus       | String  | O        | 요청 상태(COMPLETED: 성공, FAILED: 실패)(message 객체 존재 시 Not Null)                                     | 
| - isAddedChannel      | boolean | O        | 채널 친구 여부(message 객체 존재 시 Not Null)                                                              | 
| - resultCode          | String  | X        | 수신 결과 코드(메시지 내)                                                                                 | 
| - resultCodeName      | String  | X        | 수신 결과 코드명(메시지 내)                                                                                | 
| - resendStatusCode    | String  | X        | 대체 발송 상태 코드                                                                                      | 
| - resendStatusName    | String  | X        | 대체 발송 상태 코드명                                                                                     | 
| - resendResultCode    | String  | X        | 대체 발송 결과 코드                                                                                      | 
| - resendRequestId     | String  | X        | 대체 발송 요청 ID                                                                                      | 
| - createUser          | String  | X        | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                      |
| - senderGroupingKey   | String  | X        | 발신 그룹핑 키                                                 |
| - recipientGroupingKey | String  | X        | 수신자 그룹핑 키                                           |

## 메시지 발송 취소

#### 요청

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	고유의 앱키 |
| requestId | String  | 요청 ID   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름           | 	타입     | 	필수 | 	설명                                         |
|--------------|---------|-----|---------------------------------------------|
| recipientSeq | 	String | 	X  | 수신자 시퀀스 번호<br>(입력하지 않으면 요청 ID의 모든 발송 건을 취소) |

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    X     | 헤더 영역  |
| - resultCode    | Integer |    X     | 결과 코드  |
| - resultMessage | String  |    X     | 결과 메시지 |
| - isSuccessful  | Boolean |    X     | 성공 여부  |

[예시]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://api-alimtalk.cloud.toast.com/brand-message/v1.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

## 템플릿 관리

### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름           | 타입      | 필수 | 설명                            |
|--------------|---------|----|-------------------------------|
| templateCode | String  | X  | 템플릿 코드                        |
| templateName | String  | X  | 템플릿 이름                        |
| status       | String  | X  | 템플릿 상태 코드                     |
| pageNum      | Integer | X  | 페이지 번호(Default: 1)            |
| pageSize     | Integer | X  | 조회 건수(Default: 15, Max: 1000) |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateListResponse": {
    "templates": [
      {
        "plusFriendId": String,
        "plusFriendType": String,
        "templateCode": String,
        "templateName": String,
        "chatBubbleType": String,
        "content": String,
        "header": String,
        "additionalContent": String,
        "adult": boolean,
        "image": {
          "imageUrl": String,
          "imageLink": String
        },
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer,
          }
        ],
        "item": {
          "list": [
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String
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
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
        "video": {
          "videoUrl": String,
          "thumbnailUrl": String
        },
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
              "header": String,
              "message": String,
              "additionalContent": String,
              "imageUrl": String,
              "imageLink": String,
              "commerce": {
                "title": String,
                "regularPrice": Integer,
                "discountPrice": Integer,
                "discountRate": Integer,
                "discountFixed": Integer
              },
              "buttons": [
                {
                  "name": String,
                  "type": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeIos": String,
                  "schemeAndroid": String,
                  "bizFormId": Integer,
                }
              ],
              "coupon": {
                "title": String,
                "description": String,
                "linkMo": String,
                "linkPc": String,
                "schemeAndroid": String,
                "schemeIos": String
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
        "status": String,
        "createDate": String,
        "updateDate": String
      }
    ],
    "totalCount": Integer
  }
}
```

| 이름                      | 타입      | Not Null | 설명                                     |
|:------------------------|:--------|:---------|:---------------------------------------|
| header                  | Object  | O        | 헤더 영역                                  |
| - resultCode            | Integer | O        | 결과 코드                                  |
| - resultMessage         | String  | O        | 결과 메시지                                 |
| - isSuccessful          | boolean | O        | 성공 여부                                  |
| templateListResponse    | Object  | O        | 본문 영역                                  |
| - templates             | Array   | O        | 템플릿 리스트                                |
| -- plusFriendId         | String  | O        | 발신 프로필 ID                              |
| -- plusFriendType       | String  | O        | 발신 프로필 타입                              |
| -- templateCode         | String  | O        | 템플릿 코드                                 |
| -- templateName         | String  | O        | 템플릿 명                                  |
| -- chatBubbleType       | String  | O        | 메시지 타입                                 |
| -- content              | String  | X        | 메시지 내용                                 |
| -- header               | String  | X        | 헤더                                     |
| -- additionalContent    | String  | X        |(설명 테이블 참고)                            |
| -- adult                | boolean | O        | 성인용 메시지 여부                             |
| -- image                | Object  | X        | 이미지 정보                                 |
| --- imageUrl            | String  | O        | 이미지 URL                                |
| --- imageLink           | String  | X        | 이미지 링크                                 |
| -- buttons              | Array   | X        | 버튼 목록                                  |
| --- name                | String  | O        | 버튼 제목                                  |
| --- type                | String  | O        | 버튼 타입                                  |
| --- linkMo              | String  | X        | 모바일 웹 링크                               |
| --- linkPc              | String  | X        | PC 웹 링크                                |
| --- schemeIos           | String  | X        | IOS 앱 링크                               |
| --- schemeAndroid       | String  | X        | 안드로이드 앱 링크                             |
| --- bizFormId           | Integer | X        | 비즈폼 ID(JSON 기준)                       |
| -- item                 | Object  | X        | 와이드 리스트 요소                             |
| --- list                | Array   | X        | 와이드 리스트                                |
| ---- title              | String  | X        | 아이템 제목                                 |
| ---- imageUrl           | String  | O        | 아이템 이미지 URL                            |
| ---- linkMo             | String  | O        | 모바일 웹 링크                               |
| ---- linkPc             | String  | X        | PC 웹 링크                                |
| ---- schemeAndroid      | String  | X        | 안드로이드 앱 링크                             |
| ---- schemeIos          | String  | X        | IOS 앱 링크                               |
| -- coupon               | Object  | X        | 쿠폰 요소                                  |
| --- title               | String  | O        | 쿠폰 제목                                  |
| --- description         | String  | O        | 쿠폰 상세 설명                               |
| --- linkMo              | String  | X        | 모바일 웹 링크                               |
| --- linkPc              | String  | X        | PC 웹 링크                                |
| --- schemeAndroid       | String  | X        | 안드로이드 앱 링크                             |
| --- schemeIos           | String  | X        | IOS 앱 링크                               |
| -- commerce             | Object  | X        | 커머스 요소                                 |
| --- title               | String  | O        | 상품 제목                                  |
| --- regularPrice        | Integer | X        | 정상 가격                                  |
| --- discountPrice       | Integer | X        | 할인가격                                   |
| --- discountRate        | Integer | X        | 할인율                                    |
| --- discountFixed       | Integer | X        | 정액할인가격                                 |
| -- video                | Object  | X        | 동영상 요소                                 |
| --- videoUrl            | String  | O        | 카카오TV 동영상 URL                          |
| --- thumbnailUrl        | String  | X        | 동영상 섬네일용 이미지 URL                       |
| -- carousel             | Object  | X        | 캐러셀                                    |
| --- head                | Object  | X        | 캐러셀 인트로                                |
| ---- header             | String  | O        | 캐러셀 인트로 헤더                             |
| ---- content            | String  | O        | 캐러셀 인트로 내용                             |
| ---- imageUrl           | String  | O        | 캐러셀 인트로 이미지 주소                         |
| ---- linkMo             | String  | X        | 모바일 웹 링크                               |
| ---- linkPc             | String  | X        | PC 웹 링크                                |
| ----- schemeAndroid     | String  | X        | 안드로이드 앱 링크                             |
| ----- schemeIos         | String  | X        | IOS 앱 링크                               |
| ---- list               | Array   | O        | 캐러셀 리스트                                |
| ----- header            | String  | O        | 캐러셀 아이템 헤더                             |
| ----- message           | String  | O        | 캐러셀 아이템 메시지(Not Null 리스트의 content 매핑) |
| ----- additionalContent | String  | X        | 부가 정보                                  |
| ----- imageUrl          | String  | O        | 이미지 URL(캐러셀 아이템 내)                    |
| ----- imageLink         | String  | X        | 이미지 링크(캐러셀 아이템 내)                     |
| ----- commerce          | Object  | O        | 커머스(캐러셀 아이템 내)                        |
| ------ title            | String  | O        | 상품 제목                                  |
| ------ regularPrice     | Integer | X        | 정상 가격                                  |
| ------ discountPrice    | Integer | X        | 할인가격                                   |
| ------ discountRate     | Integer | X        | 할인율                                    |
| ------ discountFixed    | Integer | X        | 정액할인가격                                 |
| ----- buttons           | Array   | O        | 캐러셀 리스트 버튼 목록                          |
| ------ name             | String  | O        | 버튼 제목                                  |
| ------ type             | String  | O        | 버튼 타입                                  |
| ------ linkMo           | String  | X        | 모바일 웹 링크                               |
| ------ linkPc           | String  | X        | PC 웹 링크                                |
| ------ schemeIos        | String  | X        | IOS 앱 링크                               |
| ------ schemeAndroid    | String  | X        | 안드로이드 앱 링크                             |
| ------ bizFormId        | Integer | X        | 비즈폼 ID(JSON 기준)                       |
| ----- coupon            | Object  | X        | 쿠폰 요소(캐러셀 아이템 내)                      |
| ------ title            | String  | X        | 쿠폰 제목                                  |
| ------ description      | String  | X        | 쿠폰 상세 설명                               |
| ------ linkMo           | String  | X        | 모바일 웹 링크                               |
| ------ linkPc           | String  | X        | PC 웹 링크                                |
| ------ schemeAndroid    | String  | X        | 안드로이드 앱 링크                             |
| ------ schemeIos        | String  | X        | IOS 앱 링크                               |
| ---- tail               | Object  | X        | 더보기 버튼 정보                              |
| ----- linkMo            | String  | O        | 모바일 웹 링크                               |
| ----- linkPc            | String  | X        | PC 웹 링크                                |
| ----- schemeAndroid     | String  | X        | 안드로이드 앱 링크                             |
| ----- schemeIos         | String  | X        | IOS 앱 링크                               |
| --- status              | String  | O        | 템플릿 상태(A: 등록, S: 차단)                   |
| --- createDate          | String  | O        | 등록 일시                                  |
| --- updateDate          | String  | X        | 수정 일시                                  |
| - totalCount            | Integer | O        | 총 개수                                   |

### 템플릿 단건 조회

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 타입     | 설명     |
|--------------|--------|--------|
| appkey       | String | 고유의 앱키 |
| senderKey    | String | 발신 키   |
| templateCode | String | 템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "plusFriendId": String,
    "plusFriendType": String,
    "templateCode": String,
    "templateName": String,
    "chatBubbleType": String,
    "content": String,
    "header": String,
    "additionalContent": String,
    "adult": boolean,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "bizFormId": Integer
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
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
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
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
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
              "bizFormId": Integer
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
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
    "status": String,
    "createDate": String,
    "updateDate": String
  }
}
```

| 이름                    | 타입      | Not Null | 설명                   |
|:----------------------|:--------|:---------|:---------------------|
| header                | Object  | O        | 헤더 영역                |
| - resultCode          | Integer | O        | 결과 코드                |
| - resultMessage       | String  | O        | 결과 메시지               |
| - isSuccessful        | boolean | O        | 성공 여부                |
| template              | Object  | O        | 템플릿 본문 영역            |
| - plusFriendId        | String  | O        | 발신 프로필 ID            |
| - plusFriendType      | String  | O        | 발신 프로필 타입            |
| - templateCode        | String  | O        | 템플릿 코드               |
| - templateName        | String  | O        | 템플릿 명                |
| - chatBubbleType      | String  | O        | 메시지 타입               |
| - pushAlarm           | boolean | X        | 푸시 알림 여부             |
| - content             | String  | X        | 메시지 내용               |
| - adult               | boolean | O        | 성인용 메시지 여부           |
| - header              | String  | X        | 헤더(템플릿 내)           |
| - additionalContent   | String  | X        | 부가 정보(템플릿 내)        |
| - image               | Object  | X        | 이미지 정보               |
| -- imageUrl           | String  | O        | 이미지 URL              |
| -- imageLink          | String  | X        | 이미지 링크               |
| - buttons             | Array   | X        | 버튼 목록                |
| -- name               | String  | O        | 버튼 제목                |
| -- type               | String  | O        | 버튼 타입                |
| -- linkMo             | String  | X        | 모바일 웹 링크             |
| -- linkPc             | String  | X        | PC 웹 링크              |
| -- schemeIos          | String  | X        | IOS 앱 링크             |
| -- schemeAndroid      | String  | X        | 안드로이드 앱 링크           |
| -- bizFormId          | Integer | X        | 비즈폼 ID(JSON 기준)     |
| - item                | Object  | X        | 와이드 리스트 요소           |
| -- list               | Array   | X        | 와이드 리스트              |
| --- title             | String  | X        | 아이템 제목               |
| --- imageUrl          | String  | O        | 아이템 이미지 URL          |
| --- linkMo            | String  | O        | 모바일 웹 링크             |
| --- linkPc            | String  | X        | PC 웹 링크              |
| --- schemeIos         | String  | X        | IOS 앱 링크             |
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크           |
| - coupon              | Object  | X        | 쿠폰 요소                |
| -- title              | String  | O        | 쿠폰 제목                |
| -- description        | String  | O        | 쿠폰 상세 설명             |
| -- linkMo             | String  | X        | 모바일 웹 링크             |
| -- linkPc             | String  | X        | PC 웹 링크              |
| -- schemeIos          | String  | X        | IOS 앱 링크             |
| -- schemeAndroid      | String  | X        | 안드로이드 앱 링크           |
| - commerce            | Object  | X        | 커머스 요소               |
| -- title              | String  | O        | 상품 제목                |
| -- regularPrice       | Integer | X        | 정상 가격                |
| -- discountPrice      | Integer | X        | 할인가격                 |
| -- discountRate       | Integer | X        | 할인율                  |
| -- discountFixed      | Integer | X        | 정액할인가격               |
| - video               | Object  | X        | 동영상 요소               |
| -- videoUrl           | String  | O        | 카카오TV 동영상 URL        |
| -- thumbnailUrl       | String  | X        | 동영상 섬네일용 이미지 URL     |
| - carousel            | Object  | X        | 캐러셀                  |
| -- head               | Object  | X        | 캐러셀 인트로              |
| --- header            | String  | O        | 캐러셀 인트로 헤더           |
| --- content           | String  | O        | 캐러셀 인트로 내용           |
| --- imageUrl          | String  | O        | 캐러셀 인트로 이미지 주소       |
| --- linkMo            | String  | X        | 모바일 웹 링크             |
| --- linkPc            | String  | X        | PC 웹 링크              |
| --- schemeIos         | String  | X        | IOS 앱 링크             |
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크           |
| -- list               | Array   | O        | 캐러셀 리스트              |
| --- header            | String  | O        | 캐러셀 아이템 헤더           |
| --- message           | String  | O        | 캐러셀 아이템 메시지          |
| --- additionalContent | String  | X        | 부가 정보(캐러셀 아이템 내)    |
| --- imageUrl          | String  | O        | 이미지 URL(캐러셀 아이템 내)  |
| --- imageLink         | String  | X        | 이미지 링크(캐러셀 아이템 내)   |
| --- commerce          | Object  | O        | 커머스(캐러셀 아이템 내)      |
| ---- title            | String  | O        | 상품 제목                |
| ---- regularPrice     | Integer | X        | 정상 가격                |
| ---- discountPrice    | Integer | X        | 할인가격                 |
| ---- discountRate     | Integer | X        | 할인율                  |
| ---- discountFixed    | Integer | X        | 정액할인가격               |
| --- buttons           | Array   | O        | 캐러셀 리스트 버튼 목록        |
| ---- name             | String  | O        | 버튼 제목                |
| ---- type             | String  | O        | 버튼 타입                |
| ---- linkMo           | String  | X        | 모바일 웹 링크             |
| ---- linkPc           | String  | X        | PC 웹 링크              |
| ---- schemeIos        | String  | X        | IOS 앱 링크             |
| ---- schemeAndroid    | String  | X        | 안드로이드 앱 링크           |
| ---- bizFormId        | Integer | X        | 비즈폼 ID(JSON 기준)     |
| --- coupon            | Object  | X        | 쿠폰 요소(캐러셀 아이템 내)    |
| ---- title            | String  | X        | 쿠폰 제목                |
| ---- description      | String  | X        | 쿠폰 상세 설명             |
| ---- linkMo           | String  | X        | 모바일 웹 링크             |
| ---- linkPc           | String  | X        | PC 웹 링크              |
| ---- schemeIos        | String  | X        | IOS 앱 링크             |
| ---- schemeAndroid    | String  | X        | 안드로이드 앱 링크           |
| -- tail               | Object  | X        | 더보기 버튼 정보            |
| --- linkMo            | String  | O        | 모바일 웹 링크             |
| --- linkPc            | String  | X        | PC 웹 링크              |
| --- schemeIos         | String  | X        | IOS 앱 링크             |
| --- schemeAndroid     | String  | X        | 안드로이드 앱 링크           |
| - status              | String  | O        | 템플릿 상태(A: 등록, S: 차단) |
| - createDate          | String  | O        | 등록 일시                |
| - updateDate          | String  | X        | 수정 일시                |

### 템플릿 등록

#### 요청

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 유의 사항

* 쿠폰 제목에 치환자를 적용할 경우 다음과 같은 고정 치환자를 사용해야 합니다.

```
- #{할인금액}원 할인 쿠폰(#{할인금액} 범위는 1 ~ 99,999,999)
- #{할인율}% 할인 쿠폰(#{할인율} 범위는 1 ~ 100)
- 배송비 할인 쿠폰
- #{상품명} 무료 쿠폰(#{상품명}은 최대 7자)
- #{상품명} UP 쿠폰(#{상품명}은 최대 7자)
```

* 커머스에서 상품 제목을 제외한 regularPrice, discountPrice, discountRate, discountFixed 필드에는 사용자가 치환자를 지정할 수 없습니다.
    * 상품 제목을 제외한 모든 필드들의 값을 비울 경우 자동으로 고정 치환자가 채워져 저장됩니다.
    * regularPrice -> #{정상가격}
    * discountPrice -> #{할인가격}
    * discountRate -> #{할인율}
    * discountFixed -> #{정액할인가격}

#### 텍스트형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "TEXT",
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름              | 타입      | 필수 | 설명                                                                                                                                                                                                                                             |
|-----------------|---------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | 템플릿명(최대 200자)                                                                                                                                                                                                                                  |
| chatBubbleType  | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                           |
| adult           | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                         |
| content         | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음, 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음 |
| buttons         | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                                  |
| - name          | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                                       |
| - type          | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫 번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야 함             |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                             |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                            |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                         |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                           |
| - bizFormKey    | String  | X  | BF 타입 버튼일 경우 비즈폼 키<br>치환자 사용 불가능                                                                                                                                                                                                               |
| coupon          | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                          |
| - title         | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                             |
| - description   | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                       |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                     |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                            |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                   |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                     |

#### 이미지형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "IMAGE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름              | 타입      | 필수 | 설명                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | 템플릿명(최대 200자)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음, 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음 |
| image           | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                                      |
| - imageUrl      | String  | O  | 이미지 URL, 일반 이미지로 업로드된 이미지 URL 사용<br>치환자 사용 불가능                                                                                                                                                                                                      |
| - imageLink     | String  | X  | 이미지 클릭 시 이동할 URL, 500자 제한<br>미설정 시 카카오톡 내 이미지 뷰어 사용<br>치환자 사용 불가능                                                                                                                                                                                    |
| buttons         | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                                       |
| - name          | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                                            |
| - type          | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫 번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야 함                   |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                               |
| - title         | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                  |
| - description   | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                            |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                       |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |

#### 와이드 이미지형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름              | 타입      | 필수 | 설명                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | 템플릿명(최대 200자)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈: 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음, 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음 |
| image           | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                                      |
| - imageUrl      | String  | O  | 이미지 URL, 와이드 이미지로 업로드된 이미지 URL 사용<br>치환자 사용 불가능                                                                                                                                                                                                     |
| - imageLink     | String  | X  | 이미지 클릭 시 이동할 URL, 500자 제한<br>미설정 시 카카오톡 내 이미지 뷰어 사용<br>치환자 사용 불가능                                                                                                                                                                                    |
| buttons         | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                                       |
| - name          | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                                            |
| - type          | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫 번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야 함                   |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                               |
| - title         | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                  |
| - description   | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                            |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                       |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |

#### 와이드 아이템 리스트형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름               | 타입      | 필수 | 설명                                                                                                                                                                                                                                |
|------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName     | String  | O  | 템플릿 명(최대 200자)                                                                                                                                                                                                                   |
| chatBubbleType   | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                             |
| adult            | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                           |
| header           | String  | O  | 헤더<br>- WIDE_ITEM_LIST 타입일 경우 필수 필드이고 최대 20자(줄바꿈: 불가)<br>- PREMIUM_VIDEO 타입일 경우 선택 필드이고 최대 20자(줄바꿈: 불가)                                                                                                                         |
| item             | Object  | O  | 와이드 리스트 요소(WIDE_ITEM_LIST 타입에서만 사용 가능)                                                                                                                                                                                           |
| - list           | List    | O  | 와이드 리스트(최소: 3, 최대 4)                                                                                                                                                                                                             |
| -- title         | String  | O  | 아이템 제목<br>- 1번째 아이템은 최대 25자 제한(줄바꿈: 최대 1개, 1번째 아이템의 경우 title이 필수 값이 아님)<br>- 1번째 아이템은 title이 필수 필드가 아님<br>- 2~4번째 아이템 최대 30자 제한(줄바꿈: 최대 1개)                                                                                     |
| -- imageUrl      | String  | O  | 아이템 이미지 URL<br>- 1번째 아이템에는 첫번째 와이드 아이템리스트 이미지로 업로드된 이미지 URL 사용<br>- 2~4번째 아이템은 일반 와이드 아이템리스트 이미지로 업로드된 이미지 URL 사용<br>치환자 사용 불가능                                                                                                   |
| -- linkMo        | String  | O  | 모바일 웹 링크, 500자 제한                                                                                                                                                                                                               |
| -- linkPc        | String  | X  | PC 웹 링크, 500자 제한                                                                                                                                                                                                                |
| -- schemeAndroid | String  | X  | 안드로이드 앱 링크, 500자 제한                                                                                                                                                                                                             |
| -- schemeIos     | String  | X  | IOS 앱 링크, 500자 제한                                                                                                                                                                                                               |
| buttons          | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                     |
| - name           | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                          |
| - type           | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야함 |
| - linkMo         | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| - linkPc         | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| - schemeAndroid  | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                           |
| - schemeIos      | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| - bizFormKey     | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                |
| coupon           | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                             |
| - title          | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                |
| - description    | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                          |
| - linkMo         | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| - linkPc         | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| - schemeAndroid  | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                     |
| - schemeIos      | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |

#### 프리미엄 동영상형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름              | 타입      | 필수 | 설명                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | 템플릿명(최대 200자)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                                             |
| content         | String  | X  | - TEXT 타입일 경우 최대 1,300자(줄바꿈: 최대 99개, URL 형식 입력 가능)<br>- IMAGE 타입일 경우 최대 400자(줄바꿈: 최대 29개, URL 형식 입력 가능)<br>- WIDE 타입일 경우 최대 76자(줄바꿈 : 최대 1개)<br>- PREMIUM_VIDEO 타입일 경우 해당 필드를 옵셔널하게 사용할 수 있음, 최대 76자(줄바꿈: 최대 1개)<br>- 이외의 타입일 경우 해당 필드를 사용하지 않음 |
| header          | String  | X  | 헤더<br>- WIDE_ITEM_LIST 타입일 경우 필수 필드이고 최대 20자(줄바꿈: 불가)<br>- PREMIUM_VIDEO 타입일 경우 선택 필드이고 최대 20자(줄바꿈: 불가)                                                                                                                                           |
| video           | Object  | O  | 동영상 요소(PREMIUM_VIDEO 타입만 사용 가능)                                                                                                                                                                                                                    |
| - videoUrl      | String  | O  | 카카오TV 동영상 URL(카카오TV에 업로드된 동영상 주소만 사용 가능), 최대 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                 |
| - thumbnailUrl  | String  | X  | 동영상 섬네일용 이미지 URL. 일반 이미지로 업로드된 URL만 사용 가능(없는 경우 카카오TV 동영상 기본 섬네일 사용), 최대 500자 제한<br>치환자 사용 불가능                                                                                                                                                    |
| buttons         | List    | X  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                                       |
| - name          | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                                            |
| - type          | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫 번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야 함                   |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 1,000자 제한                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                                               |
| - title         | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                                  |
| - description   | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                                            |
| - linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |
| - linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                       |
| - schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                                         |

#### 커머스형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| 이름                | 타입      | 필수 | 설명                                                                                                                                                                                                                                |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | 템플릿 명(최대 200자)                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                             |
| adult             | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                           |
| additionalContent | String  | X  | 부가 정보(최대 34자, 줄바꿈: 최대 1개), 커머스형에서만 사용 가능                                                                                                                                                                                          |
| image             | Object  | O  | 이미지 요소<br>- IMAGE, WIDE, COMMERCE 타입일 경우 필수 필드                                                                                                                                                                                    |
| - imageUrl        | String  | O  | 이미지 URL, 일반 이미지로 업로드된 이미지 URL 사용<br>치환자 사용 불가능                                                                                                                                                                                    |
| - imageLink       | String  | X  | 이미지 클릭 시 이동할 URL, 500자 제한<br>미설정 시 카카오톡 내 이미지 뷰어 사용<br>치환자 사용 불가능                                                                                                                                                                  |
| commerce          | Object  | O  | 커머스(COMMERCE 타입에서만 사용 가능)                                                                                                                                                                                                        |
| title             | String  | O  | 상품 제목(최대 30자, 줄바꿈: 불가)                                                                                                                                                                                                           |
| regularPrice      | Integer | O  | 정상 가격(0 ~ 99,999,999)<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{정상가격}`으로 저장됨                                                                                                                                                          |
| discountPrice     | Integer | X  | 할인가격(0 ~ 99,999,999)<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{할인가격}`으로 저장됨                                                                                                                                                            |
| discountRate      | Integer | X  | 할인율(0 ~ 100), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{할인율}`으로 저장됨                                                                                                                                      |
| discountFixed     | Integer | X  | 정액할인가격(0 ~ 999,999), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{정액할인가격}`으로 저장됨                                                                                                                            |
| buttons           | List    | O  | 버튼 목록<br>- TEXT, IMAGE 타입일 경우 쿠폰 적용 시 최대 4개, 그 외 최대 5개<br>- WIDE, WIDE_ITEM_LIST 타입일 경우 최대 2개<br>- PREMIUM_VIDEO 타입일 경우 최대 1개<br>- COMMERCE 타입일 경우 최소 1개 최대 2개                                                                     |
| - name            | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                          |
| - type            | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야함 |
| - linkMo          | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| - linkPc          | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| - schemeAndroid   | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                           |
| - schemeIos       | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| - bizFormKey      | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                |
| coupon            | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                             |
| - title           | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                |
| - description     | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                          |
| - linkMo          | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| - linkPc          | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| - schemeAndroid   | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                     |
| - schemeIos       | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |

#### 캐러셀 피드형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| 이름                | 타입      | 필수 | 설명                                                                                                                                                                                                                                |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | 템플릿 명(최대 200자)                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                             |
| pushAlarm         | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                       |
| adult             | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                           |
| carousel          | Object  | O  | 캐러셀                                                                                                                                                                                                                               |
| - list            | List    | O  | 캐러셀 리스트(최소 2개, 최대 6개)                                                                                                                                                                                                            |
| -- header         | String  | O  | 캐러셀 아이템 제목(최대 20자), 캐러셀 피드형에서만 사용 가능                                                                                                                                                                                              |
| -- message        | String  | O  | 캐러셀 아이템 제목(최대 20자), 캐러셀 아이템 메시지(최대 180자), 캐러셀 피드형에서만 사용 가능                                                                                                                                                                        |
| -- imageUrl       | String  | O  | 이미지 URL(캐러셀 피드형 이미지로 업로드된 이미지만 사용 가능)<br>치환자 사용 불가능                                                                                                                                                                              |
| -- imageLink      | String  | X  | 이미지 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                    |
| -- buttons        | List    | O  | 캐러셀 리스트 버튼 목록 최소 1개, 최대 2개                                                                                                                                                                                                        |
| --- name          | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                          |
| --- type          | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫 번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야 함 |
| --- linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| --- linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| --- schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                           |
| --- schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| --- bizFormKey    | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                |
| -- coupon         | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                             |
| --- title         | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                |
| --- description   | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                          |
| --- linkMo        | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| --- linkPc        | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| --- schemeAndroid | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                     |
| --- schemeIos     | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| - tail            | Object  | X  | 더보기 버튼 정보                                                                                                                                                                                                                         |
| -- linkMo         | String  | O  | 모바일 웹 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                 |
| -- linkPc         | String  | X  | PC 웹 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                  |
| -- schemeAndroid  | String  | X  | 안드로이드 앱 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                               |
| -- schemeIos      | String  | X  | IOS 앱 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                 |
| recipientList     | List    | O  | 수신자 목록(최대 1,000명)                                                                                                                                                                                                                 |
| - recipientNo     | String  | O  | 수신 번호                                                                                                                                                                                                                             |
| createUser        | String  | X  | 등록자(콘솔에서 발송 시 사용자 UUID로 저장)                                                                                                                                                                                                       |

#### 캐러셀 커머스형 템플릿 등록 요청

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "adult": boolean,
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
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
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
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| 이름                   | 타입      | 필수 | 설명                                                                                                                                                                                                                                |
|----------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName         | String  | O  | 템플릿 명(최대 200자)                                                                                                                                                                                                                   |
| chatBubbleType       | String  | O  | 메시지 타입(TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                             |
| pushAlarm            | boolean | X  | 메시지 푸시 알람 발송 여부(기본값: true)                                                                                                                                                                                                       |
| adult                | boolean | X  | 성인용 메시지 여부(기본값: false)                                                                                                                                                                                                           |
| carousel             | Object  | O  | 캐러셀                                                                                                                                                                                                                               |
| - head               | Object  | X  | 캐러셀 인트로                                                                                                                                                                                                                           |
| -- header            | String  | O  | 캐러셀 인트로 헤더(최대 20자)                                                                                                                                                                                                                |
| -- content           | String  | O  | 캐러셀 인트로 내용(최대 50자)                                                                                                                                                                                                                |
| -- imageUrl          | String  | O  | 캐러셀 인트로 이미지 주소(캐러셀 커머스형 이미지로 업로드된 이미지 사용, 사용되는 이미지는 캐러셀의 이미지와 비율이 동일해야 함)<br>치환자 사용 불가능                                                                                                                                          |
| -- linkMo            | String  | X  | 모바일 웹 링크(linkMo, linkPc, schemeAndroid, schemeIos 중 하나라도 사용하려는 경우 linkMo은 필수 값), 500자 제한                                                                                                                                        |
| -- linkPc            | String  | X  | PC 웹 링크, 500자 제한                                                                                                                                                                                                               |
| -- schemeAndroid     | String  | X  | 안드로이드 앱 링크, 500자 제한                                                                                                                                                                                                             |
| -- schemeIos         | String  | X  | IOS 앱 링크, 500자 제한                                                                                                                                                                                                               |
| - list               | List    | O  | 캐러셀 리스트(head가 존재할 경우 최소 1개, 최대 5개 / 그 외에는 최소 2개, 최대 6개)                                                                                                                                                                          |
| -- additionalContent | String  | X  | 부가 정보(최대 34자), 캐러셀 커머스형에서만 사용 가능                                                                                                                                                                                                  |
| -- imageUrl          | String  | O  | 이미지 URL(캐러셀 커머스형 이미지로 업로드된 이미지 사용)<br>치환자 사용 불가능                                                                                                                                                                                 |
| -- imageLink         | String  | X  | 이미지 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                    |
| -- commerce          | Object  | O  | 커머스(CAROUSEL_COMMERCE 타입에서만 사용 가능)                                                                                                                                                                                               |
| --- title            | String  | O  | 상품 제목(최대 30자, 줄바꿈: 불가)                                                                                                                                                                                                           |
| --- regularPrice     | Integer | O  | 정상 가격(0~99,999,999)<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{정상가격}`으로 저장됨                                                                                                                                                          |
| --- discountPrice    | Integer | X  | 할인가격(0 ~ 99,999,999)<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{할인가격}`으로 저장됨                                                                                                                                                            |
| --- discountRate     | Integer | X  | 할인율(0 ~ 100), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{할인율}`으로 저장됨                                                                                                                                      |
| --- discountFixed    | Integer | X  | 정액할인가격(0 ~ 999,999), 할인가격 존재시 할인율, 정액할인가격 중 하나는 필수<br>치환자 사용자 지정 불가능, 값을 비워두면 고정 치환자 `#{정액할인가격}`으로 저장됨                                                                                                                            |
| -- buttons           | List    | O  | 캐러셀 리스트 버튼 목록 최소 1개, 최대 2개                                                                                                                                                                                                        |
| --- name             | String  | O  | 버튼 제목<br>- TEXT, IMAGE 타입일 경우 최대 14자<br>- 이외의 타입일 경우 최대 8자<br>치환자 사용 불가능                                                                                                                                                          |
| --- type             | String  | O  | 버튼 타입(WL: 웹 링크, AL: 앱 링크, BK: 봇 키워드, MD: 메시지 전달, AC: 채널 추가, BC: 상담톡 전환, BT: 챗봇 전환, BF: 비즈니스 폼 )<br>- 템플릿에서는 BC 타입 이용 불가 <br>- BT 타입 <br>- 템플릿에서는 BF 타입 이용 불가<br>- AC 타입은 TEXT, IMAGE의 경우 첫번째 버튼으로, 그 외 메시지 타입의 경우 마지막 버튼으로 등록해야함 |
| --- linkMo           | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| --- linkPc           | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                           |
| --- schemeIos        | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한                                                                                                                                                                                             |
| --- bizFormKey       | String  | X  | BF 타입 버튼일 경우 비즈폼 키                                                                                                                                                                                                                |
| -- coupon            | Object  | X  | 쿠폰 요소                                                                                                                                                                                                                             |
| --- title            | String  | O  | title의 경우 5가지 형식으로 제한됨<br>- "${숫자}원 할인 쿠폰" 숫자는 1 이상 99,999,999 이하<br>- "${숫자}% 할인 쿠폰" 숫자는 1 이상 100 이하<br>- "배송비 할인 쿠폰"<br>- "${7자 이내} 무료 쿠폰"<br>- "${7자 이내} UP 쿠폰"                                                                |
| --- description      | String  | O  | 쿠폰 상세 설명<br>- WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO 타입일 경우 최대 18자, 줄바꿈: 불가<br>- 이외의 타입일 경우 최대 12자, 줄바꿈: 불가                                                                                                                          |
| --- linkMo           | String  | X  | 모바일 웹 링크(WL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| --- linkPc           | String  | X  | PC 웹 링크(WL 타입일 경우 선택 필드), 500자 제한                                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | 안드로이드 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                     |
| --- schemeIos        | String  | X  | IOS 앱 링크(AL 타입일 경우 필수 필드), 500자 제한<br>쿠폰에 linkMo 필드를 입력할 경우 나머지 필드는 선택 사항(옵션)이 되며,<br>scheme_android 또는 scheme_ios 필드에 채널 쿠폰 URL(형식: alimtalk=coupon://)을 입력할 경우 나머지 필드가 선택 사항(옵션)이 됩니다.                                       |
| - tail               | Object  | X  | 더보기 버튼 정보                                                                                                                                                                                                                         |
| -- linkMo            | String  | O  | 모바일 웹 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                 |
| -- linkPc            | String  | X  | PC 웹 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                  |
| -- schemeAndroid     | String  | X  | 안드로이드 앱 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                               |
| -- schemeIos         | String  | X  | IOS 앱 링크, 500자 제한<br>치환자 사용 불가능                                                                                                                                                                                                 |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "templateCode": String
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |
| template        | Object  | X        | 템플릿 정보 |
| - templateCode  | String  | O        | 템플릿 코드 |

### 템플릿 수정

#### 요청

[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 타입     | 설명     |
|--------------|--------|--------|
| appkey       | String | 고유의 앱키 |
| senderKey    | String | 발신 키   |
| templateCode | String | 템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Request Body]

* 템플릿 등록과 스펙이 같음

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

### 템플릿 삭제

#### 요청

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름           | 타입     | 설명     |
|--------------|--------|--------|
| appkey       | String | 고유의 앱키 |
| senderKey    | String | 발신 키   |
| templateCode | String | 템플릿 코드 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

## 이미지 관리

### 이미지 업로드

#### 요청

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Request parameter]

| 이름        | 타입     | 필수 | 설명                                                                                                                             |
|-----------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| image     | File   | O  | 이미지                                                                                                                            |
| imageType | String | O  | 이미지 타입 <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |

#### 응답

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
      "imageName": String,
  }
}
```

| 이름              | 타입      | Not Null | 설명      |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | 헤더 영역   |
| - resultCode    | Integer | O        | 결과 코드   |
| - resultMessage | String  | O        | 결과 메시지  |
| - isSuccessful  | boolean | O        | 성공 여부   |
| image           | Object  | X        | 이미지 영역  |
| - imageSeq      | Integer | O        | 이미지 시퀀스 |
| - imageUrl      | String  | O        | 이미지 URL |
| - imageName     | String  | X        | 이미지명    |

#### 업로드 이미지 규격
| 이미지 타입                     | 사용처                                                     | 업로드 이미지 규격                                                                                                                              |
|:---------------------------|:--------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------|
| IMAGE                      | 이미지형 이미지, 커머스형 이미지, 프리미엄 비디오형 썸네일                       | 권장 사이즈: 800 X 400px (가로 500px 이상)<br/>이미지 비율: 0.5 ≤ 세로 ÷ 가로 ≤ 1.333<br/>파일 형식 및 용량 제한: jpg, png / 최대 5MB                                |
| WIDE_IMAGE                 | 와이드 이미지형 이미지                                            | 권장 사이즈: 800 X 600px (가로 500px 이상)<br/>이미지 비율: 0.5 ≤ 세로 ÷ 가로 ≤ 1<br>파일 형식 및 용량 제한: jpg, png / 최대 5MB                                     |
| MAIN_WIDE_ITEMLIST_IMAGE   | 와이드 아이템 리스트형 첫번째 아이템 이미지                                | 제한 사이즈: 가로 500px 이상<br/>이미지 비율: 세로 ÷ 가로 = 0.5<br/>파일 형식 및 용량 제한: jpg, png / 최대 5MB                                                      |
| NORMAL_WIDE_ITEMLIST_IMAGE | 와이드 아이템 리스트형 2~4번쨰 아이템 이미지                              | 제한 사이즈: 가로 500px 이상<br/>이미지 비율: 세로 ÷ 가로 = 1<br/>파일형식 및 크기 : jpg, png / 각 파일 최대 5MB                                                      |
| CAROUSEL_FEED_IMAGE        | 캐러셀 피드형 셀별 이미지                                          | 권장 사이즈: 800 X 600px 또는 800 X 400px (가로 500px 이상)<br/>이미지 비율: 0.5 ≤ 세로 ÷ 가로 ≤ 1.333<br/>파일 형식 및 용량 제한: jpg, png / 최대 5MB                 |
| CAROUSEL_COMMERCE_IMAGE    | 캐러셀 커머스형 인트로 이미지, 캐러셀 커머스형 셀별 이미지 | 권장 사이즈: 800 X 600px 또는 800 X 400px (가로 500px 이상)<br/>이미지 비율: 0.5 ≤ 세로 ÷ 가로 ≤ 1.333<br/>파일 형식 및 용량 제한: jpg, png / 최대 5MB                 |

### 이미지 조회

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Request parameter]

| 이름         | 타입     | 필수 | 설명                                                                                                                             |
|------------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| imageTypes | List   | O  | 이미지 타입 <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |
| pageNum    | String | X  | 페이지 번호(기본: 1)                                                                                                                  |
| pageSize   | String | X  | 조회 건수(기본: 15)                                                                                                                  |

#### 응답

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

| 이름              | 타입      | Not Null | 설명      |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | 헤더 영역   |
| - resultCode    | Integer | O        | 결과 코드   |
| - resultMessage | String  | O        | 결과 메시지  |
| - isSuccessful  | boolean | O        | 성공 여부   |
| image           | Object  | X        | 이미지 영역  |
| - imageSeq      | Integer | O        | 이미지 시퀀스 |
| - imageUrl      | String  | O        | 이미지 URL |
| - imageName     | String  | X        | 이미지명    |

### 이미지 삭제

#### 요청

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 타입     | 설명     |
|--------|--------|--------|
| appkey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름       | 타입     | 필수 | 설명     |
|----------|--------|----|--------|
| imageSeq | String | O  | 이미지 번호 |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

## 업로드

### 비즈폼 키 업로드

#### 요청

[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

## 발신 프로필 관리

### 발신 프로필 조회

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

#### 응답

```
{
  "header":{
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "sender":{
    "plusFriendId" : String,
    "senderKey" : String,
    "categoryCode" : String,
    "unsubscribePhoneNumber": String,
    "unsubscribeAuthNumber": String,
    "status" : String,
    "statusName" : String,
    "kakaoStatus" : String,
    "kakaoStatusName" : String,
    "kakaoProfileStatus" : String,
    "kakaoProfileStatusName" : String,
    "profileSpamLevel" : String,
    "profileMessageSpamLevel" : String,
    "brandMessage" : {
      "resendAppKey": String,
      "isResend": boolean,
      "resendSendNo": String,
      "resendUnsubscribeNo": String
    },
    "dormant" : boolean,
    "marketingAgreement" : boolean,
    "block" : boolean,
    "createDate" : String,
    "initialUserRestriction" : boolean
  }
}
```

| 이름                        | 타입      | Not Null | 설명                                                                                                                    |
|:--------------------------|:--------|:---------|:----------------------------------------------------------------------------------------------------------------------|
| header                    | Object  | O        | 헤더 영역                                                                                                                 |
| - resultCode              | Integer | O        | 결과 코드                                                                                                                 |
| - resultMessage           | String  | O        | 결과 메시지                                                                                                                |
| - isSuccessful            | boolean | O        | 성공 여부                                                                                                                 |
| sender                    | Object  | X        | 발신 프로필                                                                                                                |
| - plusFriendId            | String  | O        | 플러스친구 ID                                                                                                              |
| - senderKey               | String  | O        | 발신 키                                                                                                                  |
| - categoryCode            | String  | X        | 카테고리 코드                                                                                                               |
| - unsubscribePhoneNumber  | String  | X        | 무료수신거부 전화번호                                                                                                           |
| - unsubscribeAuthNumber   | String  | X        | 무료수신거부 인증번호                                                                                                           |
| - status                  | String  | X        | NHN Cloud 플러스친구 상태 코드 <br>(YSC02: 등록 대기 중, YSC03: 정상 등록)                                                              |
| - statusName              | String  | X        | NHN Cloud 플러스친구 상태명(등록 대기 중, 정상 등록)                                                                                   |
| - kakaoStatus             | String  | X        | 카카오 플러스친구 상태 코드<br>(A: 정상, S: 차단)<br>status가 YSC02일 경우, kakaoStatus null 값을 가집니다.                                     |
| - kakaoStatusName         | String  | X        | 카카오 플러스친구 상태명(정상, 차단)<br>status가 YSC02일 경우, kakaoStatusName null 값을 가집니다.                                             |
| - kakaoProfileStatus      | String  | X        | 카카오 플러스친구 프로필 상태 코드<br>(A: 활성화, B:차단, C: 비활성화, D:삭제 E:삭제 처리 중)<br>status가 YSC02일 경우, kakaoProfileStatus null 값을 가집니다. |
| - kakaoProfileStatusName  | String  | X        | 카카오 플러스친구 프로필 상태명(활성화, 비활성화, 차단, 삭제 처리 중, 삭제)<br>status가 YSC02일 경우, kakaoProfileStatusName null 값을 가집니다.              |
| - profileSpamLevel        | String  | X        | 카카오톡 채널 스팸 상태명(영구제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                           |
| - profileMessageSpamLevel | String  | X        | 카카오톡 메시지 스팸 상태명(활동제한, 경고제한, 정상)<br>발신 프로필 상태가 정상적이지 않을 경우 null 값을 가질 수 있습니다.                                          |
| - block                   | boolean | O        | 발신 프로필 차단 여부                                                                                                           |
| - brandMessage            | Object  | X        | 브랜드 메시지 설정 정보                                                                                                         |
| -- resendAppKey           | String  | X        | 대체 발송으로 설정할 SMS 서비스 앱키                                                                                                |
| -- isResend               | boolean | O        | 대체 발송 설정(재발송) 여부                                                                                                      |
| -- resendSendNo           | String  | X        | 재발송 시, tc-sms 발신 번호                                                                                                   |
| -- resendUnsubscribeNo    | String  | X        | 재발송 시, tc-sms 080 수신 거부 번호                                                                                            |
| - dormant                 | boolean | O        | 발신 프로필 휴면 여부                                                                                                           |
| - marketingAgreement      | boolean | O        | M/N 타입 사용 신청 여부                                                                                                       |
| - createDate              | String  | X        | 등록 일자                                                                                                                 |
| - initialUserRestriction  | boolean | O        | 최초 사용자 제한 여부                                                                                                          |

### 발신 프로필 080 수신거부번호 수정

#### 요청

[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 타입     | 설명     |
|-----------|--------|--------|
| appkey    | String | 고유의 앱키 |
| senderKey | String | 발신 키   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| 이름                | 	타입     | 	필수 | 	설명                                                                                                                           |
|-------------------|---------|-----|-------------------------------------------------------------------------------------------------------------------------------|
| unsubscribeNo     | 	String | 	O  | 080 무료수신거부 전화번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx  |
| unsubscribeAuthNo | 	String | 	X  | 080 무료수신거부 인증번호(모두 미입력 시 발신 프로필에 등록된 무료수신거부 정보로 발송됨)<br>unsubscribe_phone_number 없이 unsubscribe_auth_number만 입력 불가<br>예: 1234 |

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |

## 대체 발송 관리

### SMS AppKey 등록

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request body]

```
{
    "resendAppKey": String
}
```

| 이름           | 	타입     | 	필수 | 	설명                    |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | 대체 발송으로 설정할 SMS 서비스 앱키 |

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

#### 응답

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

### 대체 발송 설정 등록

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | 콘솔에서 생성할 수 있습니다. |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 이름                  | 	타입      | 	필수 | 	설명                                                                                                        |
|---------------------|----------|-----|------------------------------------------------------------------------------------------------------------|
| senderKey           | 	String  | 	O  | 발신 키                                                                                                       |
| isResend            | 	Boolean | 	O  | 발송 실패 시, 문자 대체발송 여부<br>Console에서 대체 발송 설정 시, default로 대체 발송 됩니다.                                           |
| resendSendNo        | 	String  | 	X  | 대체 발송 발신번호<br><span style="color:red">(SMS 서비스에 등록된 발신 번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span>                  |
| resendUnsubscribeNo | 	String  | 	X  | 대체 발송 080 수신 거부 번호<br><span style="color:red">(SMS 서비스에 등록된 080 수신거부번호가 아닐 경우, 대체 발송에 실패할 수 있습니다.)</span> |

[예시]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://api-alimtalk.cloud.toast.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

#### 응답

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | 헤더 영역  |
| - resultCode    | Integer | O        | 결과 코드  |
| - resultMessage | String  | O        | 결과 메시지 |
| - isSuccessful  | boolean | O        | 성공 여부  |
