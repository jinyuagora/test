# Events and Callbacks

Agora Chat supports HTTP callbacks (webhooks).
You can listen for event callbacks and add app logics accordingly. Once a registered callback is triggered by a specified type of event, the Agora Chat server sends an HTTP POST request to your app server, notifying you that the event occurs. The request body is a JSON string encoded in UTF-8 characters.

This page introduces the events and callbacks in Agora Chat.

## User login and logout events

When a user logs in to or logs out of the Agora Chat app, the Agora Chat server sends a callback to your app server.

### Log in to the app

When a user logs in to the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "callId":"XXXX#XXXXe393c568-5ae5-4a0e-8a2c-008b52b49eed",
    "reason":"login",
    "security":"XXXXae2eXXXXced298883a0cf06d41b9",
    "os":"ios",
    "ip":"************",
    "host":"*******",
    "appkey":"XXXX#XXXX",
    "user":"XXXX#XXXXtstXXXX/ios_XXXX01fd-b5a4-84d5-ebeb-bf10XXXX0442",
    "version":"3.8.9.1",
    "timestamp":1642585154644,
    "status":"online"
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `reason` | Object | The reason that triggers the callback. `login` indicates that a user logs in to the app. |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `os` | String | The operating system of the device. Valid values: `ios`, `android`, `linux`, `win`, and `other.` |
| `ip` | String | The IP address of the user. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `user` | String | The unique identifier of the user, in the format of `{appKey}/{OS}_{deviceId}`. |
| `version` | String | The version of the Agora Chat SDK. |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the login request, in milliseconds. |
| `status` | String | The current status of the user. `online` indicates that the user is online. |

### Log out of the app voluntarily

When a user logs out of the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "callId":"XXXX#XXXX25b54a81-1376-4669-bb3d-178339a8f11b",
    "reason":"logout",
    "security":"XXXXd77eXXXXf26801627fdaadca987e",
    "os":"ios",
    "ip":"223.71.97.198:4XXXX",
    "host":"********",
    "appkey":"XXXX#XXXX",
    "user":"XXXX#XXXXtstXXXX/ios_XXXX0737-db3a-d2b5-da18-b604XXXX195b",
    "version":"3.8.9.1",
    "timestamp":1642648914742,
    "status":"offline"
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `reason` | Object | The reason that triggers the callback. `logout` indicates that a user logs out of the app. |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `os` | String | The operating system of the device. Valid values: `ios`, `android`, `linux`, `win`, and `other.` |
| `ip` | String | The IP address of the user. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `user` | String | The unique identifier of the user, in the format of `{appKey}/{OS}_{deviceId}`. |
| `version` | String | The version of the Agora Chat SDK. |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the logout request, in milliseconds. |
| `status` | String | The current status of the user. `offline` indicates that the user is offline. |

### Log out of the app passively

When a user logs out of the Agora Chat app due to being kicked out by another device, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "callId":"XXXX#XXXX260ae3eb-ba31-4f01-9a62-8b3b05f3a16c",
    "reason":"replaced",
    "security":"XXXX00b1XXXX4fe76dbfdc664cbaa76b",
    "os":"ios","ip":"223.71.97.198:52709",
    "host":"msync@ebs-ali-beijing-msync40",
    "appkey":"XXXX#XXXX",
    "user":"XXXX#XXXXtst01XXXX/ios_XXXX01fd-b5a4-84d5-ebeb-bf10XXXX0442",
    "version":"3.8.9.1",
    "timestamp":1642648955563,
    "status":"offline"
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `reason` | Object | The reason that triggers the callback. `replaced` indicates that a user logs out of the app due to being kicked out by another device. |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `os` | String | The operating system of the device. Valid values: `ios`, `android`, `linux`, `win`, and `other.` |
| `ip` | String | The IP address of the user. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `user` | String | The unique identifier of the user, in the format of `{appKey}/{OS}_{deviceId}`. |
| `version` | String | The version of the Agora Chat SDK. |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the logout request, in milliseconds. |
| `status` | String | The current status of the user. `offline` indicates that the user is offline. |


## Message events

### Send a message

When a user sends a message within a one-to-one chat, chat group, or chat room in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "callId":"{appkey}_{uuid}",
    "eventType":"chat_offline",
    "timestamp":1600060847294,
    "chat_type":"groupchat", 
    "group_id":"1693XXXX238921545",
    "from":"user1",
    "to":"user2",
    "msg_id":"8924XXXX42322",
    "payload":{
    // The details of the callback message.
    },
    "securityVersion":"1.0.0",
    "security":"XXXX2c39XXXX9e7abc83958bcc3156d3"
}
```

| Field | Data Type | Description |
| -- | -- | -- |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `eventType` | String | The message type of the callback. <ul><li>`chat`: Uplink messages. The messages that are about to be sent by the Agora Chat server to end devices.</li><li>`chat_offline`: Offline messages. The messages that are not being sent by the Agora Chat server as the end user is offline.</li></ul> |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the message, in milliseconds. |
| `chat_type` | String | The type of chat. <ul><li>`chat`: One-to-one chats.</li><li>`groupchat`: Chat groups and chat rooms.</li></ul> |
| `group_id` | String | The ID of the chat group or chat room where the message resides. This field only exists if `chat_type` is set to `groupchat`. |
| `from` | String | The sender of the message. |
| `to`  | String | The recipient of the message. |
| `msg_id` | String | The ID of the message callback. This ID is the same as the `msg_id` when the end user sends the message. |
| `payload` | Object | The structure of the callback. This field varies according to the type of the sent message within an one-to-one chat, chat group, or chat room. See below for details. |
| `securityVersion` | String | This parameter is reserved for future use.  |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |

#### Send a text message

When a user sends a text message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "msg":"rr",
            "type":"txt"
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the message callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| :------------ | :------- | :---------------------------------------- |
| `msg`        | String   | The content of the text message.                                  |
| `type`       | String   | The type of the message. <li>`txt`: A text message. |


#### Send an image message

When a user sends an image message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "filename":"image",
            "size":
            {
                "width":746,
                "height":1325
            },
            "secret":"XXXXqnkRXXXXAUHNhFQyIhTJxWxvGOwyx1",
            "file_length":118179,
            "type":"img",
            "url":"https://a1.agora.com/"
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the message callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| :------------ | :------- | :---------------------------------------- |
| `filename`   | String    | The filename of the image. |
| `secret`     | String    | The secret returned after uploading the image file. |
| `size`       | Json      | The dimension of the image in pixels. <ul><li>`height`: The height of the image.</li><li>`width`: The width of the image.</li></ul>   |
| `file_length` | Int      | The size of the image in bytes. |
| `url`   | String  | The URL of the image, in the format of `https://{host}/{org_name}/{app_name}/chatfiles/{uuid}`, where `uuid` is the ID of the image file. You can fetch `uuid` from the response body after the file is uploaded. |
| `type`       | String   | The type of the message. <li>`img`: An image message. |


#### Send an audio message

When a user sends an audio message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "filename":"audio",
            "length":4,
            "secret":"XXXXynkRXXXX1e0Ksmmt2Ym6AzpRr9SxsUpF",
            "file_length":6374,
            "type":"audio",
            "url":"https://a1.agora.com/"
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the message callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| :------------ | :------- | :---------------------------------------- |
| `filename`        | String   | The filename of the audio.                               |
| `secret`          | String    | The secret returned after uploading the audio file.  |
| `file_length`  |  Long  | The size of the audio file in bytes. |
| `length`    | Int | The duration of the audio file in seconds. |
| `url`   | String  | The URL of the audio, in the format of `https://{host}/{org_name}/{app_name}/chatfiles/{uuid}`, where `uuid` is the ID of the audio file. You can fetch `uuid` from the response body after the file is uploaded. |
| `type`       | String   | The type of the message. <li>`audio`: An audio message. |

#### Send a video message

When a user sends a video message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "thumb_secret":"t1AEXXXXEeyS81-d10_HOpjSZc8TD-ud40XXXXOStQrr7Mbc",
            "filename":"video.mp4",
            "size":
            {
                "width":360,
                "height":480
            },
            "thumb":"https://a1.agora.com/agora-demo/shuang/chatfiles/XXXX0400-7a8b-11ec-8d83-7106XXXX33e6",
            "length":10,
            "secret":"XXXXgHqLXXXXBfuoalZCJPD7PVcoOu_RHTRa78bjU_KQAPr2",
            "file_length":601404,
            "type":"video",
            "url":"https://a1.agora.com/agora-demo/shuang/chatfiles/XXXX3270-7a8b-11ec-9735-6922XXXXb891"
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the message callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| --- | --- | --- |
| `thumb_secret` | String | The secret returned after uploading the video thumbnail. |
| `filename` | String | The filename of the video. |
| `size` | Json | The dimension of the video thumbnail.<ul><li>`height`: The height of the thumbnail.</li><li>`width`: The width of the thumbnail.</li></ul> |
| `thumb`   | String  | The URL of the thumbnail, in the format of `https://{host}/{org_name}/{app_name}/chatfiles/{uuid}`, where `uuid` is the ID of the video thumbnail. You can fetch `uuid` from the response body after the video thumbnail is uploaded. |
| `secret` | String | The secret returned after uploading the video file. |
| `file_length` | Long | The size of the video file in bytes. |
| `url`   | String  | The URL of the video, in the format of `https://{host}/{org_name}/{app_name}/chatfiles/{uuid}`, where `uuid` is the ID of the video file. You can fetch `uuid` from the response body after the video file is uploaded. |
| `type`       | String   | The type of the message. <li>`video`: A video message. |

#### Send a location message

When a user sends a location message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "lng":116.32309156766605,
            "type":"loc",
            "addr":"********",
            "lat":39.96612729238626
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the message callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| --- | --- | --- |
| `log` | String | The longitude of the location. |
| `lat` | String | The latitude of the location. |
| `addr` | String | The address of the location. |
| `type`       | String   | The type of the message. <li>`loc`: A location message. |

#### Send a command message

When a user sends a command message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload":
{
    "ext":{},
    "bodies":[
        {
            "msg":"rr",
            "type":"cmd"
        }
    ]
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the body of the callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| :------------ | :------- | :---------------------------------------- |
| `msg`        | String   | The content of the command message.                                 |
| `type`       | String   | The type of the message. <li>`cmd`: A command message. |

#### Send a custom message

When a user sends a custom message in a one-to-one chat, chat group, or chat room, the Agora Chat server sends a callback to your app server. Within this callback, the sample code of the `payload` field is as follows:

```json
"payload": 
{
    "ext":{}, 
    "bodies":[
        { 
            "customExts": [ {"name": 1 } ], 
            "customEvent": "flower", 
            "type": "custom" 
        }
    ] 
}
```

The `ext` field is the message extension in Object data type. The `bodies` field is the message body of the callback in Object data type, which contains the following fields:

| Field | Data Type | Description |
| --- | --- | --- |
| `customExts` | Json | The attributes of the custom event, in the `Map<String, String>` forma. The attributes can contain a maximum of 16 elements. |
| `customEvent`| String | The type of the custom event. The type can be 1 to 32 characters. |
| `type`       | String   | The type of the message. <li>`custom`: A custom message. |

### Recall a message

When a user recalls a message within a one-to-one chat, chat group, or chat room in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "chat_type":"recall",
    "callId":"orgname#appname_9664XXXX5536657404",
    "security":"ea7aXXXX14fbXXXX33d5f4f169eb4f8d",
    "payload":
    {
        "ext":{},
        "ack_message_id":"9664XXXX0900644860",
        "bodies":[]
    },
    "host":"******",
    "appkey":"orgname#appname",
    "from":"tst",
    "recall_id":"9664XXXX0900644860",
    "to":"1709XXXX2023810",
    "eventType":"chat",
    "msg_id":"9664XXXX5536657404",
    "timestamp":1642589932646
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `eventType` | String | The message type of the callback. <ul><li>`chat`: Uplink messages. The messages that are about to be sent by the Agora Chat server to end devices.</li><li>`chat_offline`: Offline messages. The messages that are not being sent by the Agora Chat server as the end user is offline.</li></ul> |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the message, in milliseconds. |
| `chat_type` | String | The type of chat. <ul><li>`chat`: One-to-one chats.</li><li>`groupchat`: Chat groups and chat rooms.</li></ul> |
| `group_id` | String | The ID of the chat group or chat room where the message resides. This field only exists if `chat_type` is set to `groupchat`. |
| `from` | String | The sender of the message. |
| `to`  | String | The recipient of the message. |
| `recall_id` | String | The ID of the message to recall. |
| `msg_id` | String | The ID of the message callback. This ID is the same as the `msg_id` when the end user sends the message. |
The `ext` field is the message extension 
| `payload` | Object | The structure of the callback that contains the following fields:<ul><li>`ext`: The message extension. This field is empty when recalling a message.</li><li>`ack_message_id`: The ID of the message to recall. This ID is the same as `recall_id`. </li><li>`bodies`: The body of the message callback. This filed is empty when recalling a message.</ul> |
| `securityVersion` | String | This parameter is reserved for future use.  |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |

## Chat group and chat room events

When a user performs operations on a chat group or chat room in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{ 
    "chat_type": "muc",
    "callId": "XXXX#XXXX", 
    "security": "XXXX", 
    "payload":{
    // The details of the callback message.
    },
    "group_id": "1735XXXX6122369",
    "host": "XXXX",
    "appkey": "XXXX#XXXX",
    "from": "XXXX#XXXX_1111@easemob.com/android_8070d7b2-795eb6e63d",
    "to": "1111",
    "eventType": "chat",
    "msg_id": "9764XXXX3882744164",
    "timestamp": 1644914583273
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `chat_type` | String | The type of the event. `muc` indicates an event occurred in a chat group or chat room. |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `eventType` | String | The message type of the callback. <ul><li>`chat`: Uplink messages. The messages that are about to be sent by the Agora Chat server to end devices.</li><li>`chat_offline`: Offline messages. The messages that are not being sent by the Agora Chat server as the end user is offline.</li></ul> |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the callback message, in milliseconds. |
| `group_id` | String | The ID of the chat group or chat room where the message resides. This field only exists if `chat_type` is set to `groupchat`. |
| `from` | String | The sender of the message. |
| `to`  | String | The recipient of the message. |
| `msg_id` | String | The ID of the callback message. This ID is the same as the `msg_id` when sending the message. |
| `payload` | Object | The structure of the callback that contains the following fields: <ul><li>`muc_id`: The unique identifier of the chat group or chat room in the Agora Chat server, in the format of `{appkey}_{group_ID}@conference.easemob.com`. </li><li>`reason`: Optional. The detailed information of the current operation. See below for details.</li><li>`is_chatroom`: Whether this event occurs in a chat room.<ul><li>`true`: Yes.</li><li>`false`: No, this event occurs in a chat group.</li></ul><li>`operation`: The current operation. See below for details.</li><li>`status`: The status of the current operation that contains the following field.<ul><li>`description`: The status description.</li><li>`error_code`: The status code.</li></ul> |
| `securityVersion` | String | This parameter is reserved for future use.  |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |

### Create a chat group or chat room

<div class="alert note">This callback is triggered only if the multi-device service is enabled. Once a user creates a chat group or chat room on one device, the Agora Chat server sends callbacks to other devices, notifying about the creation of the chat group or chat room. </div>

When a user creates a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    "muc_id": "XXXX#XXXX_173556296122369@conference.easemob.com", 
    "reason": "",
    "is_chatroom": false,
    // "create" indicates that the current operation is to create a chat group or chat room.
    "operation": "create",
    "status":
    {
        "description":"",
        "error_code": "ok"
    }
}
```

### Destroy a chat group or chat room

When a user destroys a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload": 
{ 
    "muc_id": "XXXX#XXXX_173548612157441@conference.easemob.com", 
    "reason": "", 
    "is_chatroom": false, 
    // "destroy" indicates that the current operation is to destroy a chat group or chat room.
    "operation": "destroy", 
    "status": { 
        "description": "", 
        "error_code": "ok" 
    } 
} 
```

### Request to join a chat group

When a user requests to join a chat group, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX_.com", 
    // The content of the join request.
    "reason": "join group123", 
    "is_chatroom": false, 
    // "apply" indicates that the current operation is to send a join request to a chat group or chat room.
    "operation": "apply", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Accept a join request

When a user accepts a join request to the chat group from another user, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX", 
    "reason": "", 
    "is_chatroom": false, 
    // "apply_accept" indicates that the current operation is to accept a join request.
    "operation": "apply_accept", 
    "status":
    { 
        "description": "",
        "error_code": "ok"
    }
}
```

### Invite a user to join a chat group

When a user invites another user to join a chat group, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX",
    // The content of the group invitation.
    "reason": "Hello", 
    "is_chatroom": false, 
    // "invite" indicates that the current operation is to send a group invitation to a user.
    "operation": "invite", 
    "status": { 
        "description": "",
        "error_code": "ok" 
    } 
}
```

### Accept a group invitation

When a user accepts a group invitation, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "reason": "", 
    "is_chatroom": false, 
    // "invite_accept" indicates that the current operation is to accept a group invitation.
    "operation": "invite_accept", 
    "status": { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Decline a group invitation

When a user declines a group invitation, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "reason": "", 
    "is_chatroom": false, 
    // "invite_decline" indicates that the current operation is to decline a group invitation.
    "operation": "invite_decline", 
    "status": { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Join a chat group or chat room

When a user joins a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "presence" indicates that the current operation is to join a chat group or chat room.
    "operation": "presence", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Leave a chat group or chat room

When a member leaves a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "absence" indicates that the current operation is to leave a chat group or chat room.
    "operation": "absence", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

When the owner leaves a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "leave" indicates that the current operation is to leave a chat group or chat room.
    "operation": "leave", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Remove a member from a chat group or chat room

When the owner or an admin removes a member from a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "is_chatroom": false, 
    // "kick" indicates that the current operation is to remove a member from a chat group or chat room.
    "operation": "kick", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Add a member to the block list of a chat group or chat room

When the owner or an admin adds a member to the block list of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173558358671361@conference.easemob.com", 
    "reason": "", 
    "is_chatroom": false, 
    // "ban" indicates that the current operation is to add a member to the block list.
    "operation": "ban", 
    "status": { 
        "description": "",
        "error_code": "ok" 
    } 
}
```

### Remove a member from the block list of a chat group or chat room

When the owner or an admin removes a member from the block list of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "reason": "undefined", 
    "is_chatroom": false, 
    // "allow" indicates that the current operation is to remove a member from the block list.
    "operation": "allow", 
    "status": { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Add a member to the allow list of a chat group or chat room

When the owner or an admin adds a member to the allow list of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "is_chatroom": false, 
    // "add_user_white_list" indicates that the current operation is to add a member to the allow list.
    "operation": "add_user_white_list", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
        } 
}
```

### Remove a member from the allow list of a chat group or chat room

When the owner or an admin removes a member from the allow list of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    "is_chatroom": false, 
    // "remove_user_white_list" indicates that the current operation is to remove a member from the allow list.
    "operation": "remove_user_white_list", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Mute a chat group or chat room

When a member mutes a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "block" indicates that the current operation is to mute a chat group or chat room.
    "operation": "block", 
    "status": { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Unmute a chat group or chat room

When a member unmutes a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "unblock" indicates that the current operation is to unmute a chat group or chat room.
    "operation": "unblock", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Transfer the ownership

When the owner transfers the ownership of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "assing_owner" indicates that the current operation is to transfer the ownership of a chat group or chat room.
    "operation": "assing_owner", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Promote a member to an admin

When the owner promotes a member to an admin in a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "add_admin" indicates that the current operation is to promote a member to an admin.
    "operation": "add_admin", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Demote an admin to a member

When the owner demotes an admin to a member in a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "is_chatroom": false, 
    // "remove_admin" indicates that the current operation is to demote an admin to a member.
    "operation": "remove_admin", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Mute a member in a chat group or chat room

When the owner or an admin mutes a member in a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "reason": "", 
    "is_chatroom": false, 
    // "add_mute" indicates that the current operation is to mute a member.
    "operation": "add_mute", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Unmute a member in a chat group or chat room

When the owner or an admin unmutes a member in a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "reason": "", 
    "is_chatroom": false, 
    // "remove_mute" indicates that the current operation is to unmute a member.
    "operation": "remove_mute", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Mute a chat group or chat room

When a member mutes a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173553668390913XXXX", 
    "is_chatroom": false, 
    // "ban_group" indicates that the current operation is to mute a chat group or chat room.
    "operation": "ban_group", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Unmute a chat group or chat room

When a member unmutes a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173553668390913XXXX", 
    "is_chatroom": false, 
    // "remove_ban_group" indicates that the current operation is to unmute a chat group or chat room.
    "operation": "remove_ban_group", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Update the information of a chat group or chat room

When the owner or an admin updates the information of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549200408577XXXX", 
    "is_chatroom": false, 
    // "update" indicates that the current operation is to update the information about a chat group or chat room.
    "operation": "update", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

### Update the announcements of a chat group or chat room

When the owner or an admin updates the announcements of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:


```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX",
    // The updated announcement. 
    "reason": "gogngao", 
    "is_chatroom": false, 
    // "update_announcement" indicates that the current operation is to update the announcement of a chat group or chat room.
    "operation": "update_announcement", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Delete the announcements of a chat group or chat room

When the owner or an admin deletes the announcements of a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    // The deleted announcement, which is empty.
    "reason": "", 
    "is_chatroom": false, 
    // "delete_announcement" indicates that the current operation is to delete the announcement of a chat group or chat room.
    "operation": "delete_announcement", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

### Upload a shared file to a chat group or chat room

When a user uploads a shared file to a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173560762007553XXXX", 
    "reason": "{
        \"data\":{
            \"file_id\":\"79ddf840-8e2f-11ec-bec3-ad40868b03f9\",
            \"file_name\":\"a.csv\",
            \"file_owner\":\"@ppAdmin\",
            \"file_size\":6787,
            \"created\":1644909510085
            }
    }",
    "is_chatroom": false, 
    // "upload_file" indicates that the current operation is to upload a shared file to a chat group or chat room.
    "operation": "upload_file", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    }
}
```

`reason` indicates the details of the shared file uploaded to a chat group, which contains the following fields:
- `file_id`: The ID of the file.
- `file_name`: The filename.
- `file_owner`: The owner of the file, that is, the user who uploads the file.
- `file_size`: The size of the file in bytes.
- `created`: The Unix timestamp when the file is created, in milliseconds.

### Delete a shared file in a chat group or chat room

When a user deletes a shared file in a chat group or chat room, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{ 
    "muc_id": "XXXX#XXXX173549292683265XXXX", 
    // "reason" specifies the ID of the shared file to delete. This ID is the same as the "file_id" used when uploading the file.
    "reason": "79ddf840-8e2f-11ec-bec3-ad40868b03f9", 
    "is_chatroom": false, 
    // "delete_file" indicates that the current operation is to delete a shared file in a chat group or chat room.
    "operation": "delete_file", 
    "status":
    { 
        "description": "", 
        "error_code": "ok" 
    } 
}
```

## User contact events

When a user performs operations on contacts in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "chat_type": "roster",
    "callId": "orgname#appname_9664XXXX5536657404",
    "security": "XXXXa9feXXXX69241e17b15e2783dbb1",
    "payload": {
        // The details of the callback message.
    },
    "host": "msync@ebs-ali-beijing-msync26",
    "appkey":"orgname#appname",
    "from":"tst",
    "to":"tst01",
    "eventType":"chat",
    "msg_id":"9664XXXX5536657404",
    "timestamp":1642589932646
}
```

| Field | Data Type | Description |
| --- | --- | --- |
| `chat_type` | String | The type of the event. `roster` indicate an event occurred in user contacts. |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `eventType` | String | The message type of the callback. <ul><li>`chat`: Uplink messages. The messages that are about to be sent by the Agora Chat server to end devices.</li><li>`chat_offline`: Offline messages. The messages that are not being sent by the Agora Chat server as the end user is offline.</li></ul> |
| `timestamp` | Long | The Unix timestamp when the Agora Chat server receives the message, in milliseconds. |
| `from` | String | The user who operates the contact. |
| `to`  | String | The contact that is being operated by the user. |
| `msg_id` | String | The ID of the message. This ID is the same as the `msg_id` when sending the message. |
| `payload` | Object | The structure of the callback. See below for details. |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |

### Send a contact invitation

When a user adds a contact in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    "reason":"",
    // "add" indicates that the current operation is to add a contact.
    "operation":"add"
}
```

### Remove a contact

When a user removes a contact in the Agora Chat app, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    // The version of the contact list.
    "roster_ver":"XXXXD920XXXX5B51EB0B806E83BDD97F089B0092",
    // "remove" indicates that the current operation is to remove a contact.
    "operation":"remove"
}
```

### Accept a contact invitation

When you accept the contact invitation from the other users, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    // The version of the contact list.
    "roster_ver":"XXXX14FEXXXXA9ABC52CA86C5DE1601CF729BFD6",
    // "accept" indicates that the current operation is to accept the contact invitation.
    "operation":"accept"
}
```

When the other users accept the contact invitation from you, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload": { 
    // The version of the contact list.
    "roster_ver": "XXXX718EXXXX3F0C572A5157CFC711D4F6FA490F", 
    // "remote_accept" indicates that the current operation is to accept the contact invitation.
    "operation": "remote_accept" 
}
```

### Decline a contact invitation

When you decline the contact invitation from the other users, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    // The version of the contact list.
    "roster_ver":"XXXXEC24XXXX32B2EB1B654AA446930DB9BAFE59",
    // "decline" indicates that the current operation is to decline the contact invitation.
    "operation":"decline"
}
```

When the other users decline the contact invitation from you, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload": { 
    // The version of the contact list.
    "roster_ver": "1BD5718E9C9D3F0C572A5157CFC711D4F6FA490F", 
    // "remote_decline" indicates that the current operation is to decline the contact invitation.
    "operation": "remote_decline" 
}
```

### Add a contact to the block list

When a user adds a contact to the block list, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    // "ban" indicates that the current operation is to add a contact to the block list.
    "operation":"ban",
    "status":
    {
        "error_code":"ok"
    }
}
```

### Remove a contact from the block list

When a user removes a contact from the block list, the Agora Chat server sends a callback to your app server. The sample code of the `payload` field is as follows:

```json
"payload":
{
    // "allow" indicates that the current operation is to remove a contact from the block list.
    "operation":"allow",
    "status":
    {
        "error_code":"ok"
    }
}
```

## Acknowledgement receipt events

When a user sends a receipt, the Agora Chat server sends a callback to your app server. The sample code is as follows:

```json
{
    "chat_type": "read_ack",
    "callId": "XXXX#XXXX968665325555943556",
    "security": "bd63XXXX8f72XXXX6d33e09a43aa4239",
    "payload": {
        "ext": {},
        "ack_message_id": "9686XXXX3572037776",
        "bodies": []
    },
    "host": "msync@ebs-ali-beijing-msync45",
    "appkey": "XXXX#XXXX",
    "from": "1111",
    "to": "2222",
    "eventType": "chat",
    "msg_id": "9686XXXX5555943556",
    "timestamp": 1643099771248
}
```

| Field | Data Type | Description |
| :---------- | :------- | :----------------------------------------------------------- |
| `chat_type` | String   | The type of the event. <ul><li>`read_ack`: The read receipts.</li><li>`delivery_ack`: The delivery receipts.</li></ul>                                        |
| `callId` | String | The ID of the callback. The unique identifier assigned to each callback, in the format of `{appKey}_{uuid}`, among which `uuid` is randomly generated. |
| `security` | String | The signature in the callback request used to confirm whether this callback is sent from the Agora Chat server. The signature is the MD5 hash of the `{callId} + {secret} + {timestamp}` string, among which `secret` can be found on [Agora Console](https://console.agora.io/).|
| `payload`   | Object   | The structure of the callback that contains the following fields:<ul><li>`ext`: The message extension field. </li><li>`ack_message_id`: The message ID of the receipt callback.</li><li>`bodies`: The message body.</li></ul> |
| `host` | String | The domain name assigned by the Agora Chat service to access RESTful APIs. |
| `appkey` | String | The unique identifier assigned to each app by the Agora Chat service. |
| `from`      | String   | The user who sends the receipt.                                     |
| `to`        | String   | The user who receives the receipt.                                     |
| `eventType` | String | The message type of the callback. <ul><li>`chat`: Uplink messages. The messages that are about to be sent by the Agora Chat server to end devices.</li><li>`chat_offline`: Offline messages. The messages that are not being sent by the Agora Chat server as the end user is offline.</li></ul> |
| `timestamp` | long     | The Unix timestamp when the Agora Chat server receives the event, in milliseconds.               |
| `msg_id`    | String   | The ID of the message callback.                                    |
