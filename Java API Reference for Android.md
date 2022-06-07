# Java API Reference for Android

<div class="alert note">本页提供元直播定制功能的 API 参考。实时音频互动相关功能，请查看 <a href="https://docs.agora.io/cn/video-call-4.x-beta/API%20Reference/java_ng/API/rtc_api_overview_ng.html">RTC SDK 4.0.0 API</a> 参考。</div>


## 概览

| 类  | 描述     |
| -------- | ------- |
| [RtcEngine](#rtcengine) |   实时音视频的基础接口类。   |
| [IAvatarEngine](#iavatarengine) |   元直播的基础接口类。   |
| [IAvatarEngineEventHandler](#iavatarengineeventhandler) |  元直播的回调接口类。   |
| [类型定义](#类型定义) |   元直播接口的类型定义。   |


## RtcEngine

实时音视频引擎。

### queryAvatarEngine

创建虚拟形象引擎类。

```java
public abstract IAvatarEngine queryAvatarEngine();
```

在调用其他接口前，先调用该方法创建一个 `IAvatarEngine` 对象。

**返回值**
- 方法调用成功，返回一个 `IAvatarEngine` 对象。
- 方法调用失败，返回值为空。



## IAvatarEngine

元直播的基础接口类，app 可调用主要方法实现元直播的相关功能。


### initialize

初始化虚拟形象引擎。

```java
public abstract int initialize(AvatarContext context);
```

请确保在调用其他 API 前先调用该方法创建并初始化 `IAvatarEngine`。

**参数**  
`context`  
初始化虚拟形象引擎时的配置参数。详见 [AvatarContext](#avatarcontext)。

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### enableOrUpdateLocalAvatarVideo

启用或更新本地虚拟形象的视频轨道。

```java
public abstract int enableOrUpdateLocalAvatarVideo(boolean enabled, AvatarConfigs configs);
```

**参数**  
`enabled`  
是否为本地虚拟形象启用视频轨道： 
- `true`: 启用。
- `false`: (默认) 不启用。

`configs`  
启用视频轨道的配置参数。详见 [AvatarConfigs](#avatarconfigs)。

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### enableLocalUserAvatarItems

enable avatar items. // TODO: 这里 item 具体指的是啥？在什么时候调用这个接口？时序图里没看到。

```java
public abstract int enableLocalUserAvatarItems(boolean enable, int type, String bundle);
```

**参数**  
`enabled`
- true: Enable avatar items.
- false: (default) Disable avatar items.

`type`  
avatar item type.

`bundle`  
bundle ID.

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### setupLocalVideoCanvas

初始化虚拟形象的本地视频流。// TODO: 这里的 canvas 是仅指虚拟形象的画布，还是也包含真人的？

```java
public abstract int setupLocalVideoCanvas(VideoCanvas canvas);
```

请确保在调用 [joinChannel](https://docs.agora.io/cn/video-call-4.x-beta/API%20Reference/java_ng/API/class_irtcengine.html#api_joinchannel) 方法加入频道前，先调用该方法。
调用本方法后，初始化效果仅供用户在本地设备上预览，不会对外发布。

**参数**  
`canvas`  
视频画布对象的属性。详见 [VideoCanvas](https://docs.agora.io/cn/live-streaming-premium-4.x-beta/API%20Reference/java_ng/API/rtc_api_data_type.html?platform=Android#class_videocanvas_ng)。
如需释放本地视频流，可将 `VideoCanvas` 中的 `view` 属性设为空字符串。// TODO: unbind 怎么理解？

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### setLocalUserAvatarOptions

设置本地虚拟形象选项。

```java
public abstract int setLocalUserAvatarOptions(String key, byte[] value);
```

**参数**  
设置不同的功能选项，需将 `key` 和 `value` 指定为不同的值。具体见下表：

| 功能 | `key` | `value` |
|:-----|:-----|:-----|
|开启/关闭面捕| 开启：`start_facetracking`<br>关闭：`stop_facetracking` | 无 |
|设置渲染质量| `set_quality` | 渲染质量分为 4 档：<br><li>`0`: low<li>`1`: medium<li>`2`: high<li>`3`: ultra //TODO: 默认是哪一种？|
|设置背景| `set_bg` | 预置 3 种背景：<br><li>`bg2d10001`<li>`bg2d10002`<li>`bg2d10003` //TODO: 默认是哪一种？|
|调试面板控制| 打开 bs info 面板：`open_bspanel` //TODO: 什么是bs info面板？<br>关闭 bs info 面板：`close_bspanel` | 无 |
|面捕识别不到人脸控制|  调用虚拟形象休眠动画：`playAniIdle`<br>恢复面捕：`resumeFaceTrack`  | 无 |
|更换环境设置|  `update_setting` //TODO: input里key和value的具体取值不太明晰  | 无 |
|开启/关闭换装| 开启：`start_dress`<br>关闭：`stop_dress` | 无 |
| 换装 | `send_showview`// TODO: 这几个key分别代表什么不同的操作？ | 传入换装项的 type，示例：`{\"type\":\"53\"}`。// TODO: type是啥？在getxxx那里也没有这个属性。 |
| 换装 | `send_dress` | 传入换装项的 id，示例：`"{\"id\":\"111071\"}"`。 |
| 换装 | `send_takeoff` | 传入换装项的 type，示例：`{\"type\":\"53\"}`。 |
| 换装 | `send_setcolor` | cocos 未给出定义 // |
|开启/关闭捏脸| 开启：`start_faceedit`<br>关闭：`stop_faceedit` | 无 |
|捏脸| `send_felist` | 传入一个或多个由捏脸项 ID 和对应系数组成的 JSON 字符串。参考 [示例代码](#samplecode)。|


**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### getLocalUserAvatarOptions

获取本地虚拟形象的参数。

```java
public abstract String getLocalUserAvatarOptions(String key, String args);
```

**参数**  
`key`  
该参数指定为不同值时，代表不同的操作：
- `request_dresslist`: 获取本地用户的服装可选项。<a name="dress"></a>
- `request_felist`: 获取本地用户的捏脸可选项。<a name="face"></a>

`args`  
该参数指定为不同值时，代表不同的范围：
- 传入空字符串，获取全部可选项的信息。
- 传入可选项 ID，获取指定可选项的信息。// TODO: 怎么知道某个 ID 对应的是哪个选项呢？

**返回值**
- 方法调用成功，会触发 [`onLocalUserAvatarEvent`](#onlocaluseravatarevent) 回调，异步返回对应信息。// TODO: 这里 string 指的是啥？
- 方法调用失败，返回值为空。


### registerEventHandler

添加虚拟形象引擎的事件回调。

```java
public abstract int registerEventHandler(IAvatarEngineEventHandler handler);
```

**参数**  
`handler`  
待添加的回调句柄。详见 [IAvatarEngineEventHandler](#iavatarengineeventhandler)。

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。


### unregisterEventHandler

移除订阅虚拟形象引擎的事件回调。

```java
public abstract int unregisterEventHandler(IAvatarEngineEventHandler handler);
```
 
**参数**  
`handler`  
待移除的回调句柄。详见 [IAvatarEngineEventHandler](#iavatarengineeventhandler)。

**返回值**
- 0: 方法调用成功。
- < 0: 方法调用失败。



## IAvatarEngineEventHandler

元直播的回调接口类，用于 SDK 向 app 发送事件通知。


### onLocalUserAvatarStarted

本地虚拟形象创建回调。

```java
public abstract void onLocalUserAvatarStarted(boolean success, int error_code, String msg);
```

在调用 ``//TODO: 待确认 方法后，会触发该回调，通知本地用户虚拟形象的创建结果。

**参数**  
`success`  
本地用户虚拟形象启动状态：
- `true`: 启动成功。
- `false`: 启动失败。

`error_code`  
错误码。0 表示启动成功，其他表示失败。// TODO: 是否有错误码和错误描述的类型定义可以补充？

`msg`  
错误描述。


### onLocalUserAvatarError

本地虚拟形象异常回调。
 
```java
public abstract void onLocalUserAvatarError(int err_code, String msg);
```

当用户的本地虚拟形象抛出异常时，会触发该回调，返回错误码及错误描述用于后续排查。

**参数**
`err_code`  
错误码。// TODO: 是否有错误码和错误描述的类型定义可以补充？

`msg`  
错误描述。


### onLocalUserAvatarEvent

获取本地虚拟形象参数回调。// TODO: 角色加载成功或失败是哪个方法触发的回调？和它有什么区别 onLocalUserAvatarStarted？

```java
public abstract void onLocalUserAvatarEvent(String key, String buf);
```

在成功调用 [`getLocalUserAvatarOptions`](#getlocaluseravataroptions) 方法后，会触发该回调，返回本地用户的可用服装列表和可用捏脸特性等。

**参数**  
如获取本地用户的服装可选项，回调参数如下：  
`key`  
[`request_dresslist`](#dress)。与调用 `getLocalUserAvatarOptions` 方法时传入的 key 值相同。

`buf`  
JSON 字符串。示例如下：// TODO: char* string 怎么理解？

```json
{
  // "id" 表示可选项的 ID。
  // "name" 表示可选项的名称。
  // "icon" 后的 HTTPS 链接为图标的链接。
  // "isUsing" 表示当前是否穿戴。
  // TODO: version / tag / status / zOrder 的含义？
    "55":[
            {"id":"55001","name":"眉毛1","icon":"https:\/\/store.gtx.fun\/avatar\/cjie\/icon\/55001.png","version":0,"tag":0,"status":0,"isUsing":0,"zOrder":0},
            {"id":"55002","name":"眉毛2","icon":"https:\/\/store.gtx.fun\/avatar\/cjie\/icon\/55002.png","version":0,"tag":0,"status":0,"isUsing":1,"zOrder":0}
        ],
    "70":[
            {"id":"70001","name":"发型1","icon":"https:\/\/store.gtx.fun\/avatar\/cjie\/icon\/70001.png","version":0,"tag":0,"status":0,"isUsing":0,"zOrder":0},
            {"id":"70002","name":"发型2","icon":"https:\/\/store.gtx.fun\/avatar\/cjie\/icon\/70002.png","version":0,"tag":0,"status":0,"isUsing":0,"zOrder":0},
        ]
}
```

如获取本地用户的捏脸可选项，回调参数如下：  
`key`  
[`request_felist`](#face)。与调用 `getLocalUserAvatarOptions` 方法时传入的 key 值相同。

`buf`  
JSON 字符串。示例如下：
<a name="samplecode"></a>

```json
{
  // 前面的字符串表示捏脸项的 ID。
  // 后面的浮点数表示捏脸项对应的系数。
    "110203":0.5,
    "110204":0.6,
    "110205":0.5
}
```



## 类型定义

元直播 API 的类型定义。

### AvatarContext

虚拟形象引擎的参数。

```java
public class AvatarContext {
  public String ai_appId;
  public String ai_license;
 
  public AvatarContext(String ai_appId, String ai_license) {
    this.ai_appId = ai_appId;
    this.ai_license = ai_license;
  }
}
```

**属性**  // TODO: 需要补充属性描述、调整代码原型
`ai_appId`

`ai_license`

 
### AvatarConfigs

虚拟形象视频轨道的配置。

```java
public class AvatarConfigs {
  public Constants.MediaSourceType mediaSource = Constants.MediaSourceType.PRIMARY_CAMERA_SOURCE;
  public Context context;
  public boolean enable_face_detection = true;
  public boolean enable_human_detection = true;
  public Constants.AvatarProcessingMode mode =
      Constants.AvatarProcessingMode.AVATAR_PROCESSING_MODE_AVATAR;
 
  public AvatarConfigs(Context context) {
    this.context = context;
  }
 
  public AvatarConfigs(Constants.MediaSourceType mediaSource, Context context,
      boolean enable_face_detection, boolean enable_human_detection,
      Constants.AvatarProcessingMode mode) {
    this.mediaSource = mediaSource;
    this.context = context;
    this.enable_face_detection = enable_face_detection;
    this.enable_human_detection = enable_human_detection;
    this.mode = mode;
  }
};
```

**属性**  // TODO: 需要补充属性描述、调整代码原型
`mediaSource`

`context`

`enable_face_detection`

`enable_human_detection`

`mode`