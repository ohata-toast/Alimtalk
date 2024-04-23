## Notification > KakaoTalk Bizmessage > FriendTalk > Overview

FriendTalk enables to send advertisement messages to KakaoTalk users who become Plus Friend based on mobile phone numbers.  
RESTful API is provided for easy integration.

## Benefits
* Send various promotional messages, including advertising messages, to users who are friends with you.
* Send text and images with attachments at a lower cost than SMS.
* If you attach an image, you can enter up to 400 characters of text. Otherwise, you can enter up to 1,000 characters.
* Send a template with no set content and no registration, just free text.
* If FriendTalk fails, you can send a text message instead.
* According to the Information and Communication Network Act, sending advertising information is restricted from 20:50 to 08:00 the following day.

|Category	|Description|
|-- |-- |
|Text	|Up to 1,000 characters of text + up to 5 link buttons|
|Image	|Up to 400 characters of text + 1 image + up to 5 link buttons|
|Wide image	|Up to 76 characters of text + 1 wide image + up to 2 link buttons|
|Wide item list|	Text + item list image + up to 2 link buttons (horizontally aligned)<br><li>Requires a list of maximum 4 items / minimum 3 items.</li><li>Text phrases are limited to 25 characters for the first item and 30 characters for items 2-4.</li><li>You can only send ads.</li>|
|Carousel feed|	Per 1 carousel: title (header) + text copy + link buttons (2/arranged horizontally) + send image for carousel<li>Requires a list of maximum 10 / minimum 2 carousel.</li><li>The title (header) is limited to 20 characters and the text copy to 180 characters.</li><li>Each carousel can have a maximum of two buttons and is sent horizontally aligned.</li><li>You can only send ads.</li>

## Main Features
* RESTful APIs are provided to Send/Query Messages or Manage Images.
* Console allows sending/querying messages, managing images, and queries statistics of sent messages.    
