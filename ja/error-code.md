## Notification > KakaoTalk Bizmessage > 오류 코드

## API 응답 코드

| 카테고리      | 성공 여부 | 결과 코드 | 결과 코드 메시지                                                                                                                                           | API 응답 메시지                                                                                                                                                                                                                                                                                                                                                                 |
|-----------|-------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 공통        | true  | 0     | 성공                                                                                                                                                  | SUCCESS                                                                                                                                                                                                                                                                                                                                                                    |
| 공통        | false | 4     | 파라미터 유효성 검증 실패(resultMessage 참고)                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                            |
| 공통        | false | -1000 | 유효하지 않은 앱키                                                                                                                                          | Invalid appKey.                                                                                                                                                                                                                                                                                                                                                            |
| 공통        | false | -1001 | 유효하지 않은 비밀 키(secretKey)                                                                                                                             | Invalid secretKey.                                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -1002 | 유효하지 않은 SMS 앱키                                                                                                                                      | Invalid Sms appkey                                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -1003 | 유효하지 않은 SMS 발신 번호                                                                                                                                   | Invalid Sms Sendno                                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -1005 | 10분간 동일한 X-NC-API-IDEMPOTENCY-KEY 값으로 메시지 발송을 요청한 경우                                                                                                | The same 'X-NC-API-IDEMPOTENCY-KEY' was used within the last 10 minute.                                                                                                                                                                                                                                                                                                    |
| 공통        | false | -1006 | 발신 프로필이 발신 키를 가지고 있지 않은 경우                                                                                                                          | Plus friend don't have senderKey.                                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -1019 | 유효하지 않은 대체 발송 080수신거부 번호                                                                                                                            | Invalid Sms UnSubscribeno                                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -1020 | 유효하지 않은 UUID                                                                                                                                        | Invalid uuid                                                                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -1027 | 발신 프로필이 차단 상태일 경우                                                                                                                                   | The sender is blocked status.                                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -1028 | 템플릿 변수가 치환될 때 그 차이가 14개 글자를 초과하는 부분이 있는 경우(최초 사용자 제한)                                                                                               | Blacklist can't use more than 14 characters in template value.                                                                                                                                                                                                                                                                                                             |
| 공통        | false | -2000 | 특정 필드값이 최대 길이 초과할 경우                                                                                                                                | The '{}' must be at less than or equal to {}.                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -2001 | 특정 필드값이 필수 값이 공백인 경우                                                                                                                                | The '{}' must not be blank.                                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -2002 | 특정 필드값이 NULL일 경우                                                                                                                                    | The '{}' must not be null.                                                                                                                                                                                                                                                                                                                                                 |
| 공통        | false | -2003 | 특정 필드값이 최소 길이보다 작을 경우                                                                                                                               | The '{}' must be greater than or equal to {}.                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -2004 | 특정 필드값이 최대 길이보다 클 경우                                                                                                                                | The '{}' must be between {} and {}.                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -2005 | 특정 필드값이 제한된 범위 내의 값이 아닌 경우                                                                                                                          | The '{}' must be less than or equal to {}.                                                                                                                                                                                                                                                                                                                                 |
| 공통        | false | -2017 | 존재하지 않는 발신 프로필                                                                                                                                      | Not exist plus friend.                                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -2018 | 유효하지 않은 버튼 파라미터                                                                                                                                     | Button parameter is invalid.                                                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -2033 | 템플릿 아이템 파라미터가 유효하지 않은 경우                                                                                                                            | TemplateItem parameter is invalid.                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -2034 | 템플릿 아이템 하이라이트 파라미터가 유효하지 않은 경우                                                                                                                      | TemplateItemHighlight parameter is invalid.                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -2036 | TemplateRepresentLink 파라미터가 유효하지 않은 경우                                                                                                              | TemplateRepresentLink parameter is invalid.                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -2037 | 아이템 리스트 포함 템플릿 등록 시 메시지 본문 글자 수가 700자를 초과한 경우                                                                                                       | The 'content' is too long. If you request the 'templateItem', It must be less than 1000 characters.                                                                                                                                                                                                                                                                        |
| 공통        | false | -2502 | 발송 실패 설정이 되어 있지 않고, 대체 발송을 요청했을 경우                                                                                                                  | Please set up plus-friend resend setting.                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -2504 | 유효하지 않은 바로연결                                                                                                                                        | The quickReplies parameter is invalid.                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -3000 | 웹 링크(WL) 버튼 타입의 템플릿일 경우, linkMo 필수 필드                                                                                                               | If template have a WL(webLink), must input linkMo                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -3002 | 필요한 요청 본문(request body) 내용을 읽을 수 없을 경우                                                                                                              | Field is not valid.                                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -3003 | 존재하는 않는 템플릿                                                                                                                                         | Template does not exist.                                                                                                                                                                                                                                                                                                                                                   |
| 공통        | false | -3004 | 템플릿 파라미터 오류                                                                                                                                         | Please check template parameter.                                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -3005 | 템플릿 상태 오류(승인 전 발송 요청 시)                                                                                                                             | Please check template status.                                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -3006 | linkMo/linkPC 는 반드시 http:// 또는 https://를 포함해야 합니다.                                                                                                  | The linkMo/linkPc must include http:// or https://                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3007 | 앱링크(AL)는  schemeAndroid, schemeIos, linkMo 중 최소 2개 이상을 입력해야 합니다.                                                                                    | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, linkMo must be entered.                                                                                                                                                                                                                                                              |
| 공통        | false | -3008 | 버튼 명에는 치환자 입력이 불가능합니다.                                                                                                                              | The button name can't include the replacement parameter.                                                                                                                                                                                                                                                                                                                   |
| 공통        | false | -3009 | 버튼명이 존재하지 않습니다.                                                                                                                                     | Not exist button name.                                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -3010 | 등록한 템플릿 본문과 일치하지 않는 경우                                                                                                                              | The contents are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                                           |
| 공통        | false | -3026 | AC 타입 버튼의 이름은 "채널 추가" 만 등록 가능                                                                                                                       | AC type button name must have 채널 추가                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -3038 | templateItem summary의 title은 치환 변수를 가질 수 없음                                                                                                         | The templateItem's summary title can't include the replacement parameter.                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -3040 | 썸네일이 있는 아이템하이라이트의 title은 최대 21글자, description은 최대 13글자                                                                                              | The itemHighlight's title with thumbnail can not exceed 21 characters, and description can not excced 13 characters                                                                                                                                                                                                                                                        |
| 공통        | false | -3041 | imageUrl은 http:// https:// 을 반드시 포함해야 함                                                                                                             | The imageUrl must include http:// or https://                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -3051 | TN 버튼 이름은 '전화 연결', '고객센터 연결', '상담원 연결' 중 하나여야 합니다.                                                                                                  | TN button name must be one of: 전화 연결, 고객센터 연결, 상담원 연결                                                                                                                                                                                                                                                                                                                      |
| 공통        | false | -3052 | TN 버튼에는 telNumber 필드가 필수입니다.                                                                                                                        | TN button must have telNumber field.                                                                                                                                                                                                                                                                                                                                       |
| 공통        | false | -3053 | 유효하지 않은 전화번호 형식입니다. 숫자와 하이픈만 사용 가능하며, 최대 14자까지 입력할 수 있습니다.                                                                                          | Invalid telephone number format. Must be digits/hyphens only, max 14 characters.                                                                                                                                                                                                                                                                                           |
| 공통        | false | -3101 | 바로연결명이 존재하지 않는 경우                                                                                                                                   | Not exist quickReply name.                                                                                                                                                                                                                                                                                                                                                 |
| 공통        | false | -3102 | 바로연결명은 치환자를 포함할 수 없음                                                                                                                                | The quickReply name can't include the replacement parameter.                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -3303 | 필수 입력값이 누락되었습니다.                                                                                                                                    | {} is empty.                                                                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -3304 | 입력값이 유효한 범위를 벗어났습니다. 지정된 범위 내의 값을 입력해주세요.                                                                                                           | The '{}' must be between {} and {}.                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -3305 | 선택한 chatBubbleType에는 특정 필드를 사용할 수 없습니다.                                                                                                             | The chatBubbleType {} is not allowed {} field.                                                                                                                                                                                                                                                                                                                             |
| 공통        | false | -3306 | 커머스 타입 메시지에는 버튼이 반드시 포함되어야 합니다.                                                                                                                     | The commerce type require buttons.                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3307 | linkType의 필수 입력 항목이 누락되었습니다.                                                                                                                        | linkType {} is required field {}                                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -3308 | linkType의 여러 필수 입력 항목이 누락되었습니다.                                                                                                                     | linkType {} is required fields {}                                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -3309 | 템플릿에 앱링크(AL) 사용 시, schemeAndroid, schemeIos, linkMo 중 최소 두 가지를 입력해야 합니다.                                                                            | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, linkMo must be entered.                                                                                                                                                                                                                                                              |
| 공통        | false | -3310 | 웹링크 URL(linkMo/linkPc)은 'http://' 또는 'https://'를 포함해야 합니다.                                                                                          | The linkMo/linkPc must include http:// or https://                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3311 | 메시지 타입이 텍스트 또는 이미지인 경우, 채널 추가(AC) 버튼은 첫 번째 버튼이어야 합니다.                                                                                               | When the type is TEXT or IMAGE, AC  button type must be the first button.                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -3312 | 메시지 타입이 텍스트 또는 이미지가 아닌 경우, 채널 추가(AC) 버튼은 마지막 버튼이어야 합니다.                                                                                             | When the type is not TEXT or IMAGE, AC button type must be the last button.                                                                                                                                                                                                                                                                                                |
| 공통        | false | -3313 | 쿠폰 URL 정보가 잘못되었습니다. 기본 쿠폰 사용 시 linkMo 필드가, 채널 쿠폰 URL 사용 시 schemeAndroid 또는 schemeIos 필드가 필요합니다.                                                     | If a basic coupon is used instead of a channel coupon URL, the linkMo field is required. If a channel coupon URL (format: alimtalk=coupon://) is used, either schemeAndroid or schemeIos must be provided.                                                                                                                                                                 |
| 공통        | false | -3314 | 쿠폰 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                          | The coupon field require {}                                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -3315 | 쿠폰 제목이 유효하지 않습니다.                                                                                                                                   | The title is not valid coupon title                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -3316 | 커머스 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                         | commerce field require {}                                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -3317 | 비디오 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                         | video field require {}                                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -3318 | 비디오 URL이 유효한 형식이 아닙니다.                                                                                                                              | The videoUrl is not valid video url                                                                                                                                                                                                                                                                                                                                        |
| 공통        | false | -3319 | 이미지 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                         | image field require {}                                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -3320 | 캐러셀 아이템 개수가 허용된 범위(최소/최대값)를 벗어났습니다.                                                                                                                 | The carousel list size must be between {} and {}                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -3321 | 캐러셀 헤더에 링크 입력 시, linkMo는 필수입니다.                                                                                                                     | If at least one link is entered in the Carousel Head, linkMo cannot be empty.                                                                                                                                                                                                                                                                                              |
| 공통        | false | -3322 | 캐러셀 헤더 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                      | The carousel head field require {}                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3323 | 캐러셀 푸터 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                      | The carousel tail field require {}                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3324 | 템플릿 변수값이 잘못되었습니다.                                                                                                                                   | The template parameter is invalid.                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3325 | 아이템(item) 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                   | item field require {}                                                                                                                                                                                                                                                                                                                                                      |
| 공통        | false | -3326 | 이미지 URL은 'http://' 또는 'https://'를 포함해야 합니다.                                                                                                         | The imageUrl must include http:// or https://                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -3327 | 이미지 링크 URL은 'http://' 또는 'https://'를 포함해야 합니다.                                                                                                      | The imageLink must include http:// or https://                                                                                                                                                                                                                                                                                                                             |
| 공통        | false | -3328 | 허용되지 않는 버튼 타입입니다.                                                                                                                                   | The button type {} is not allowed.                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3330 | 채널 추가 버튼이 없는 세로 버튼 배열 시, BF 버튼은 첫 번째여야 합니다.                                                                                                         | BF button must be the first button in vertical layout without channel-add button.                                                                                                                                                                                                                                                                                          |
| 공통        | false | -3331 | 허용되지 않는 버튼 이름입니다.                                                                                                                                   | The button is not allowed to have {} as its name.                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -3332 | 캐러셀 커머스 아이템 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                 | The carousel commerce item field require {}                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -3333 | 캐러셀 커머스 아이템에 허용되지 않는 정보가 포함되어 있습니다.                                                                                                                 | The carousel commerce field not allowed {}                                                                                                                                                                                                                                                                                                                                 |
| 공통        | false | -3334 | 캐러셀 일반 아이템 정보 입력 시 필수 항목이 누락되었습니다.                                                                                                                  | The carousel item field require {}                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -3335 | 캐러셀 일반 아이템에 허용되지 않는 정보가 포함되어 있습니다.                                                                                                                  | The carousel item field not allowed {}                                                                                                                                                                                                                                                                                                                                     |
| 공통        | false | -3336 | 채널 추가 버튼이 있는 세로 버튼 배열 시, BF 버튼은 두 번째여야 합니다.                                                                                                         | BF button must be the second button in vertical layout with channel-add button.                                                                                                                                                                                                                                                                                            |
| 공통        | false | -3337 | 채널 추가 버튼이 없는 가로 버튼 배열 시, BF 버튼은 오른쪽(두 번째)이어야 합니다.                                                                                                   | BF button must be the second (right side) button in horizontal layout without channel-add button.                                                                                                                                                                                                                                                                          |
| 공통        | false | -3338 | 채널 추가 버튼이 있는 가로 버튼 배열 시, BF 버튼은 왼쪽(첫 번째)이어야 합니다.                                                                                                    | BF button must be the first (left side) button in horizontal layout with channel-add button.                                                                                                                                                                                                                                                                               |
| 공통        | false | -3340 | 잘못된 080 수신거부번호                                                                                                                                      | Invalid unsubscribeNo format. Expected format: 080-xxx-xxxx or 080-xxxx-xxxx or 080xxxxxxx or 080xxxxxxxx.                                                                                                                                                                                                                                                                 |
| 공통        | false | -3341 | 잘못된 080 인증번호                                                                                                                                        | Invalid format. Only digits are allowed, up to a maximum of 9.                                                                                                                                                                                                                                                                                                             |
| 공통        | false | -4000 | 유효하지 않은 파라미터                                                                                                                                        | Invalid parameter                                                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -4003 | 조회 범위는 한 달 초과                                                                                                                                       | Search is possible within 31 days.                                                                                                                                                                                                                                                                                                                                         |
| 공통        | false | -4004 | 존재하지 않는 앱키                                                                                                                                          | Not exist appkey.                                                                                                                                                                                                                                                                                                                                                          |
| 공통        | false | -4005 | 사용 종료 상태의 앱키                                                                                                                                        | Appkey is disabled status.                                                                                                                                                                                                                                                                                                                                                 |
| 공통        | false | -4007 | 파일 사이즈 초과                                                                                                                                           | The file size is less than {}.                                                                                                                                                                                                                                                                                                                                             |
| 공통        | false | -4009 | 유효하지 않은 파일 확장자                                                                                                                                      | Check the file extension.                                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -4010 | 파일을 찾을 수 없음                                                                                                                                         | Not found file.                                                                                                                                                                                                                                                                                                                                                            |
| 공통        | false | -4011 | 수신자 목록을 찾을 수 없음                                                                                                                                     | Not found recipientList.                                                                                                                                                                                                                                                                                                                                                   |
| 공통        | false | -4014 | 파일에 recipient_no 헤더가 없음                                                                                                                             | There is no recipient_no header in the file.                                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -4016 | 데이터가 존재하지 않음                                                                                                                                        | Not exist data.                                                                                                                                                                                                                                                                                                                                                            |
| 공통        | false | -4018 | 파일 업로드 오류                                                                                                                                           | Upload attach file error.                                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -4020 | 파일 읽기 실패                                                                                                                                            | Failed to reading the file.                                                                                                                                                                                                                                                                                                                                                |
| 공통        | false | -4023 | 상품을 비활성화하려면 모든 발신자를 삭제해야 함                                                                                                                          | For disabling the product, have to delete all senders                                                                                                                                                                                                                                                                                                                      |
| 공통        | false | -4101 | 유효하지 않은 통계 조회 파라미터                                                                                                                                  | Invalid search parameter.                                                                                                                                                                                                                                                                                                                                                  |
| 공통        | false | -4103 | 유효하지 않은 요청Id 또는 요청시작일시/요청종료일시                                                                                                                       | RequestId or startRequestDate/endRequestDate is invalid.                                                                                                                                                                                                                                                                                                                   |
| 공통        | false | -5000 | 유효하지 않은 수신 번호                                                                                                                                       | RecipientNo is invalid.                                                                                                                                                                                                                                                                                                                                                    |
| 공통        | false | -7000 | 벤더 요청 API 실패                                                                                                                                        | Vender request API is failed.                                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -8001 | 이미지 파일이 정상적이지 않은 경우                                                                                                                                 | The image you upload is invalid.                                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -8002 | 이미지를 찾을 수 없는 경우. <br/>캐러셀 피 - 캐러셀 타입 이미지를 사용<br/> 캐러셀 커머스 - 커머스 타입 이미지를 사용 <br/> 커머스 유형 - 이미지 타입을 사용                                                | It is not found any images. If you want to send carousel-feed type messages, you must use carousel type images. If you want to send carousel-commerce type messages, you must use commerce type images. If you want to send commerce type messages, you must use IMAGE type images.                                                                                        |
| 공통        | false | -8007 | 스토리지 설정이 비어있음                                                                                                                                       | The storage configs can't empty.                                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -8009 | 이미 공유된 프로젝트                                                                                                                                         | This project has already been shared.                                                                                                                                                                                                                                                                                                                                      |
| 공통        | false | -8010 | 서버 이슈로 이미지 파일 업로드 실패                                                                                                                                | Uploading image file has failed for an unexpected error.                                                                                                                                                                                                                                                                                                                   |
| 공통        | false | -9992 | fade-out된 친구톡 API를 호출했을 경우                                                                                                                          | The API is no longer supported. Please migrate to the new brand-message endpoint/API.                                                                                                                                                                                                                                                                                      |
| 공통        | false | -9993 | 필수 요청 부분이 누락됨                                                                                                                                       | Required request part is not present.                                                                                                                                                                                                                                                                                                                                      |
| 공통        | false | -9994 | 메서드 인수의 타입이 예상과 다름                                                                                                                                  | A method argument has not the expected type.                                                                                                                                                                                                                                                                                                                               |
| 공통        | false | -9996 | Content-type이 application/json가 아닐 경우                                                                                                               | Only application/json Content-type is supported.                                                                                                                                                                                                                                                                                                                           |
| 공통        | false | -9997 | 적절하지 않은 요청으로 인한 실패                                                                                                                                  | Client Error.                                                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -9998 | 존재하지 않는 API                                                                                                                                         | Not exist API                                                                                                                                                                                                                                                                                                                                                              |
| 공통        | false | -9999 | 시스템 오류                                                                                                                                              | System error. Please inquire at support@toast.com.                                                                                                                                                                                                                                                                                                                         |
| 발신 프로필    | false | -3342 | 마케팅 수신동의 증적자료 업로드 실패                                                                                                                                | Failed to upload marketing agreement file.                                                                                                                                                                                                                                                                                                                                 |
| 발신 프로필    | false | -3343 | M/N 타입 사용 신청 실패                                                                                                                                     | {}                                                                                                                                                                                                                                                                                                                                                                         |
| 발신 프로필    | false | -3345 | 080 수신거부정보가 유효하지 않음                                                                                                                                 | Failed to update unsubscribe content. message: {}                                                                                                                                                                                                                                                                                                                          |
| 발신 프로필 그룹 | false | -1010 | 발신 프로필 그룹이 존재하지 않을 경우                                                                                                                               | Not exist plus friend group.                                                                                                                                                                                                                                                                                                                                               |
| 발신 프로필 그룹 | false | -1013 | 이미 존재하는 발신 프로필 그룹일 경우                                                                                                                               | Already exist plus friend group.                                                                                                                                                                                                                                                                                                                                           |
| 발신 프로필 그룹 | false | -1018 | 발신 프로필이 이미 발신 프로필 그룹에 등록된 경우                                                                                                                      | This is a plusFriend that has already been added.                                                                                                                                                                                                                                                                                                                          |
| 발신 프로필 그룹 | false | -1022 | 발신 프로필이 발신 프로필 그룹에 등록되지 않은 경우                                                                                                                     | This is not a plusFriend added to the group.                                                                                                                                                                                                                                                                                                                               |
| 발신 프로필 그룹 | false | -1023 | 발신 프로필 그룹 내 발신 프로필이 최대 10개가 초과한 경우                                                                                                                | The max group size is 10.                                                                                                                                                                                                                                                                                                                                                  |
| 발신 프로필 그룹 | false | -1025 | 발신 프로필 그룹 삭제 불가                                                                                                                                   | The sender-group can't deleted.                                                                                                                                                                                                                                                                                                                                            |
| 발신 프로필 그룹 | false | -1026 | 발신 프로필 그룹의 멤버가 5000개를 초과한 경우                                                                                                                       | The maximum number of members in a group is 5000.                                                                                                                                                                                                                                                                                                                          |
| 발신 프로필 그룹 | false | -1029 | 발신 프로필이 블랙리스트인 경우 발신 프로필 그룹에 둥록 불가                                                                                                                  | Blacklist can't join the group.                                                                                                                                                                                                                                                                                                                                            |
| 템플릿       | false | -1014 | 활성화되지 않은 발신 프로필일 경우                                                                                                                                | Not active status plus friend.                                                                                                                                                                                                                                                                                                                                             |
| 템플릿       | false | -2505 | 바로연결 포함 발송 시, 최대 버튼 개수 2개를 초과한 경우                                                                                                                   | The 'buttons' is too many. If you request the 'quickReplies', the buttons size muse be 2 or less.                                                                                                                                                                                                                                                                          |
| 템플릿       | false | -3001 | 이미 존재하는 템플릿 코드 또는 템플릿명                                                                                                                              | Already exist templateCode or templateName.                                                                                                                                                                                                                                                                                                                                |
| 템플릿       | false | -3012 | 수정할 수 없는 템플릿 상태(승인/반려 상태만 가능)                                                                                                                       | Only templates with TSC03(APPROVE)/TSC04(REJECT) can be modified.                                                                                                                                                                                                                                                                                                          |
| 템플릿       | false | -3013 | 이미 수정 중인 템플릿이 존재                                                                                                                                    | There are templates already being modified.                                                                                                                                                                                                                                                                                                                                |
| 템플릿       | false | -3016 | 강조 표기형 템플릿은 templateTitle, templateSubtitle 필수 필드                                                                                                   | The Template which emphasizeType is 'TEXT' must have templateTitle, templateSubtitle.                                                                                                                                                                                                                                                                                      |
| 템플릿       | false | -3017 | templateSubtitle 은 치환 변수 사용 불가                                                                                                                     | The templateSubtitle can't include the replacement parameter.                                                                                                                                                                                                                                                                                                              |
| 템플릿       | false | -3018 | 부가 정보형 템플릿은 templateExtra 필수 필드                                                                                                                     | The Template which messageType is 'EX' must have templateExtra.                                                                                                                                                                                                                                                                                                            |
| 템플릿       | false | -3020 | 복합형 템플릿은 templateExtra 필수 필드                                                                                                                        | The Template which messageType is 'MI' must have templateExtra.                                                                                                                                                                                                                                                                                                            |
| 템플릿       | false | -3021 | templateExtra은 치환 변수 사용 불가                                                                                                                         | The templateExtra can't include the replacement parameter.                                                                                                                                                                                                                                                                                                                 |
| 템플릿       | false | -3024 | AC 타입 버튼은 채널 추가형 복합형 템플릿만 등록 가능                                                                                                                     | The button of AC type can using only templateMessageType (AD/MI).                                                                                                                                                                                                                                                                                                          |
| 템플릿       | false | -3025 | AC 타입 버튼은 단독 사용 또는 가장 상단 배치해야 하며, 해당 제약에 어긋난 경우                                                                                                       | AC type buttons should be located alone or on top.                                                                                                                                                                                                                                                                                                                         |
| 템플릿       | false | -3027 | 템플릿 강조 타입 선택 안 함(NONE)으로 등록 시, templateTitle, templateSubtitle 필드를 등록할 수 없음                                                                       | The Template which emphasizeType is 'NONE' can't have templateTitle, templateSubtitle.                                                                                                                                                                                                                                                                                     |
| 템플릿       | false | -3028 | 템플릿 메시지 타입이 기본형(BA)일 경우, templateExtra 필드를 등록할 수 없음                                                                                                  | The Template which messageType is 'BA' can't have templateExtra.                                                                                                                                                                                                                                                                                                           |
| 템플릿       | false | -3030 | 템플릿 메시지 타입이 채널 추가형(AD)일 경우, templateExtra 필드를 등록할 수 없음                                                                                               | The Template which messageType is 'AD' can't have templateExtra.                                                                                                                                                                                                                                                                                                           |
| 템플릿       | false | -3032 | 템플릿 강조 타입이 이미지형(IMAGE)일 경우, templateImageName, templateImageUrl 필드가 필수 값임                                                                            | The Template which emphasizeType is 'IMAGE' must have templateImageName, templateImageUrl.                                                                                                                                                                                                                                                                                 |
| 템플릿       | false | -3037 | templateItem list의 title은 치환 변수를 가질 수 없음                                                                                                            | The templateItem's title can't include the replacement parameter.                                                                                                                                                                                                                                                                                                          |
| 템플릿       | false | -3047 | 템플릿 등록 시 서체 스타일을 적용하려고 한 경우 (서체 스타일은 발송 시점에 적용 가능합니다.)                                                                                              | The templateTitle and templateItemHighlight's title can't end with \s.                                                                                                                                                                                                                                                                                                     |
| 템플릿       | false | -3050 | 템플릿 메시지 타입(AD/MI)은 AC 타입의 버튼을 가져야 함                                                                                                              | The templateMessageType (AD/MI) must have button of AC type.                                                                                                                                                                                                                                                                                                               |
| 템플릿       | false | -3100 | 템플릿 요청/승인 상태의 경우 comment를 추가할 수 없음                                                                                                                  | Could not add comment on registered/completed template status.                                                                                                                                                                                                                                                                                                             |
| 템플릿       | false | -3103 | button 또는 바로연결의 형식이 올바르지 않은 경우(json 형식이 아니거나, escape 처리되지 않은 문자가 포함됨)                                                                               | The button or quickReply has an invalid format.                                                                                                                                                                                                                                                                                                                            |
| 템플릿       | false | -4021 | 파일 사이즈 초과(10MB)                                                                                                                                    | The file size is less than 10MB.                                                                                                                                                                                                                                                                                                                                           |
| 템플릿       | false | -4024 | 휴면 템플릿 해제 실패                                                                                                                                        | Failed to release the dormant template.                                                                                                                                                                                                                                                                                                                                    |
| 템플릿       | false | -4025 | 템플릿 업로드 시, 한번에 업로드 가능한 최대 개수인 20개를 초과한 경우                                                                                                                           | Only upload up to 20 templates at a time.                                                                                                                                                                                                                                                                                                                                  |
| 템플릿       | false | -4026 | 템플릿 업로드 파일의 헤더가 유효하지 않은 경우                                                                                                                          | Uploaded template's headers are invalid.                                                                                                                                                                                                                                                                                                                                   |
| 템플릿       | false | -4027 | 채널 추가형 전환 실패. button 길이 초과                                                                                                                          | Conversion to AD/MI type failed. The 'buttons' length does not exceed its maximum length.                                                                                                                                                                                                                                                                                  |
| 템플릿       | false | -4028 | 채널 추가형 전환 실패. 승인되지 않은 템플릿 사용                                                                                                                        | Conversion to AD/MI type failed. The template must be approved                                                                                                                                                                                                                                                                                                             |
| 발송/조회     | false | -1016 | requestId 혹은 recipientSeq로 메시지를 찾을 수 없는 경우                                                                                                          | It is not found any messages responding with that requestId or recipientSeq.                                                                                                                                                                                                                                                                                               |
| 발송/조회     | false | -1024 | 발신 프로필이 그룹으로 메시지를 발송할 경우                                                                                                                            | The sender-group can't send the message.                                                                                                                                                                                                                                                                                                                                   |
| 발송/조회     | false | -1031 | 발송 요청한 전체 수신자가 발송에 실패했을 경우                                                                                                                          | All of receivers are failed to send.                                                                                                                                                                                                                                                                                                                                       |
| 발송/조회     | false | -2019 | 템플릿 본문 1,300글자 초과로 실패                                                                                                                               | The content that is replaced by the template parameters can not exceed 1,300 characters.                                                                                                                                                                                                                                                                                   |
| 발송/조회     | false | -2025 | 예약 일시가 과거의 일시일 경우                                                                                                                                   | You can not send messages on past dates. Please check `requestDate` again.                                                                                                                                                                                                                                                                                                 |
| 발송/조회     | false | -2026 | 예약 일시가 90일 이후의 일시일 경우(최대 90일까지 가능)                                                                                                                  | You can not send messages after 90 days. Please check `requestDate` again.                                                                                                                                                                                                                                                                                                 |
| 발송/조회     | false | -2027 | 날짜 형식이 달라 파싱 오류 발생                                                                                                                                  | The 'requestDate' has invalid format.                                                                                                                                                                                                                                                                                                                                      |
| 발송/조회     | false | -2028 | 유효하지 않은 요청 ID                                                                                                                                       | The `requestId` is invalid                                                                                                                                                                                                                                                                                                                                                 |
| 발송/조회     | false | -2029 | 요청한 메시지가 없거나, 취소할 수 있는 메시지가 없는 경우                                                                                                                   | All of your messages to cancel are not found or do not meet conditions to cancel.                                                                                                                                                                                                                                                                                          |
| 발송/조회     | false | -2030 | 와이드 이미지 발송 시, 최대 본문 길이인 76자를 초과한 경우                                                                                                                   | The 'content' is too long. If you request the 'wide-image', It must be less than 76 characters.                                                                                                                                                                                                                                                                            |
| 발송/조회     | false | -2031 | 와이드 이미지 발송 시, 최대 버튼 개수 2개를 초과한 경우                                                                                                                    | The 'buttons' is too many. If you request the 'wide-image', It must be less than 2 buttons size.                                                                                                                                                                                                                                                                           |
| 발송/조회     | false | -2032 | 템플릿 제목이 50자를 초과한 경우                                                                                                                                 | The templateTitle that is replaced by the template parameters can not exceed 50 characters.                                                                                                                                                                                                                                                                                |
| 발송/조회     | false | -2035 | 템플릿 헤더가 16자를 초과한 경우                                                                                                                                 | The templateHeader that is replaced by the template parameters can not exceed 16 characters.                                                                                                                                                                                                                                                                               |
| 발송/조회     | false | -2500 | 대량 발송 요청을 찾을 수 없는 경우                                                                                                                                | Not found mass message request                                                                                                                                                                                                                                                                                                                                             |
| 발송/조회     | false | -2501 | 요청한 메시지가 없거나, 취소할 수 있는 메시지가 없는 경우                                                                                                                   | Send request is failed. because the deadline is expired                                                                                                                                                                                                                                                                                                                    |
| 발송/조회     | false | -3011 | 등록한 템플릿 버튼과 일치하지 않는 경우                                                                                                                              | The buttons or quickReplies are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                            |
| 발송/조회     | false | -3033 | 버튼 또는 바로연결이 템플릿에 등록되지 않았을 경우                                                                                                                        | The button or quickReply is not exist in template.                                                                                                                                                                                                                                                                                                                         |
| 발송/조회     | false | -3034 | 삭제할 템플릿이 3일 이내 발송 이력이 있을 경우                                                                                                                          | The template could not be deleted because recently sent message. requestId: {}                                                                                                                                                                                                                                                                                             |
| 발송/조회     | false | -3035 | 템플릿 강조 타입을 아이템리스트형(ITEM_LIST)으로 등록 시, templateHeader, templateImageName, templateImageUrl, templateItem, templateItemHighlight 필드 중 최소 하나를 포함해야 함   | The Template which emphasizeType is 'ITEM_LIST' must have at least one of templateImageInfo, templateHeader, templateItem and templateItemHighlight.                                                                                                                                                                                                                       |
| 발송/조회     | false | -3036 | 템플릿 강조 타입을 아이템리스트형(ITEM_LIST)으로 등록 시, 보안 템플릿으로 등록할 수 없음                                                                                            | The Template which emphasizeType is 'ITEM_LIST' can't be a security template.                                                                                                                                                                                                                                                                                              |
| 발송/조회     | false | -3039 | templateItem summary은 templateItem list 없이 존재할 수 없음                                                                                                  | The TemplateItem's summary can't exist without templateItem list                                                                                                                                                                                                                                                                                                           |
| 발송/조회     | false | -3042 | templateHeader가 템플릿과 일치하지 않는 경우                                                                                                                     | The templateHeader are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                                     |
| 발송/조회     | false | -3043 | templateItem 또는 templateItemHighlight이 템플릿과 일치하지 않는 경우                                                                                              | The templateItem or templateItemHighlight are not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                              |
| 발송/조회     | false | -3044 | 비즈니스폼(BF) 타입 버튼은 단독 사용 또는 가장 상단 배치해야 하며, 해당 제약에 어긋난 경우                                                                                                | BF type buttons should be located on top.                                                                                                                                                                                                                                                                                                                                  |
| 발송/조회     | false | -3045 | 비즈니스폼(BF) 타입의 name이 "톡에서 예약하기", "톡에서 설문하기", "톡에서 응모하기"가 아닐 경우 또는 등록되지 않은 bizFormKey일 경우                                                           | The button linkType 'BF' must follow this constraint. The BF button must have a bizFormKey. The BF button name must be in "톡에서 예약하기", "톡에서 설문하기", "톡에서 응모하기".                                                                                                                                                                                                              |
| 발송/조회     | false | -3046 | templateRepresentLink이 템플릿과 일치하지 않는 경우                                                                                                              | The templateRepresentLink is not matched to template. This message can be resend by sms service if the resend setting is on.                                                                                                                                                                                                                                               |
| 발송/조회     | false | -3048 | 템플릿 파라미터는 1300자를 넘을 수 없음                                                                                                                        | The template parameter's length can not exceed 1300 characters.                                                                                                                                                                                                                                                                                                            |
| 발송/조회     | false | -3049 | 템플릿 파라미터가 템플릿과 일치하지 않음                                                                                                                          | The template parameter is not matched to template.                                                                                                                                                                                                                                                                                                                         |
| 발송/조회     | false | -3298 | 타겟팅 정보가 설정되지 않음                                                                                                                                 | The targeting is invalid.                                                                                                                                                                                                                                                                                                                                                  |
| 발송/조회     | false | -3299 | 커머스 변수는 지정된 조합으로만 사용해야 함: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed'] | The commerce variable must be used in the following combinations: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed']                                                                                                                                                                                  |
| 발송/조회     | false | -3300 | 수신거부 처리된 번호를 찾을 수 없음                                                                                                                             | The unsubscribe number could not be found.                                                                                                                                                                                                                                                                                                                                 |
| 발송/조회     | false | -3301 | 수신거부한 사용자가 포함되어 있음                                                                                                                               | The unsubscribed recipient are found.                                                                                                                                                                                                                                                                                                                                      |
| 발송/조회     | false | -3344 | 이미지 변경 파라미터 개수가 템플릿 이미지 개수와 일치하지 않음                                                                                                                 | The image parameter is invalid. image parameter's size must be equal to a number of template images.                                                                                                                                                                                                                                                                       |
| 발송/조회     | false | -4017 | 90일 이전 메시지는 조회할 수 없음                                                                                                                                | You can not search the messages before 90 days.                                                                                                                                                                                                                                                                                                                            |
| 발송/조회     | false | -4019 | 유효하지 않은 수신자 번호                                                                                                                                      | Invalid recipient number.                                                                                                                                                                                                                                                                                                                                                  |
| 발송/조회     | false | -4022 | 데이터 내보내기 실패                                                                                                                                         | Failed to exporting data.                                                                                                                                                                                                                                                                                                                                                  |
| 발송/조회     | false | -4029 | 발신 프로필 마케팅 수신 동의가 활성화되지 않은 경우                                                                                                                       | To send 'M/N type' messages to users who are not friends with your profile, you must first apply to have the 'M/N type' messaging feature enabled. Please check if this option is active in your sender profile settings.                                                                                                                                                  |
| 발송/조회     | false | -4104 | RequestId가 비어있음                                                                                                                                     | RequestId is empty value.                                                                                                                                                                                                                                                                                                                                                  |
| 발송/조회     | false | -4200 | 유효하지 않은 대체 발송 메시지                                                                                                                                   | Resend Message is invalid.                                                                                                                                                                                                                                                                                                                                                 |
| 발송/조회     | false | -8006 | 인증 메시지 발송 시, 템플릿 내용에 인증 문구가 없는 경우                                                                                                                   | The content must contain auth guidement.                                                                                                                                                                                                                                                                                                                                   |
| 발송/조회     | false | -8008 | 메시지 내용에 금칙어가 포함된 경우                                                                                                                                 | The content has banned word.                                                                                                                                                                                                                                                                                                                                               |
| 이미지       | false | -8000 | 이미지 시퀀스(imageSeq)가 없는 경우                                                                                                                            | The 'imageSeq' is empty.                                                                                                                                                                                                                                                                                                                                                   |
| 이미지       | false | -8003 | 이미지 삭제에 실패하는 경우                                                                                                                                     | It is failed to delete images.                                                                                                                                                                                                                                                                                                                                             |
| 이미지       | false | -8004 | createUser 필드 최대 글자인 100자를 초과한 경우                                                                                                                       | The 'createUser' is too long. It can be less than 100 characters.                                                                                                                                                                                                                                                                                                          |
| 이미지       | false | -8005 | 이미지 업로드 시 프로젝트에 등록된 플러스 친구가 없는 경우                                                                                                                    | Your project doesn't have any plus friends. Please register it at first.                                                                                                                                                                                                                                                                                                   |
| 이미지       | false | -8011 | 존재하지 않는 이미지 유형                                                                                                                                      | The image type is invalid.                                                                                                                                                                                                                                                                                                                                                 |
| 금지어       | false | -8012 | 금칙어가 존재하지 않음                                                                                                                                        | Banned word does not exist.                                                                                                                                                                                                                                                                                                                                                |
| 금지어       | false | -8013 | 금칙어가 이미 존재함                                                                                                                                         | Banned word already exists.                                                                                                                                                                                                                                                                                                                                                |
| 본인인증      | false | -1030 | 본인 인증을 받지 않은 경우                                                                                                                                      | Self verification is required to use this service.                                                                                                                                                                                                                                                                                                                         |
| 친구톡 호환 발송 | false | -2023 | 메시지 본문이 400글자를 초과할 경우(이미지 첨부)                                                                                                                    | The 'content' is too long. If you request the 'image', It can be less than 400 characters.                                                                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -2024 | 메시지 본문이 1,300글자를 초과할 경우                                                                                                                          | The 'content' is too long. It can be less than 1,300 characters without any image.                                                                                                                                                                                                                                                                                         |
| 친구톡 호환 발송 | false | -3200 | 와이드 아이템 리스트에 이름이 없음                                                                                                                                 | Friendtalk wide item must have a title.                                                                                                                                                                                                                                                                                                                                    |
| 친구톡 호환 발송 | false | -3201 | 와이드 아이템 리스트에 이미지가 없음                                                                                                                                | Friendtalk wide item must have a image.                                                                                                                                                                                                                                                                                                                                    |
| 친구톡 호환 발송 | false | -3202 | 와이드 아이템 리스트에 linkMo가 없음                                                                                                                             | Friendtalk wide item must have a linkMo.                                                                                                                                                                                                                                                                                                                                   |
| 친구톡 호환 발송 | false | -3203 | 와이드 아이템 리스트는 3~4개의 리스트와 헤더가 필요함                                                                                                                      | Friendtalk wide item must have 3 ~ 4 list and a header.                                                                                                                                                                                                                                                                                                                    |
| 친구톡 호환 발송 | false | -3204 | 캐러셀에 헤더가 없음                                                                                                                                         | Friendtalk carousel must have a header.                                                                                                                                                                                                                                                                                                                                    |
| 친구톡 호환 발송 | false | -3205 | 캐러셀에 메시지가 없음                                                                                                                                        | Friendtalk carousel must have a message.                                                                                                                                                                                                                                                                                                                                   |
| 친구톡 호환 발송 | false | -3206 | 캐러셀에 첨부파일이 없음                                                                                                                                       | Friendtalk carousel must have a attachment.                                                                                                                                                                                                                                                                                                                                |
| 친구톡 호환 발송 | false | -3207 | 캐러셀에 이미지가 없음                                                                                                                                        | Friendtalk carousel must have a image.                                                                                                                                                                                                                                                                                                                                     |
| 친구톡 호환 발송 | false | -3208 | 캐러셀은 2 \~ 10개 리스트 필요<br/> 캐러셀 커머스 타입에 intro가 있으면 1 \~ 10개 리스트 필요                                                                                              | Friendtalk carousel must have 2 ~ 10 list. If message type is friendtalk carousel-commerce and carousel intro exists, carousel must have 1 ~ 10 list.                                                                                                                                                                                                                      |
| 친구톡 호환 발송 | false | -3209 | 캐러셀 꼬리에 linkMo가 없음                                                                                                                                  | Friendtalk carousel tail must have linkMo.                                                                                                                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -3210 | 쿠폰은 제목과 설명이 필요함                                                                                                                                     | Friendtalk coupon must have a title and a description.                                                                                                                                                                                                                                                                                                                     |
| 친구톡 호환 발송 | false | -3211 | 친구톡 텍스트/이미지 타입 메시지의 쿠폰 설명은 12자 초과 불가, FW/FL 타입은 18자 초과 불가                                                                                            | If the message type is friendTalk text/image type, the friendtalk coupon description's length cannot exceed 12 characters. If the message type is friendTalk wide-image/wide-item-list type, the friendtalk coupon description's length cannot exceed 18 characters                                                                                                        |
| 친구톡 호환 발송 | false | -3212 | 쿠폰 제목 내용이 유효하지 않음                                                                                                                                   | Friendtalk coupon title is invalid.                                                                                                                                                                                                                                                                                                                                        |
| 친구톡 호환 발송 | false | -3213 | 쿠폰은 모바일 링크 또는 채널 타입 ios/android 링크가 필요함                                                                                                             | Friendtalk must have a mobile link or a channel formatted ios/android link                                                                                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -3215 | 와이드 아이템 리스트와 캐러셀은 광고 타입에만 사용 가능                                                                                                                     | Friendtalk wide item / carousel can only be sent in AD type.                                                                                                                                                                                                                                                                                                               |
| 친구톡 호환 발송 | false | -3216 | 와이드 아이템 리스트의 첫 번째 아이템 제목은 25자 초과 불가, 2 ~ 4번째 아이템 제목은 30자 초과 불가                                                                                         | Friendtalk first wide item's title length cannot exceed 25 characters, and 2nd ~ 4th wide item's title length cannot exceed 30 characters.                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -3217 | 친구톡 텍스트/이미지 타입은 버튼 5개 초과 불가, 쿠폰 포함 시 4개까지 가능, 와이드 이미지/와이드 아이템 리스트는 2개 초과 불가, 프리미엄 비디오 타입은 1개 초과 불가, 커머스 타입은 1~2개 버튼까지 가능                                   | Friendtalk button size is invalid. The button size must be 5 or less. If coupon is included, the button size must be 4 or less. If the message type is friendTalk wide-image/wide-item-list type, the button size must be 2 or less. If the message type is video type, the button size must be 1 or less. If the message type is commerce, the button size must be 1 or 2 |
| 친구톡 호환 발송 | false | -3218 | 친구톡 videoUrl이 유효하지 않음                                                                                                                               | Friendtalk video url is invalid.                                                                                                                                                                                                                                                                                                                                           |
| 친구톡 호환 발송 | false | -3219 | 친구톡 내용이 최대 길이를 초과함. 프리미엄 비디오 타입 내용의 최대 길이는 76자임                                                                                                         | The 'content' is too long. If you request the 'video', It must be less than 76 characters.                                                                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -3220 | 친구톡 헤더가 최대 길이를 초과함. 프리미엄 비디오 타입 헤더의 최대 길이는 20자임                                                                                                         | The 'header' is too long. If you request the 'video', It must be 20 characters or fewer.                                                                                                                                                                                                                                                                                  |
| 친구톡 호환 발송 | false | -3221 | 캐러셀 피드 타입은 캐러셀 인트로 사용 불가                                                                                                                            | Friendtalk carousel feed type cannot have a 'head' field.                                                                                                                                                                                                                                                                                                                  |
| 친구톡 호환 발송 | false | -3222 | 캐러셀 피드 타입은 부가 정보 필드 사용 불가                                                                                                                           | Friendtalk carousel feed type cannot have a 'additionalContent' field.                                                                                                                                                                                                                                                                                                     |
| 친구톡 호환 발송 | false | -3223 | 캐러셀 피드 타입은 커머스 사용 불가                                                                                                                                | Friendtalk carousel feed type cannot have a 'commerce' field.                                                                                                                                                                                                                                                                                                              |
| 친구톡 호환 발송 | false | -3224 | 캐러셀 커머스 타입은 헤더와 메시지 필드 사용 불가                                                                                                                        | Friendtalk carousel commerce type cannot have 'header' & 'message' fields.                                                                                                                                                                                                                                                                                                 |
| 친구톡 호환 발송 | false | -3225 | 캐러셀 버튼이 유효하지 않음. 캐러셀 피드 타입은 2개 버튼 초과 불가, 캐러셀 커머스 타입은 1 ~ 2개 버튼 필요                                                                                     | Friendtalk carousel button size is invalid. If the message type is friendtalk carousel-feed, the button size must be 2 or less. If the message type is friendtalk carousel-commerce, the button size must be 1 ~ 2.                                                                                                                                                        |
| 친구톡 호환 발송 | false | -3226 | 커머스에 discountPrice 필드가 있으면 discountRate 또는 discountFixed 필드 필요                                                                                      | If commerce has 'discountPrice' field, commerce must have a 'discountRate' or 'discountFixed' field.                                                                                                                                                                                                                                                                       |

## 발송 결과 코드

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>코드값</th>
		<th>의미</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>1000</td>
		<td>성공</td>
	</tr>
  <tr>
		<td>1001</td>
		<td>Request Body가 Json형식이 아님</td>
	</tr>
  <tr>
		<td>1002</td>
		<td>허브 파트너 키가 유효하지 않음</td>
	</tr>
  <tr>
		<td>1003</td>
		<td>발신 프로필 키가 유효하지 않음</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>Request Body(JSON)에서 name을 찾을 수 없음</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>삭제된 발신 프로필(고객센터에게 문의)</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>차단 상태의 발신 프로필(고객센터에게 문의)</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>계약 정보를 찾을 수 없음(고객센터에게 문의)</td>
	</tr>
  <tr>
		<td>1012</td>
		<td>잘못된 형식의 사용자키 요청</td>
	</tr>
  <tr>
		<td>1013</td>
		<td>유효하지 않은 앱 연결</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>유효하지 않은 사업자번호</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>유효하지 않은 app user id 요청</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>사업자등록번호 불일치</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>전화번호 or app user id가 유효하지 않거나 미입력 요청</td>
	</tr>
 	<tr>
		<td>1021</td>
		<td>차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1022</td>
		<td>닫힘 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1023</td>
		<td>삭제된 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1024</td>
		<td>삭제 대기 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>1025</td>
		<td>채널 제재 상태로 인한 메시지 전송 실패</td>
	</tr>
	<tr>
		<td>1027</td>
		<td>채널 메시지 제재 상태로 인한 메시지 전송 실패</td>
	</tr>
    <tr>
       <td>1028</td>
       <td>해당 타겟팅 옵션을 사용할 수 없음</td>
    </tr>
	<tr>
		<td>1030</td>
		<td>잘못된 파라미터 요청</td>
	</tr>
    <tr>
       <td>1033</td>
       <td>템플릿 메시지 타입과 chat\_bubble\_type 파라미터 불일치</td>
    </tr>
	<tr>
		<td>2001</td>
		<td>메시지 전송 불가(예기치 않은 오류 발생)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>메시지 전송 실패(테스트 서버에서 카카오톡 채널을 추가하지 않은 경우)</td>
	</tr>
    <tr>
		<td>2005</td>
		<td>카카오 내부 시스템 오류로 이미지 정보를 읽어오는 데 실패함</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>예기치 않은 오류 발생</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>메시지를 발송했으나 수신 확인이 안 됨(성공 불확실, 서버에는 암호화되어 보관되며 3일 이내 수신 가능)</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>내부 시스템 오류로 메시지 전송 실패</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>전화번호 오류</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>예기치 않은 오류 발생</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>메시지가 존재하지 않음</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>카카오 통신 실패</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>메시지가 비어 있음</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>메시지 길이 제한 오류</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>템플릿을 찾을 수 없음</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>메시지 내용이 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>메시지를 전송할 수 없음<br>1. Android 기기 사용자의 경우 핸드폰 유심과 카카오톡 사용 번호가 다른 사람<br>2. 활성 사용자가 아닌 경우<br>활성 사용자란?<br> * 서버와 연결되어 있는 카카오톡 사용자<br> * 발송 당일 가입한 사용자를 제외한 최근 7일(168시간) 내에 카카오톡을 사용한 사용자<br>3. 제재 사용자 등<br></td>
	</tr>
    <tr>
		<td>3019</td>
		<td>메시지를 발송할 수 없음<br>카카오톡 유저가 아님</td>
	</tr>
    <tr>
		<td>3020</td>
		<td>메시지를 발송할 수 없음<br>알림톡 수신 차단</td>
	</tr>
    <tr>
		<td>3021</td>
		<td>메시지를 발송할 수 없음<br>카카오톡 최소 버전 미지원</td>
	</tr>
	<tr>
		<td>3022</td>
		<td>발송 가능한 시간이 아님(친구톡 메시지는 08시부터 20시 50분까지 발송 가능)</td>
	</tr>
	<tr>
		<td>3023</td>
		<td>메시지 문법 오류(JSON 형식 오류)</td>
	</tr>
	<tr>
		<td>3024</td>
		<td>메시지에 포함된 이미지를 전송할 수 없음(이미지주소 또는 링크가 올바르지 않거나 이미지가 규격에 맞지 않음)</td>
	</tr>
	<tr>
		<td>3025</td>
		<td>변수 글자 수 제한 초과</td>
	</tr>
	<tr>
		<td>3026</td>
		<td>상담/봇 전환 버튼 extra, event 글자수 제한 초과</td>
	</tr>
	<tr>
		<td>3027</td>
		<td>메시지 버튼/바로연결이 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3028</td>
		<td>메시지 강조 표기 타이틀이 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3029</td>
		<td>메시지 강조 표기 타이틀 길이 제한 초과(50자)</td>
	</tr>
	<tr>
		<td>3030</td>
		<td>메시지 타입과 템플릿 강조 타입이 일치하지 않음</td>
	</tr>
	<tr>
		<td>3031</td>
		<td>헤더가 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3032</td>
		<td>헤더 길이 제한 초과(16자)</td>
	</tr>
	<tr>
		<td>3033</td>
		<td>아이템 하이라이트가 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3034</td>
		<td>아이템 하이라이트 타이틀 길이 제한 초과(이미지 없는 경우 30자, 이미지 있는 경우 21자)</td>
	</tr>
	<tr>
		<td>3035</td>
		<td>아이템 하이라이트 디스크립션 길이 제한 초과(이미지 없는 경우 19자, 이미지 있는 경우 13자)</td>
	</tr>
	<tr>
		<td>3036</td>
		<td>아이템 리스트가 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3037</td>
		<td>아이템 리스트의 아이템의 디스크립션 길이 제한 초과(23자)</td>
	</tr>
	<tr>
		<td>3038</td>
		<td>아이템 요약정보가 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3039</td>
		<td>아이템 요약정보의 디스크립션 길이 제한 초과(14자)</td>
	</tr>
	<tr>
		<td>3040</td>
		<td>아이템 요약정보의 디스크립션에 허용되지 않은 문자 포함(통화기호/코드, 숫자, 콤마, 소수점, 공백을 제외한 문자 포함)</td>
	</tr>
	<tr>
		<td>3041</td>
		<td>와이드 아이템 리스트 갯수 최대 최소 갯수 불일치</td>
	</tr>
	<tr>
		<td>3042</td>
		<td>대표링크가 템플릿과 일치하지 않음</td>
	</tr>
	<tr>
		<td>3043</td>
		<td>이미지 변수 개수 템플릿 불일치</td>
	</tr>
	<tr>
		<td>3044</td>
		<td>쿠폰 변수 템플릿 불일치</td>
	</tr>
	<tr>
		<td>3045</td>
		<td>커머스 정보 변수 템플릿 불일치</td>
	</tr>
	<tr>
		<td>3046</td>
		<td>부가 정보 최대 길이 제한 오류</td>
	</tr>
	<tr>
		<td>3047</td>
		<td>커머스 정보 상품명 최대 길이 제한 오류</td>
	</tr>
	<tr>
		<td>3048</td>
		<td>유효하지 않은 그룹 태그 키 입력</td>
	</tr>
    <tr>
       <td>3050</td>
       <td>수신동의거부 스펙 (N타입) 미지원</td>
    </tr>
	<tr>
		<td>3051</td>
		<td>케러셀 아이템 리스트 갯수 최소, 최대 갯수 불일치</td>
	</tr>
	<tr>
		<td>3052</td>
		<td>케러셀 아이템 메시지 길이 초과</td>
	</tr>
	<tr>
		<td>3053</td>
		<td>캐러셀 템플릿 불일치</td>
	</tr>
    <tr>
       <td>3054</td>
       <td>캐러셀 버튼 템플릿 불일치</td>
    </tr>
    <tr>
       <td>3055</td>
       <td>캐러셀 쿠폰 템플릿 불일치</td>
    </tr>
	<tr>
		<td>3056</td>
		<td>와이드 아이템 리스트 타이틀 길이 제한 오류</td>
	</tr>
    <tr>
       <td>3057</td>
       <td>캐러셀 커머스 템플릿 불일치</td>
    </tr>
	<tr>
		<td>3058</td>
		<td>캐러셀 헤더 길이 제한 오류</td>
	</tr>
	<tr>
		<td>4000</td>
		<td>메시지 전송 결과를 찾을 수 없음</td>
	</tr>
	<tr>
		<td>4001</td>
		<td>알 수 없는 메시지 상태</td>
	</tr>
    <tr>
       <td>4100</td>
       <td>requestId 오류</td>
    </tr>
    <tr>
       <td>4101</td>
       <td>요청 날짜 오류</td>
    </tr>
    <tr>
       <td>4102</td>
       <td>Template 요청 오류</td>
    </tr>
    <tr>
       <td>4103</td>
       <td>유효한 허브파트너를 찾을 수 없음</td>
    </tr>
    <tr>
       <td>4104</td>
       <td>유효한 발신 프로필을 찾을 수 없음</td>
    </tr>
    <tr>
       <td>4110</td>
       <td>유효하지 않은 챗버블 타입 또는 메시지 타입 요청</td>
    </tr>
    <tr>
       <td>4120</td>
       <td>메시지 요청 페이로드 생성 오류</td>
    </tr>
    <tr>
       <td>4121</td>
       <td>메시지 발송 대상 오류</td>
    </tr>
    <tr>
       <td>4122</td>
       <td>메시지 결과 조회 오류</td>
    </tr>
    <tr>
       <td>4130</td>
       <td>requestId 패턴 오류</td>
    </tr>
    <tr>
       <td>4131</td>
       <td>중복 requestId</td>
    </tr>
    <tr>
       <td>4132</td>
       <td>템플릿 변수 불일치</td>
    </tr>
    <tr>
       <td>4133</td>
       <td>중단된 템플릿</td>
    </tr>
    <tr>
       <td>4134</td>
       <td>변경된 템플릿</td>
    </tr>
    <tr>
       <td>4137</td>
       <td>메시지 요청 실패</td>
    </tr>
    <tr>
       <td>4138</td>
       <td>브랜드 메시지 메시지 개수 제한</td>
    </tr>
    <tr>
       <td>4139</td>
       <td>메시지 요청 실패</td>
    </tr>
    <tr>
       <td>4140</td>
       <td>메시지 요청 실패</td>
    </tr>
    <tr>
       <td>4141</td>
       <td>메시지 요청 실패</td>
    </tr>
    <tr>
       <td>4142</td>
       <td>메시지 요청 실패</td>
    </tr>
    <tr>
       <td>4143</td>
       <td>만료된 요청</td>
    </tr>
    <tr>
       <td>4144</td>
       <td>본문 길이 제한 (30KB) 초과</td>
    </tr>
    <tr>
       <td>4156</td>
       <td>최대 발송수 초과</td>
    </tr>
    <tr>
       <td>4161</td>
       <td>처리중인 메시지</td>
    </tr>
    <tr>
		<td>9998</td>
		<td>시스템에 문제가 발생하여 담당자가 확인 중(현재 서비스 제공 중이 아님)</td>
	</tr>
    <tr>
		<td>9999</td>
		<td>시스템에 문제가 발생하여 담당자가 확인 중(시스템에 알 수 없는 오류 발생)</td>
	</tr>
</tbody>
</table>
