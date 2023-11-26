## Notification > KakaoTalk Bizmessage > Error Codes

## API Response Codes
| service | isSuccess | resultCode | resultMessage                                                |
| ------- | --------- | ---------- | ------------------------------------------------------------ |
| Common  | true      | 0          | Successful                                                   |
| Common  | false     | 4          | Parameter validation failed(see resultMessage)              |
| Common  | false     | -1000      | Invalid appkey                                               |
| Common  | false     | -1001      | Invalid secret key                                           |
| Common  | false     | -1002      | Invalid SMS appkey                                           |
| Common  | false     | -1003      | Invalid SMS sender number                                    |
| Common  | false     | -1004      | Already registered Plus Friend                               |
| Common  | false     | -1008      | Registration failed for Plus Friend token                    |
| Common  | false     | -1010      | 발신프로필 그룹이 존재하지 않을 경우                                  |
| Common  | false     | -1013      | 이미 존재하는 발신프로필 그룹일 경우                                  |
| Common  | false     | -1014      | 활성화 되지 않은 발신프로필일 경우                                    |
| Common  | false     | -1016      | Message is not found                                         |
| Common  | false     | -1017      | Sending in excess of daily volume failed                     |
| Common  | false     | -1018      | 이미 발신프로필 그룹에 등록된 경우                                   |
| Common  | false     | -1019      | 유효하지 않은 대체 발송 080수신거부 번호                              |
| Common  | false     | -1022      | 발신프로필이 그룹에 등록되지 않은 발신프로필일 경우                       |
| Common  | false     | -1023      | 발신프로필이 그룹이 최대 10개가 초과한 경우                            |
| Common  | false     | -1024      | 발신프로필이 그룹으로 메시지를 발송할 경우                              |
| Common  | false     | -1025      | 발신프로필 삭제 API로 발신프로필 그룹 삭제 시도한 경우                   |
| Common  | false     | -1026      | 발신프로필 그룹의 멤버가 5000개가 초과한 경우                          |
| Common  | false     | -1027      | 발신프로필이 차단 상태일 경우                                       |
| Common  | false     | -1028      | 템플릿 변수가 치환될 때 그 차이가 14개 글자를 초과하는 부분이 있는 경우(최초 사용자 제한)        |
| Common  | false     | -1029      | 그룹 프로필에 멤버로 추가할 수 없는 경우(최초 사용자 제한)                      |
| Common  | false     | -1030      | 본인인증을 받지 않은 경우                                          |
| Common  | false     | -2000      | 특정 필드값이 최대 길이 초과할 경우                                   |
| Common  | false     | -2001      | 특정 필드값이 필수 값이 공백인 경우                                   |
| Common  | false     | -2002      | 특정 필드값이 NULL일 경우                                         |
| Common  | false     | -2003      | 특정 필드값이 최소 길이보다 작을 경우                                  |
| Common  | false     | -2017      | Plus Friend does not exist                                   |
| Common  | false     | -2018      | Invalid button parameter                                     |
| Common  | false     | -2019      | Failed due to template body with above 1,000 characters      |
| Common  | false     | -2022      | ImageLink is missing for attached image                      |
| Common  | false     | -2023      | FriendTalk body message exceeding 400 characters(image attached) |
| Common  | false     | -2024      | FriendTalk body message exceeding 1,000 characters           |
| Common  | false     | -2025      | Scheduled date and time is from the past                     |
| Common  | false     | -2026      | Scheduled date and time is 90 days after(available up to 90 days) |
| Common  | false     | -2027      | Error in parsing due to different date format                |
| Common  | false     | -2028      | Invalid request ID                                           |
| Common  | false     | -2029      | Requested message is missing or there is no message to cancel |
| Common  | false     | -2030      | 와이드 이미지 발송 시, 최대 본문 길이 76자 초과한 경우                    |
| Common  | false     | -2031      | 와이드 이미지 발송 시, 최대 버튼 개수 2개 초과한 경우                    |
| Common  | false     | -2032      | 템플릿 제목이 50자를 초과한 경우                                     |
| Common  | false     | -2033      | 템플릿 아이템 파라미터가 유효하지 않은 경우                             |
| Common  | false     | -2034      | 템플릿 아이템 하이라이트 파라미터가 유효하지 않은 경우                     |
| Common  | false     | -2035      | 템플릿 헤더가 16자를 초과한 경우                                     |
| Common  | false     | -2036      | Requested message is missing or there is no message to cancel |
| Common  | false     | -2500      | 대량 발송 요청을 찾을 수 없는 경우                                   |
| Common  | false     | -2501      | 요청한 메시지가 없거나, 취소할 수 있는 메시지가 없는 경우                  |
| Common  | false     | -2502      | Alternative delivery requested when failed delivery is not configured |
| Common  | false     | -2504      | 유효하지 않은 바로연결                                             |
| Common  | false     | -2505      | 바로연결 포함 발송 시, 최대 버튼 개수 2개를 초과한 경우                   |
| Common  | false     | -2999      | Request of all recipients requesting delivery failed         |
| Common  | false     | -3000      | Button name/mobile link(linkMo) is required for free button-type templates. |
| Common  | false     | -3001      | Template code or template name already exists                |
| Common  | false     | -3002      | Unable to read request body which is required                |
| Common  | false     | -3003      | Template does not exist                                      |
| Common  | false     | -3004      | Error in template parameter to send                          |
| Common  | false     | -3005      | Error in template status(when requested for delivery before approval) |
| Common  | false     | -3006      | Button URL must include http:// or https://.                 |
| Common  | false     | -3007      | Only free button-type templates allow the input of button name and button URL. |
| Common  | false     | -3008      | Query delivery button type does not allow the input of button URL. |
| Common  | false     | -3009      | Button name does not exist.                                  |
| Common  | false     | -3010      | Template body does not match.                                |
| Common  | false     | -3011      | Template button does not match.                              |
| Common  | false     | -3012      | Unavailable to modify template(either approved or rejected)           |
| Common  | false     | -3013      | Template under modification exists                           |
| Common  | false     | -3014      | Invalid button type                         |
| Common  | false     | -3015      | Plus Friend with CBT deactivated                  |
| Common  | false     | -3016      | Requires templateTitle and templateSubtitle for Emphasized templates            |
| Common  | false     | -3017      | Unable to use replacement variable for templateSubtitle            |
| Common  | false     | -3018      | Requires templateExtra for Extra Information-type templates           |
| Common  | false     | -3020      | Requires templateExtra and templateAd for Mixed-purposes templates   |
| Common  | false     | -3021      | Unable to use replacement variable for templateExtra               |
| Common  | false     | -3024      | CA-type button can be registration only for Ad-included or Mixed Purposes-type templates       |
| Common  | false     | -3025      | AC 타입 버튼은 단독으로 사용하거나, 가장 위에 위치해야하는 제약에 어긋난 경우       |
| Common  | false     | -3026      | AC 타입 버튼의 이름은 "채널 추가" 만 등록 가능                              |
| Common  | false     | -3027      | 템플릿 강조 표시 타입 기본(NONE)로 등록 시, templateTitle, templateSubtitle 필드를 등록할 수 없음       |
| Common  | false     | -3028      | 템플릿 메시지 유형 기본형(BA)일 경우, templateExtra 필드를 등록할 수 없음        |
| Common  | false     | -3030      | 템플릿 메시지 유형 채널 추가형(AD)일 경우, templateExtra 필드를 등록할 수 없음          |
| Common  | false     | -3031      | 그룹 템플릿은 템플릿 메시지 유형 채널추가형(AD), 복합형(MI)를 사용할 수 없음       |
| Common  | false     | -3032      | 템플릿 강조 표시 타입 이미지형(IMAGE)일 경우, templateImageName, templateImageUrl 필드 필수값       |
| Common  | false     | -3033      | 버튼 또는 바로연결이 템플릿에 등록되지 않았을 경우                                 |
| Common  | false     | -3034      | Only templates that have not been sent for 3 days can be deleted       |
| Common  | false     | -3035      | 템플릿 강조 표시 타입 아이템리스트형(ITEM_LIST)로 등록 시, templateHeader, templateImageName, templateImageUrl, templateItem, templateItemHighlight 필드 중 1개 이상 필수로 포함       |
| Common  | false     | -3036      | 템플릿 강조 표시 타입 아이템리스트형(ITEM_LIST)로 등록 시, 보안 템플릿으로 등록할 수 없음       |
| Common  | false     | -3037      | templateItem list의 title은 치환 변수를 가질 수 없음                         |
| Common  | false     | -3038      | templateItem summary의 title은 치환 변수를 가질 수 없음                      |
| Common  | false     | -3039      | templateItem summary은 templateItem list없이 존재할 수 없음                 |
| Common  | false     | -3040      | 아이템하이라이트의 title은 최대 21글자, description은 최대 13글자                |
| Common  | false     | -3041      | imageUrl은 http:// https:// 프로토콜을 반드시 포함해야 함                     |
| Common  | false     | -3042      | templateHeader이 템플릿과 일치하지 않는 경우                                 |
| Common  | false     | -3043      | templateItem 또는 templateItemHighlight이 템플릿과 일치하지 않는 경우          |
| Common  | false     | -3044      | 비지니스폼(BF) 타입 버튼은 단독으로 사용하거나, 가장 위에 위치해야하는 제약에 어긋난 경우            |
| Common  | false     | -3045      | 비지니스폼(BF) 타입의 name이 "톡에서 예약하기", "톡에서 설문하기", "톡에서 응모하기" 이 아닐 경우, 또는 등록되지 않은 bizFormKey일 경우       |
| Common  | false     | -3046      | templateRepresentLink이 템플릿과 일치하지 않는 경우                  |
| Common  | false     | -4003      | Query range exceeding a month                                |
| Common  | false     | -4004      | Appkey does not exist                                        |
| Common  | false     | -4005      | Appkey closed for service                                    |
| Common  | false     | -4007      | File size exceeding 500KB                                    |
| Common  | false     | -4009      | 유효하지 않은 파일 확장자                                          |
| Common  | false     | -4015      | Invalid request ID(requestId)                               |
| Common  | false     | -4101      | Invalid query parameter for statistics                       |
| Common  | false     | -4103      | Start/End time value of delivery request is unavailable for queries |
| Common  | false     | -4200      | Invalid alternative delivery message                                       |
| Common  | false     | -5000      | Invalid recipient number                                     |
| Common | false     | -6000      | 고정형(F) 내용 유형은 변수를 포함할 수 없음            |
| Common | false     | -6001      | 변수형(V) 내용 유형은 반드시 변수를 포함해야 함         |
| Common | false     | -6002      | 변수는 총 20개 이상 포함할 수 없음                   |
| Common | false     | -6003      | 변수는 영문/숫자/특수문자('-', '')만 사용할 수 있으며 20자 이내로만 가능 |
| Common | false     | -6004      | 와이드형(W) 메시지 타입은 본문 76자 이내, 버튼 2개까지 가능 |
| Common | false     | -6005      | 채널 추가(AC) 타입 버튼은 이미지형(I) 메시지 타입일 경우, 첫 번째 순서에 위치해야 합니다.<br>와이드형(W) 메시지 타입일 경우, 마지막에 위치해야 합니다.|
| Common | false     | -6006      | 비지니스폼(BF) 타입 버튼은 bizFormId 값이 반드시 존재해야 합니다.<br>이미지형(I) 메시지 타입일 경우, 첫 번째 순서에 위치해야 합니다.<br>와이드형(W) 메시지 타입일 경우, 마지막에 위치해야 합니다.<br>이미지형(I) 메시지 타입이면서 채널 추가(AC) 타입 버튼과 같이 사용할 경우, 두 번째에 위치해야 합니다.<br>와이드형(W) 메시지 타입이면서 채널 추가(AC) 타입 버튼과 같이 사용할 경우, 첫 번째에 위치해야 합니다.|
| Common  | false     | -6007      | 080 수신거부 번호 유효성 검사                                      |
| Common  | false     | -7000      | Vendor request API failed                                    |
| Common  | false     | -8000      | Image sequence(imageSeq) is missing                         |
| Common  | false     | -8001      | Image file is not normal                                     |
| Common  | false     | -8002      | No image available corresponding to image sequence           |
| Common  | false     | -8003      | Deleting image failed                                        |
| Common  | false     | -8004      | createUser 필드 최대 100자를 초과한 경우                            |
| Common  | false     | -8005      | No Plus Friend is registered in project to upload images     |
| Common  | false     | -8006      | Authentication message not included in Template, when sending an authentication message       |
| Common  | false     | -8008      | Banned word was included in message content , when sending an message       |
| Common  | false     | -9995      | Called API of a faded version                                |
| Common  | false     | -9996      | Content-type is not application/json                         |
| Common  | false     | -9998      | API does not exist                                           |
| Common  | false     | -9999      | Error in system                                              |


## AlimTalk/FriendTalk Delivery Result Codes

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>Code Value</th>
		<th>Significance</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>1000</td>
		<td>Successful</td>
	</tr>
  <tr>
		<td>1001</td>
		<td>Server Busy(Queue full for RS internal saving)</td>
	</tr>
  <tr>
		<td>1002</td>
		<td>Format error of recipient number</td>
	</tr>
  <tr>
		<td>1003</td>
		<td>Invalid sender profile key </td>
	</tr>
  <tr>
		<td>1004</td>
		<td>Cannot find name from request body(JSON)</td>
	</tr>
  <tr>
		<td>1006</td>
		<td>Deleted sender profile(contact Customer Center)</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>Blocked sender profile(contact Customer Center)</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>Contract information is not found(contact Customer Center) </td>
	</tr>
  <tr>
		<td>1012</td>
		<td>Invalid user key request format</td>
	</tr>
  <tr>
		<td>1013</td>
		<td>Invalid app connection</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>Invalid app user ID request </td>
	</tr>
  <tr>
		<td>1021</td>
		<td>Blocked KakaoTalk channel </td>
	</tr>
	<tr>
		<td>1022</td>
		<td>Blocked KakaoTalk channel </td>
	</tr>
	<tr>
		<td>1023</td>
		<td>Deleted KakaoTalk channel </td>
	</tr>
	<tr>
		<td>1024</td>
		<td>KakaoTalk channel waiting for deletion </td>
	</tr>
	<tr>
		<td>1025</td>
		<td>KakaoTalk channel blocked for message </td>
	</tr>
	<tr>
		<td>1030</td>
		<td>Invalid parameter request</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>Delivery time exceeded</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>Unable to send messages(due to unexpected error)</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>Error occurred when template consistency is checked(internal error occurred)</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Unexpected error occurred</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Message is delivered but receipt is not confirmed(Uncertain if successful; Encrypted and saved in server and available for sending within 3 days)</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Message delivery failed due to internal system error </td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Phone number error </td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Format error in message </td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Unexpected error occurred </td>
	</tr>
	<tr>
		<td>3011</td>
		<td>Message does not exist </td>
	</tr>
	<tr>
		<td>3012</td>
		<td>Communication failed with KakaoTalk </td>
	</tr>
	<tr>
		<td>3013</td>
		<td>Message is empty </td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Error of length restriction in message</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>msg_type error(neither 1008 nor 1009)</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Unable to send messages<br>1. KakaoTalk user who has withdrawn <br>2. User who has never been subscribed to KakaoTalk <br>3. Blocked user from AlimTalk messages <br>4. Android users who use different "KakaoTalk numbers from USIM on device" <br>5. Deactivated users(for push) <br>6. User of the minimum KakaoTalk version or unsupported device, or punished user </td>
	</tr>
	<tr>
		<td>3022</td>
		<td>Not available time(Friend Talk messages can be sent from 08:00 to 20:50)</td>
	</tr>
	<tr>
		<td>3023</td>
		<td>Grammatical error of message(error in JSON format)</td>
	</tr>
	<tr>
		<td>3024</td>
		<td>Invalid sender profile key </td>
	</tr>
	<tr>
		<td>3025</td>
		<td>Exceeded the limit of variable character count </td>
	</tr>
	<tr>
		<td>3026</td>
		<td>Error occurred during consistency checked between message and template </td>
	</tr>
	<tr>
		<td>3027</td>
		<td>Message Buttons/Direct connection does not match template</td>
	</tr>
	<tr>
		<td>3028</td>
		<td>Message highlighted title does not match template </td>
	</tr>
	<tr>
		<td>3029</td>
		<td>Exceeded limit of length in message highlighted title(50 characters)</td>
	</tr>
	<tr>
		<td>3030</td>
		<td>Redundant serial number of message </td>
	</tr>
	<tr>
		<td>3031</td>
		<td>Message is empty</td>
	</tr>
	<tr>
		<td>3032</td>
		<td>Error of length restriction in message(1000 characters including spaces)</td>
	</tr>
	<tr>
		<td>3033</td>
		<td>Template not found </td>
	</tr>
	<tr>
		<td>3034</td>
		<td>Message is not consistent with template </td>
	</tr>
	<tr>
		<td>3040</td>
		<td>아이템 요약정보의 디스크립션에 허용되지 않은 문자 포함(통화기호/코드, 숫자, 콤마, 소수점, 공백을 제외한 문자 포함) </td>
	</tr>
	<tr>
		<td>3041</td>
		<td>Name not found in the request body</td>
	</tr>
	<tr>
		<td>3042</td>
		<td>Sender profile not found </td>
	</tr>
	<tr>
		<td>3043</td>
		<td>Deleted sender profile </td>
	</tr>
	<tr>
		<td>3044</td>
		<td>Blocked sender profile </td>
	</tr>
	<tr>
		<td>3045</td>
		<td>Blocked Plus Friend </td>
	</tr>
	<tr>
		<td>3046</td>
		<td>Closed Plus Friend </td>
	</tr>
	<tr>
		<td>3047</td>
		<td>Deleted Plus Friend </td>
	</tr>
	<tr>
		<td>3048</td>
		<td>Contract information not found </td>
	</tr>
	<tr>
		<td>3049</td>
		<td>Message delivery failed due to internal system error </td>
	</tr>
	<tr>
		<td>3050</td>
		<td>Non-KakaoTalk user <br>
	    User opting to block AlimTalk of users who record no KakaoTalk service use within 72 hours <br>
	    Not a friend of FriendTalk <br></td>
	</tr>
	<tr>
		<td>3051</td>
		<td>Message undelivered </td>
	</tr>
	<tr>
		<td>3054</td>
                <td>Unavailable time to send messages</td>
	</tr>
  <tr>
		<td>3055</td>
		<td>Message group information not found </td>
	</tr>
  <tr>
		<td>3056</td>
		<td>Message delivery result not found </td>
	</tr>
  <tr>
		<td>3060</td>
		<td>Sent to user but not sure if received(Polling)</td>
	</tr>
	<tr>
		<td>4000</td>
		<td>Message delivery result is not found </td>
	</tr>
	<tr>
		<td>4001</td>
		<td>Unknown message status </td>
	</tr>
  <tr>
		<td>9998</td>
		<td>Under administrator's checkup for issue occurred in system(currently unavailable) </td>
	</tr>
  <tr>
		<td>9999</td>
		<td> Under administrator's checkup for error occurred in system(unknown error in system)</td>
	</tr>
	<tr>
		<td>E900</td>
		<td>Transfer key is not available</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>Recipient number is not available </td>
	</tr>
	<tr>
		<td>E903</td>
		<td>Title is not available </td>
	</tr>
	<tr>
		<td>E904</td>
		<td>Message is not available </td>
	</tr>
	<tr>
		<td>E905</td>
		<td>Reply number is not available</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>Message key is not available</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>Duplicate message</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>Blocked number at authenticated server </td>
	</tr>
	<tr>
		<td>E917</td>
		<td>Blocked number at customer database </td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>Message redelivery is prohibited during delivery restricted hours </td>
	</tr>
	<tr>
		<td>E920</td>
		<td>Message table includes a file group key for AlimTalk messages </td>
	</tr>
	<tr>
		<td>E999</td>
		<td>Other errors </td>
	</tr>
</tbody>
</table>

## 브랜드톡 발송 결과 코드

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>코드값</th>
		<th>의미</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>A0000</td>
		<td>성공</td>
	</tr>
  <tr>
		<td>A1001</td>
		<td>Request Body가 JSON형식이 아님</td>
	</tr>
  <tr>
		<td>A1003</td>
		<td>발신 프로필 키가 유효하지 않음</td>
	</tr>
  <tr>
		<td>A1004</td>
		<td>Request Body(JSON)에서 name을 찾을 수 없음</td>
	</tr>
	<tr>
		<td>A1006</td>
		<td>삭제된 발신프로필</td>
	</tr>
	<tr>
		<td>A1007</td>
		<td>차단 상태의 발신프로필</td>
	</tr>
	<tr>
		<td>A1021</td>
		<td>차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>A1022</td>
		<td>닫힘 상태의 카카오톡 채널</td>
	</tr>
  <tr>
		<td>A1023</td>
		<td>삭제된 카카오톡 채널</td>
	</tr>
  <tr>
		<td>A1024</td>
		<td>삭제대기 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>A1025</td>
		<td>메시지차단 상태의 카카오톡 채널</td>
	</tr>
	<tr>
		<td>A1030</td>
		<td>잘못된 파라메터 요청</td>
	</tr>
	<tr>
		<td>A1033</td>
		<td>템플릿 타입과 메시지타입 불일치</td>
	</tr>
	<tr>
		<td>A2040</td>
		<td>동보발송 요청이 진행중이라서 타겟팅 값을 사용할 수 없음</td>
	</tr>
	<tr>
		<td>A3000</td>
		<td>예기치 않은 오류 발생</td>
	</tr>
	<tr>
		<td>A3006</td>
		<td>내부 시스템 오류로 메시지 전송 실패</td>
	</tr>
	<tr>
		<td>A3008</td>
		<td>전화번호 오류</td>
	</tr>
	<tr>
		<td>A3010</td>
		<td>Json 파싱 오류</td>
	</tr>
	<tr>
		<td>A3011</td>
		<td>메시지가 존재하지 않음</td>
	</tr>
	<tr>
		<td>A3013</td>
		<td>메시지가 비어 있음</td>
	</tr>
	<tr>
		<td>A3014</td>
		<td>메시지 길이 제한 오류(템플릿별 제한 길이 또는 1000자 초과)</td>
	</tr>
	<tr>
		<td>A3015</td>
		<td>템플릿을 찾을 수 없음</td>
	</tr>
	<tr>
		<td>A3016</td>
		<td>메시지 내용이 변수와 일치하지 않음</td>
	</tr>
	<tr>
		<td>A3018</td>
		<td>메시지를 전송할 수 없음</td>
	</tr>
	<tr>
		<td>A3020</td>
		<td>메시지 확인 정보를 찾을 수 없음</td>
	</tr>
	<tr>
		<td>A3022</td>
		<td>메시지 발송 가능한 시간이 아님(브랜드톡/마케팅 메시지는 08시부터 20시까지 발송 가능)</td>
	</tr>
	<tr>
		<td>A3024</td>
		<td>메시지에 포함된 이미지를 전송할 수 없음</td>
	</tr>
	<tr>
		<td>A3027</td>
		<td>버튼이 변수와 일치하지 않음</td>
	</tr>
	<tr>
		<td>A3031</td>
		<td>텍스트 유형 불일치</td>
	</tr>
	<tr>
		<td>A4000</td>
		<td>메시지 전송 결과를 찾을 수 없음</td>
	</tr>
	<tr>
		<td>A4001</td>
		<td>알 수 없는 메시지 상태</td>
	</tr>
	<tr>
		<td>A9998</td>
		<td>시스템에 문제가 발생하여 담당자가 확인하고 있는 경우</td>
	</tr>
	<tr>
		<td>A9999</td>
		<td>시스템에 문제가 발생하여 담당자가 확인하고 있는 경우</td>
	</tr>
	<tr>
		<td>B7013</td>
		<td>발송 제약 시간(20:50~익일 08:00) 에러</td>
	</tr>
	<tr>
		<td>B7014</td>
		<td>메시지 유효 시간 초과 에러</td>
	</tr>
	<tr>
		<td>B7015</td>
		<td>메시지 길이 제한 오류</td>
	</tr>
	<tr>
		<td>B7017</td>
		<td>내부 시스템 오류로 메시지 전송 실패</td>
	</tr>
</tbody>
</table>
