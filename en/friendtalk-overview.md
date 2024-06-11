## Notification > KakaoTalk Bizmessage > FriendTalk > Overview

FriendTalk is a service that can send various messages, including advertising messages such as events and occasions, to customers with added friends on my Kakao channel based on their mobile phone number. 
It provides RESTful API for easy integration.

## Characteristics
* Send various promotional messages, including advertising messages, to users who are friends with you.
* Text and images can be attached and sent at a lower cost than LMS/MMS.
* Send a template with no set content and no registration, just free text.
* If FriendTalk fails, you can send a text message instead.
* According to the Information and Communication Network Act, sending advertising information is restricted from 20:50 to 08:00 the following day.

## Main Features
* It provides RESTful API for message sending, query, and image management.
* From the console, you can send messages, query them, manage images, and query the statistics of messages you sent.


## FriendTalk Delivery Supported Types

|Classification	|Description| Kakao Image Upload Specification |
|-- |-- | --|
|Text	|Up to 1,000 characters of text + up to 5 link buttons| Not available  |
|Image	|Up to 400 characters of text + 1 image + up to 5 link buttons| </li><li> Size Limit- Width 500px or more, Width: Length ratio 2:1 or more and 3:4 or less</li><li>File type and size: jpg, png / up to 5MB |
|Wide image	|Up to 76 characters of text + 1 wide image + up to 2 link buttons| </li><li> Size limit – width 800px, length 600px</li><li>File type and size: jpg, png / up to 5MB |
|Wide item list|	Text + item list image + up to 2 link buttons (horizontally aligned)<br><li>Requires a list of maximum 4 items / minimum 3 items.</li><li>Text phrases are limited to 25 characters for the first item and 30 characters for items 2-4.</li><li>You can only send ads.</li>| </li><li> Upload and use at least 3 images and up to 4 images according to the number of item lists.</li><li>Size limit - width 400px, length 400px ~ width 800px, length 400px</li><li>Check ratio and horizontal pixels X. Expose cropped to center reference for thumbnail size</li><li>File type and size: jpg, png / up to 5MB each file |
|Carousel feed|	Per 1 carousel: title (header) + text copy + link buttons (2/arranged horizontally) + send image for carousel<li>Requires a list of maximum 10 / minimum 2 carousel.</li><li>The title (header) is limited to 20 characters and the text copy to 180 characters.</li><li>Each carousel can have a maximum of two buttons and is sent horizontally aligned.</li><li>You can only send ads.</li> | </li><li>Upload and use at least 2 images and up to 10 images according to the number of carousel lists. </li><li>Size Limit- Width 500px or more, Width: Length ratio 2:1 or more and 3:4 or less</li><li>File type and size: jpg, png / up to 5MB each file |

![Figure 1](https://static.toastoven.net/prod_alimtalk/KTB_Image_1_friendtalk.png)

