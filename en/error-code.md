## Notification > KakaoTalk Bizmessage > Error Codes

## API response code

| service| isSuccess| resultCode| resultMessage                                                                                                                                                                                                                                                                                                                                    |
|----------|----------|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Common| true| 0| Success                                                                                                                                                                                                                                                                                                                                          |
| Common| false| 4| Failed to verify parameter validation (refer to the resultMessage)                                                                                                                                                                                                                                                                               |
| Common| false| -1000| Invalid appkey                                                                                                                                                                                                                                                                                                                                   |
| Common| false| -1001| Invalid secretKey                                                                                                                                                                                                                                                                                                                                |
| Common| false| -1002| Invalid SMS appkey                                                                                                                                                                                                                                                                                                                               |
| Common| false| -1003| Invalid SMS outgoing number                                                                                                                                                                                                                                                                                                                      |
| Common| false| -1004| Outgoing profile already registered                                                                                                                                                                                                                                                                                                              |
| Common| false| -1005| If you request to send a message with the same X-NC-API-IDEMPOTENCY-KEY value for 10 minutes                                                                                                                                                                                                                                                     |
| Common| false| -1008| Failed to register outgoing profile token                                                                                                                                                                                                                                                                                                        |
| Common| false| -1010| If no outgoing profile group exists                                                                                                                                                                                                                                                                                                              |
| Common| false| -1013| If outgoing profile group already exists                                                                                                                                                                                                                                                                                                         |
| Common| false| -1014| If outgoing profile is not activated                                                                                                                                                                                                                                                                                                             |
| Common| false| -1016| If there is no message                                                                                                                                                                                                                                                                                                                           |
| Common| false| -1017| Failure to send due to exceeding daily sending volume                                                                                                                                                                                                                                                                                            |
| Common| false| -1018| If you are already registered in the outgoing profile group                                                                                                                                                                                                                                                                                      |
| Common| false| -1019| Invalid fallback 080 opt-out number                                                                                                                                                                                                                                                                                                              |
| Common| false| -1022| If the outgoing profile is a outgoing profile that is not registered to a group                                                                                                                                                                                                                                                                  |
| Common| false| -1023| If the outgoing profile exceeds the maximum of 10 groups                                                                                                                                                                                                                                                                                         |
| Common| false| -1024| If the outgoing profile sends a message to a group                                                                                                                                                                                                                                                                                               |
| Common| false| -1025| When attempting to delete a outgoing profile group using the Delete Outgoing Profile API                                                                                                                                                                                                                                                         |
| Common| false| -1026| When the outgoing profile group has more than 5,000 members                                                                                                                                                                                                                                                                                      |
| Common| false| -1027| When the outgoing profile is blocked                                                                                                                                                                                                                                                                                                             |
| Common| false| -1028| When template variables are substituted, if the difference exceeds 14 characters (first-time user limit)                                                                                                                                                                                                                                         |
| Common| false| -1029| If no more member can be added to the group profile (first-time user limit)                                                                                                                                                                                                                                                                      |
| Common| false| -1030| If you have not verified your identity                                                                                                                                                                                                                                                                                                           |
| Common| false| -1031| If all recipients requested to send fail to send                                                                                                                                                                                                                                                                                                 |
| Common| false| -2000| If a specific field value exceeds the maximum length                                                                                                                                                                                                                                                                                             |
| Common| false| -2001| If a specific field value is blank                                                                                                                                                                                                                                                                                                               |
| Common| false| -2002| If a specific field value is NULL                                                                                                                                                                                                                                                                                                                |
| Common| false| -2003| If a specific field value is less than the minimum length                                                                                                                                                                                                                                                                                        |
| Common| false| -2004| If a specific field value is more than the maximum length                                                                                                                                                                                                                                                                                        |
| Common| false| -2005| If a specific field value is not within a limited range                                                                                                                                                                                                                                                                                          |
| Common| false| -2017| Non-existent sender profile                                                                                                                                                                                                                                                                                                                      |
| Common| false| -2018| Invalid button parameter                                                                                                                                                                                                                                                                                                                         |
| Common| false| -2019| Failed since because the template body exceeds 1,300 characters                                                                                                                                                                                                                                                                                  |
| Common| false| -2023| If the body of a FriendTalk message exceeds 400 characters (with an image attached)                                                                                                                                                                                                                                                              |
| Common| false| -2024| If the body of a FriendTalk message exceeds 1,000 characters                                                                                                                                                                                                                                                                                     |
| Common| false| -2025| If the scheduled date is in the past                                                                                                                                                                                                                                                                                                             |
| Common| false| -2026| If the scheduled date is more than 90 days in advance (up to 90 days in advance)                                                                                                                                                                                                                                                                 |
| Common| false| -2027| Parsing error due to different date formats                                                                                                                                                                                                                                                                                                      |
| Common| false| -2028| Invalid request ID                                                                                                                                                                                                                                                                                                                               |
| Common| false| -2029| If there is no message requested or there is no message that can be canceled                                                                                                                                                                                                                                                                     |
| Common| false| -2030| When sending a wide image, if the maximum body length exceeds 76 characters                                                                                                                                                                                                                                                                      |
| Common| false| -2031| When sending a wide image, if the maximum button number exceeds 2 characters                                                                                                                                                                                                                                                                     |
| Common| false| -2032| If the template title exceeds 50 characters                                                                                                                                                                                                                                                                                                      |
| Common| false| -2033| If the template item parameter is invalid                                                                                                                                                                                                                                                                                                        |
| Common| false| -2034| If the template item highlight parameter is invalid                                                                                                                                                                                                                                                                                              |
| Common| false| -2035| If the template header exceeds 16 characters                                                                                                                                                                                                                                                                                                     |
| Common| false| -2036| If TemplateRepresentLink parameter is invalid                                                                                                                                                                                                                                                                                                    |
| Common| false| -2037| When registering a template including an item list, if the number of characters in the message body exceeds 1,000                                                                                                                                                                                                                                |
| Common| false| -2500| If you can't find your mass delivery request                                                                                                                                                                                                                                                                                                     |
| Common| false| -2501| If there is no message requested or there is no message that can be canceled                                                                                                                                                                                                                                                                     |
| Common| false| -2502| If the sending failure setting is not set and a fallback is requested,                                                                                                                                                                                                                                                                           |
| Common| false| -2504| Invalid quick reply                                                                                                                                                                                                                                                                                                                              |
| Common| false| -2505| When sending with quick reply, if the maximum button number exceeds 2 characters                                                                                                                                                                                                                                                                 |
| Common| false| -3000| If the template is of the Web Link (WL) button type, the linkMo field is required.                                                                                                                                                                                                                                                               |
| Common| false| -3001| Template code or template name that already exists                                                                                                                                                                                                                                                                                               |
| Common| false| -3002| If the required request body content cannot be read                                                                                                                                                                                                                                                                                              |
| Common| false| -3003| Non-existent template                                                                                                                                                                                                                                                                                                                            |
| Common| false| -3004| Error in template parameter to be sent                                                                                                                                                                                                                                                                                                           |
| Common| false| -3005| Template status error (when requesting sending before approval)                                                                                                                                                                                                                                                                                  |
| Common| false| -3006| Button URL must include http:// or https://.                                                                                                                                                                                                                                                                                                     |
| Common| false| -3007| Only templates of the free button type can input button names and button URLs.                                                                                                                                                                                                                                                                   |
| Common| false| -3008| The delivery tracking button type does not allow you to enter a button URL.                                                                                                                                                                                                                                                                      |
| Common| false| -3009| Button name does not exists.                                                                                                                                                                                                                                                                                                                     |
| Common| false| -3010| If it does not match the registered template body                                                                                                                                                                                                                                                                                                |
| Common| false| -3011| If it does not match the registered template button                                                                                                                                                                                                                                                                                              |
| Common| false| -3012| Template status that cannot be modified (approved/rejected status only)                                                                                                                                                                                                                                                                          |
| Common| false| -3013| A template being modified already exists                                                                                                                                                                                                                                                                                                         |
| Common| false| -3014| If the button type is invalid                                                                                                                                                                                                                                                                                                                    |
| Common| false| -3015| If you are a Plus Friend with the CBT function disabled                                                                                                                                                                                                                                                                                          |
| Common| false| -3016| The highlighting template is a required field for templateTitle and templateSubtitle                                                                                                                                                                                                                                                             |
| Common| false| -3017| templateSubtitle cannot use substitution variables                                                                                                                                                                                                                                                                                               |
| Common| false| -3018| The templateExtra field is a required field for additional information type templates                                                                                                                                                                                                                                                            |
| Common| false| -3020| Composite templates is a required field for templateExtra and templateAd                                                                                                                                                                                                                                                                         |
| Common| false| -3021| templateExtra cannot use substitution variables                                                                                                                                                                                                                                                                                                  |
| Common| false| -3024| The AC type button can only register channel-add type composite templates                                                                                                                                                                                                                                                                        |
| Common| false| -3025| When the AC type button can be used alone or in case it violates the constraint of being located at the top                                                                                                                                                                                                                                      |
| Common| false| -3026| The name of the AC type button can only be registered as "Add Channel"                                                                                                                                                                                                                                                                           |
| Common| false| -3027| When registering the template highlight type as default (NONE), the templateTitle and templateSubtitle fields cannot be registered                                                                                                                                                                                                               |
| Common| false| -3028| If the template message type is Basic (BA), the templateExtra field cannot be registered                                                                                                                                                                                                                                                         |
| Common| false| -3030| If the template message type is channel add-on (AD), the templateExtra field cannot be registered                                                                                                                                                                                                                                                |
| Common| false| -3032| If the template highlight type is image type (IMAGE), the templateImageName and templateImageUrl fields are required                                                                                                                                                                                                                             |
| Common| false| -3033| If the button or quick reply is not registered in the template                                                                                                                                                                                                                                                                                   |
| Common| false| -3034| If the template to be deleted has a history of being sent within 3 days                                                                                                                                                                                                                                                                          |
| Common| false| -3035| When registering a template highlight type item list (ITEM_LIST), at least one of the following fields must be included: templateHeader, templateImageName, templateImageUrl, templateItem, templateItemHighlight                                                                                                                                |
| Common| false| -3036| When registering a template highlight type as an item list type (ITEM_LIST), it cannot be registered as a security template                                                                                                                                                                                                                      |
| Common| false| -3037| The title of the templateItem list cannot have substitution variables                                                                                                                                                                                                                                                                            |
| Common| false| -3038| The title of the templateItem summary cannot have substitution variables                                                                                                                                                                                                                                                                         |
| Common| false| -3039| templateItem summary cannot exist without templateItem list                                                                                                                                                                                                                                                                                      |
| Common| false| -3040| The title of the item highlight can be up to 21 characters, and the description can be up to 13 characters                                                                                                                                                                                                                                       |
| Common| false| -3041| imageUrl must include http:// https:// protocol                                                                                                                                                                                                                                                                                                  |
| Common| false| -3042| If templateHeader does not match the template                                                                                                                                                                                                                                                                                                    |
| Common| false| -3043| If templateItem or templateItemHighlight does not match the template                                                                                                                                                                                                                                                                             |
| Common| false| -3044| When the business form (BF) type button can be used alone or in case it violates the constraint of being located at the top                                                                                                                                                                                                                      |
| Common| false| -3045| If the name of the Business Form (BF) type is not "Reservation in Talk", "Survey in Talk", or "Application in Talk", or if the bizFormKey is not registered                                                                                                                                                                                      |
| Common| false| -3046| If templateRepresentLink does not match the template                                                                                                                                                                                                                                                                                             |
| Common| false| -3047| If you try to apply a font style when registering a template (the font style can be applied at the time of sending)                                                                                                                                                                                                                              |
| Common| false| -3048| Template parameter cannot exceeds 1,000 characters.                                                                                                                                                                                                                                                                                              |
| Common| false| -3049| Template parameter does not match the template.                                                                                                                                                                                                                                                                                                  |
| Common| false| -3050| Template message types (AD/MI) must have AC-type buttons.                                                                                                                                                                                                                                                                                        |
| Common| false| -3100| Comments cannot be added when the template is in request/approval status.                                                                                                                                                                                                                                                                        |
| Common| false| -3101| If the name of quick reply does not exist                                                                                                                                                                                                                                                                                                        |
| Common| false| -3102| Quick reply cannot contain substitutes                                                                                                                                                                                                                                                                                                           |
| Common| false| -3103| If the button or quick reply is not in the correct format (not in JSON format or contains unescaped characters)                                                                                                                                                                                                                                  |
| Common| false| -3200| If the name is missing in the wide item list                                                                                                                                                                                                                                                                                                     |
| Common| false| -3201| If the image is missing in the wide item list                                                                                                                                                                                                                                                                                                    |
| Common| false| -3202| If the linkMo is missing in the wide item list                                                                                                                                                                                                                                                                                                   |
| Common| false| -3203| A wide item list requires 3-4 lists of different sizes and a header                                                                                                                                                                                                                                                                              |
| Common| false| -3204| If the carousel is missing a header                                                                                                                                                                                                                                                                                                              |
| Common| false| -3205| If the carousel is missing a message                                                                                                                                                                                                                                                                                                             |
| Common| false| -3206| If the carousel is missing an attachment                                                                                                                                                                                                                                                                                                         |
| Common| false| -3207| If the carousel is missing an image                                                                                                                                                                                                                                                                                                              |
| Common| false| -3208| For carousel, a list of 10 sizes is required                                                                                                                                                                                                                                                                                                     |
| Common| false| -3209| If the linkMo is missing in the tail of carousel                                                                                                                                                                                                                                                                                                 |
| Common| false| -3210| Coupons require a title and description                                                                                                                                                                                                                                                                                                          |
| Common| false| -3211| For FriendTalk messages of FriendTalk text/image type, the coupon description cannot exceed 12 characters, and for FW/FL type, it cannot exceed 18 characters                                                                                                                                                                                    |
| Common| false| -3212| If the title content of coupon is invalild                                                                                                                                                                                                                                                                                                       |
| Common| false| -3213| Coupons require a mobile link or channel-type iOS/Android link                                                                                                                                                                                                                                                                                   |
| Common| false| -3215| Wide item lists and carousels are only available for ad types                                                                                                                                                                                                                                                                                    |
| Common| false| -3216| The title of the first item in a wide item list cannot exceed 25 characters, and the titles of the 2nd to 4th items cannot exceed 30 characters.                                                                                                                                                                                                 |
| Common| false| -3217| For FriendTalk text/image types, the number of buttons cannot exceed 5. If a coupon is included, the number of buttons cannot exceed 4. For wide image/wide item list, the number of buttons cannot exceed 2. For premium video types, the number of buttons cannot exceed 1. For commerce types, the number of buttons must be between 1 and 2. |
| Common| false| -3218| Invalid FriendTalk videoUrl                                                                                                                                                                                                                                                                                                                      |
| Common| false| -3219| FriendTalk content exceeds the maximum length. For premium video type, the maximum content length is 76 characters                                                                                                                                                                                                                               |
| Common| false| -3220| FriendTalk header exceeds the maximum length. For premium video type, the maximum header length is 20 characters                                                                                                                                                                                                                                 |
| Common| false| -3221| Carousel feed type cannot use carousel intro                                                                                                                                                                                                                                                                                                     |
| Common| false| -3222| Carousel feed type cannot use additional information field                                                                                                                                                                                                                                                                                       |
| Common| false| -3223| Carousel feed type cannot use commerce                                                                                                                                                                                                                                                                                                           |
| Common| false| -3224| Carousel commerce type cannot use header and message field                                                                                                                                                                                                                                                                                       |
| Common| false| -3225| Invalid carousel button. For carousel feed type, the number of buttons cannot exceed 2. For carousel commerce type, the number of buttons must be 1 to 2                                                                                                                                                                                         |
| Common| false| -3226| If the discountPrice field exists in commerce, the discountRate or discountFixed field is required                                                                                                                                                                                                                                               |
| Common| false| -3297| M, N targeting cannot be set when sending free-form messages.                                                                                                                                                                                                                                                                                    |
| Common| false| -3298| The targeting information is not set.                                                                                                                                                                                                                                                                                                            |
| Common| false| -3299| Commerce variables must use the specified combinations: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], \['regularPrice', 'discountPrice', 'discountFixed']                                                                                                                                                                 |
| Common| false| -3300| Cannot find the opt-out number.                                                                                                                                                                                                                                                                                                                  |
| Common| false| -3301| Contain users who have opted out.                                                                                                                                                                                                                                                                                                                |
| Common| false| -3303| The required input value is missing.                                                                                                                                                                                                                                                                                                             |
| Common| false| -3304| The input value is out of a valid range. Please enter a value within the specified range.                                                                                                                                                                                                                                                        |
| Common| false| -3305| Certain fields are not available for the selected speech bubble type.                                                                                                                                                                                                                                                                            |
| Common| false| -3306| Commerce-type message must include buttons.                                                                                                                                                                                                                                                                                                      |
| Common| false| -3307| Required fields are missing depending on the button link type.                                                                                                                                                                                                                                                                                   |
| Common| false| -3308| Multiple required fields are missing depending on the button link type.                                                                                                                                                                                                                                                                          |
| Common| false| -3309| When using App Links (AL) in a template, you must enter at least two of schemeAndroid, schemeIos, or LinkMo.                                                                                                                                                                                                                                     |
| Common| false| -3310| Web link URL (linkMo/linkPc) must contain 'http://' or 'https://'.                                                                                                                                                                                                                                                                               |
| Common| false| -3311| If the message type is text or image, the Add Channel (AC) button must be the first button.                                                                                                                                                                                                                                                      |
| Common| false| -3312| If the message type is not text or image, the Add Channel (AC) button must be the last button.                                                                                                                                                                                                                                                   |
| Common| false| -3313| Invalid coupon URL information. When using a basic coupon, the linkMo field is required, and when using a channel coupon URL, the schemeAndroid or schemeIos field is required.                                                                                                                                                                  |
| Common| false| -3314| Required fields are missing when entering coupon information.                                                                                                                                                                                                                                                                                    |
| Common| false| -3315| The coupon title is invalid.                                                                                                                                                                                                                                                                                                                     |
| Common| false| -3316| Required fields are missing when entering commerce information.                                                                                                                                                                                                                                                                                  |
| Common| false| -3317| Required fields are missing when entering video information.                                                                                                                                                                                                                                                                                     |
| Common| false| -3318| The video URL is in invalid format.                                                                                                                                                                                                                                                                                                              |
| Common| false| -3319| Required fields are missing when entering image information.                                                                                                                                                                                                                                                                                     |
| Common| false| -3320| The number of carousel items is out of the allowed range (min/max).                                                                                                                                                                                                                                                                              |
| Common| false| -3321| When entering a link in the carousel header, the mobile link (linkMo) is required.                                                                                                                                                                                                                                                               |
| Common| false| -3322| Required fields are missing when entering carousel header information.                                                                                                                                                                                                                                                                           |
| Common| false| -3323| Required fields are missing when entering carousel footer information.                                                                                                                                                                                                                                                                           |
| Common| false| -3324| Invalid template variable value.                                                                                                                                                                                                                                                                                                                 |
| Common| false| -3325| Required fields are missing when entering item (product) information.                                                                                                                                                                                                                                                                            |
| Common| false| -3326| Image URL must contain 'http://' or 'https://'.                                                                                                                                                                                                                                                                                                  |
| Common| false| -3327| Image link URL (linkMo/linkPc) must contain 'http://' or 'https://'.                                                                                                                                                                                                                                                                             |
| Common| false| -3328| This button type is not allowed.                                                                                                                                                                                                                                                                                                                 |
| Common| false| -3329| This app key does not have permission to use the FriendTalk upgrade API.                                                                                                                                                                                                                                                                         |
| Common| false| -3330| When arranging vertical buttons without an Add Channel button, the Bot Transfer (BT) button must be first.                                                                                                                                                                                                                                       |
| Common| false| -3331| This button name is not allowed.                                                                                                                                                                                                                                                                                                                 |
| Common| false| -3332| Required fields are missing when entering carousel commerce item information.                                                                                                                                                                                                                                                                    |
| Common| false| -3333| The carousel commerce item contains the information that is not allowed.                                                                                                                                                                                                                                                                         |
| Common| false| -3334| Required fields are missing when entering carousel item information.                                                                                                                                                                                                                                                                             |
| Common| false| -3335| The carousel item contains the information that is not allowed.                                                                                                                                                                                                                                                                                  |
| Common| false| -3336| When arranging vertical buttons with an Add Channel button, the Bot Transfer (BT) button must be second.                                                                                                                                                                                                                                         |
| Common| false| -3337| When arranging horizontal buttons without an Add Channel button, the Bot Transfer (BT) button must be on the right (second).                                                                                                                                                                                                                     |
| Common| false| -3338| When arranging horizontal buttons with an Add Channel button, the Bot Transfer (BT) button must be on the left (first).                                                                                                                                                                                                                          |
| Common| false| -3339| When entering the 080 verification number, the 080 opt-out number is a required value.                                                                                                                                                                                                                                                           |
| Common| false| -3340| Invalid 080 opt-out number                                                                                                                                                                                                                                                                                                                       |
| Common| false| -3341| Invalid 080 verification number                                                                                                                                                                                                                                                                                                                  |
| Common| false| -3342| Failed to upload marketing consent evidence                                                                                                                                                                                                                                                                                                      |
| Common| false| -3343| Failed to apply for M/N-type use                                                                                                                                                                                                                                                                                                                 |
| Common| false| -3344| The number of image change parameters does not match the number of template images                                                                                                                                                                                                                                                               |
| Common| false| -3345| Invalid 080 opt-out information                                                                                                                                                                                                                                                                                                                  |
| Common| false| -4003| The search range exceeds one month                                                                                                                                                                                                                                                                                                               |
| Common| false| -4004| Non-existent appkey                                                                                                                                                                                                                                                                                                                              |
| Common| false| -4005| App key in end of use status                                                                                                                                                                                                                                                                                                                     |
| Common| false| -4007| File size exceeded                                                                                                                                                                                                                                                                                                                               |
| Common| false| -4009| Invalid file extension                                                                                                                                                                                                                                                                                                                           |
| Common| false| -4015| Invalid request ID (requestId)                                                                                                                                                                                                                                                                                                                   |
| Common| false| -4025| When uploading a template, if the number exceeds 20                                                                                                                                                                                                                                                                                              |
| Common| false| -4026| If the header of the template upload file is invalid                                                                                                                                                                                                                                                                                             |
| Common| false| -4027| If you are trying to switch to channel addition mode with the button length at its maximum                                                                                                                                                                                                                                                       |
| Common| false| -4028| If you are trying to convert a channel to an unapproved template                                                                                                                                                                                                                                                                                 |
| Common| false| -4029| If you have not activated your consent to receive outgoing profile marketing messages                                                                                                                                                                                                                                                            |
| Common| false| -4101| Invalid statistics query parameter                                                                                                                                                                                                                                                                                                               |
| Common| false| -4103| When searching, if there is no sending request start time/sending request end time value                                                                                                                                                                                                                                                         |
| Common| false| -4200| Invalid fallback message                                                                                                                                                                                                                                                                                                                         |
| Common| false| -5000| Invalid incoming number                                                                                                                                                                                                                                                                                                                          |
| Common| false| -7000| Vendor request API failed                                                                                                                                                                                                                                                                                                                        |
| Common| false| -8000| If there is no image sequence (imageSeq)                                                                                                                                                                                                                                                                                                         |
| Common| false| -8001| If the image file is not normal                                                                                                                                                                                                                                                                                                                  |
| Common| false| -8002| If the image cannot be found For the carousel feed type, you must use a carousel-type image. For the carousel commerce type, you must use a carousel commerce type image. For the commerce type, you must use an image type.                                                                                                                     |
| Common| false| -8003| If image deletion fails                                                                                                                                                                                                                                                                                                                          |
| Common| false| -8004| If the createUser field exceeds the maximum of 100 characters                                                                                                                                                                                                                                                                                    |
| Common| false| -8005| If there are no Plus Friends registered in the project when uploading an image                                                                                                                                                                                                                                                                   |
| Common| false| -8006| When sending an verification message, if there is no verification text in the template content                                                                                                                                                                                                                                                   |
| Common| false| -8008| If the message content includes forbidden word                                                                                                                                                                                                                                                                                                   |
| Common| false| -8010| Failed to upload the image file due to issues such as network                                                                                                                                                                                                                                                                                    |
| Common| false| -8011| Non-existent image type                                                                                                                                                                                                                                                                                                                          |
| Common| false| -9992| When calling a faded-out friendtalk API                                                                                                                                                                                                                                                                                                 |
| Common| false| -9995| When calling a faded-out version of the API                                                                                                                                                                                                                                                                                                      |
| Common| false| -9996| If content-type is not application/json                                                                                                                                                                                                                                                                                                          |
| Common| false| -9997| Failed due to an inappropriate request                                                                                                                                                                                                                                                                                                           |
| Common| false| -9998| Non-existent API                                                                                                                                                                                                                                                                                                                                 |
| Common| false| -9999| System error                                                                                                                                                                                                                                                                                                                                     |

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
		<td>1016</td>
		<td>Business license number mismatch</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>Phone number or app user id is invalid, or request not entered.</td>
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
		<td>1027</td>
		<td>Message sending failure due to channel message sanction status</td>
	</tr>
    <tr>
       <td>1028</td>
       <td>The targeting option unavailable</td>
    </tr>
	<tr>
		<td>1030</td>
		<td>Invalid parameter request</td>
	</tr>
    <tr>
       <td>1033</td>
       <td>The template message type and chat\_bubble\_type paramater mismatch</td>
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
		<td>2005</td>
		<td>Failed to read image information due to an internal system error in KakaoTalk.</td>
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
		<td>3016</td>
		<td>The message content and template mismatch</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Unable to send messages<br>1. KakaoTalk user who has withdrawn <br>2. User who has never been subscribed to KakaoTalk <br>3. Blocked user from AlimTalk messages <br>4. Android users who use different "KakaoTalk numbers from USIM on device" <br>5. Deactivated users(for push) <br>6. User of the minimum KakaoTalk version or unsupported device, or punished user </td>
	</tr>
    <tr>
		<td>3019</td>
		<td>Cannot send a message<br>Not a KakaoTalk user</td>
	</tr>
    <tr>
		<td>3020</td>
		<td>Cannot send a message<br>AlimTalk reception blocked</td>
	</tr>
    <tr>
		<td>3021</td>
		<td>Cannot send a message<br>KakaoTalk minimum version not supported</td>
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
		<td>3035</td>
		<td>Item highlight description length limit exceeded (19 characters without image, 13 characters with image)</td>
	</tr>
	<tr>
		<td>3036</td>
		<td>Item list and template mismatch</td>
	</tr>
	<tr>
		<td>3037</td>
		<td>The item description length in the item list exceeds the limit (23 characters).</td>
	</tr>
	<tr>
		<td>3038</td>
		<td>Item summary information mismatches the template</td>
	</tr>
	<tr>
		<td>3039</td>
		<td>Description length limit for item summary information exceeded (14 characters)</td>
	</tr>
	<tr>
		<td>3040</td>
		<td>The description of the item summary information contains characters that are not allowed (characters other than currency symbols/codes, numbers, commas, decimal points, and spaces) </td>
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
       <td>Opt out spec. (N type) unsupported</td>
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
		<td>3053</td>
		<td>Carousel template mismatch</td>
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
