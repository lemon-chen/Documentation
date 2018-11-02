
---
title: 游戏 API
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:12:52 GMT+0000 (UTC)
---
# 游戏 API
<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Java 接口类</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><a href="#rtcengine">RtcEngine 接口类</a></td>
<td>包含 App 要调用的主要方法</td>
</tr>
<tr><td><a href="#iaudioeffectmanager">IAudioEffectManager 接口类</a></td>
<td>包含 App 管理音效的方法</td>
</tr>
<tr><td><a href="#irtcengineeventhandler">IRtcEngineEventHandler 抽象类</a></td>
<td>包含回调方法</td>
</tr>
</tbody>
</table>


<a id = "irtcengine"></a>
## RtcEngine 接口类

### 创建 RtcEngine 对象 (create)

```
public static RtcEngine create(Context context,
                                    String appId,
                                    IRtcEngineEventHandler  handler )
```

本方法创建 RtcEngine 对象。

目前 Agora Native SDK 只支持一个 RtcEngine 实例，每个应用程序仅创建一个 RtcEngine 对象 。 RtcEngine 类的所有接口函数，如无特殊说明，都是异步调用，对接口的调用建议在同一个线程进行。所有返回值为 int 型的 API，如无特殊说明，返回值 0 为调用成功，返回值小于 0 为调用失败。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>context</code></td>
<td>安卓活动 (Android Activity) 的上下文</td>
</tr>
<tr><td><code>appId</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
<tr><td><code>handler</code></td>
<td>IRtcEngineEventHandler 是一个提供了缺省实现的抽象类，SDK 通过该抽象类向应用程序报告 SDK 运行时的各种事件</td>
</tr>
<tr><td>返回值</td>
<td>RtcEngine 对象</td>
</tr>
</tbody>
</table>



### 设置频道属性 (setChannelProfile)

```
public int setChannelProfile (int profile)
```

该方法用于设置频道模式 (Profile)。Agora RtcEngine 需知道应用程序的使用场景, 从而使用不同的优化手段。

> -   同一频道内只能同时设置一种模式。
> -   该方法必须在加入频道前调用和进行设置，进入频道后无法再设置。


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>profile</code></td>
<td><p>频道模式:</p>
<div><ul>
<li>CHANNEL_PROFILE_GAME_FREE_MODE = 2: 自由发言模式</li>
<li>CHANNEL_PROFILE_GAME_COMMAND_MODE = 3: 指挥模式</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置用户角色 (setClientRole)

```
public int setClientRole (int role);
```

在加入频道前，用户需要通过本方法设置观众 (默认) 或主播模式。在加入频道后，用户可以通过本方法切换用户模式。

> 该方法仅适用于: 您已通过调用 `setChannelProfile` 将频道模式设置为指挥模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>指挥模式频道的用户角色:</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1; 主播(既指挥者)，主播可以发言指挥</li>
<li>CLIENT_ROLE_AUDIENCE = 2; 观众(默认，被指挥者)，观众服从指挥</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 加入频道 (joinChannel)

```
public abstract int joinChannel(String token,
                                String channelName,
                                String optionalInfo,
                                int optionalUid);
```

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `leaveChannel` 退出当前通话，才能进入下一个频道。

 
> 同一个频道里不能出现两个相同的 UID。如果你的 App 支持多设备同时登录，即同一个用户账号可以在不同的设备上同时登录 (例如微信支持在 PC 端和移动端同时登录)，请保证传入的 UID 不相同。 例如你之前都是用同一个用户标识作为 UID, 建议从现在开始加上设备 ID, 以保证传入的 UID 不相同 。如果你的 App 不支持多设备同时登录，例如在电脑上登录时，手机上会自动退出，这种情况下就不需要在 UID 上添加设备 ID。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>token</code></td>
<td><ul>
<li>安全要求不高: 将值设为 null</li>
<li>安全要求高: 将值设置为 Token 值。 如果你已经启用了 App Certificate, 请务必使用 Token。 关于如何获取 Token，详见 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a> 。</li>
</ul>
</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>optionalInfo</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户</td>
</tr>
<tr><td><code>optionalUid</code></td>
<td><p>(非必选项) 用户ID，32位无符号整数。建议设置范围：1到(2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK不对该返回值进行维护</p>
<p>uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用(uid&amp;0xffffffffL)转换成 64 位整数</p>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2)：传递的参数无效</li>
<li>ERR_NOT_READY (-3)：没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



### 离开频道 (leaveChannel)

```
public int leaveChannel();
```

离开频道，即挂断或退出通话。通过调用 joinChannel 加入频道后，必须调用 `leaveChannel` 以结束通话，否则不能进行下一次通话。 不管当前是否在通话中，都可以调用 `leaveChannel`，没有副作用。`leaveChannel` 会把会话相关的所有资源释放掉。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 打开音频 (enableAudio)

```
public int enableAudio();
```

打开音频\(默认为开启状态\)。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 关闭音频 (disableAudio)

```
public int disableAudio();
```

关闭音频。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置日志文件 (setLogFile)

```
public int setLogFile (String filePath);
```

设置 SDK 输出的日志文件。SDK 运行时产生的所有日志将写入该文件。应用程序必须保证指定的目录存在而且可写。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filepath</code></td>
<td>日志文件的完整路径</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置日志过滤器 (setLogFilter)

```
public int setLogFilter (int filter);
```

设置 SDK 的输出日志过滤器。不同的过滤器可以用或组合。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filter</code></td>
<td><p>设置过滤器等级:</p>
<div><ul>
<li>LOG_FILTER_OFF = 0;</li>
<li>LOG_FILTER_CRITICAL = 0x0008;</li>
<li>LOG_FILTER_ERROR = 0x000c;</li>
<li>LOG_FILTER_WARN = 0x000e;</li>
<li>LOG_FILTER_INFO = 0x000f;</li>
<li>LOG_FILTER_CRITICAL = 0x080f;</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 修改语音路由的默认值 (setDefaultAudioRouteToSpeakerPhone)

```
public int setDefaultAudioRouteToSpeakerphone(boolean defaultToSpeaker);
```

该方法将语音路由的默认值修改为 *扬声器 (外放)* / *听筒*。插上耳机或连接蓝牙的动作对语音路由影响的优先级高于该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>defaultToSpeaker</code></td>
<td><ul>
<li>True: 将语音路由设置为扬声器(外放)</li>
<li>False: 将语音路由设置为听筒</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

> -   该方法只在纯音频模式下工作，在有视频的模式下不工作。
> -   如果插上耳机或连接蓝牙，语音路由会发生相应改变。拔出耳机或断开蓝牙后，语音路由将恢复成默认值。


语音路由的默认值如下表:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>频道模式</strong></td>
<td><strong>默认语音路由</strong></td>
</tr>
<tr><td>自由说话模式</td>
<td>听筒</td>
</tr>
<tr><td>指挥模式</td>
<td>外放</td>
</tr>
</tbody>
</table>



### 将语音路由设置为扬声器外放 (setEnableSpeakerphone)

```
public int setEnableSpeakerphone( boolean enabled );
```

该方法将语音路由设置为 *扬声器 (外放)*。该方法对语音路由影响的优先级高于插上耳机或连接蓝牙的动作。

调用该方法后，SDK 将返回 `onAudioRouteChanged` 回调提示状态已更改。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enabled</code></td>
<td><p>是否将语音路由设置为扬声器(外放)：</p>
<div><ul>
<li><p>True:</p>
<div><ul>
<li>如果用户已在频道内，调用该 API 后，会切换到从扬声器(外放)出声</li>
<li>如果用户尚未加入频道，调用该API后，在加入频道时会默认从扬声器(外放)出声</li>
</ul>
</div>
</li>
	<li><p>False: 语音会根据语音路由的默认值出声，详见 <code>setDefaultAudioRouteToSpeakerPhone</code></p>
</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 1: 方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>


> 使用该方法后，插上耳机或连接蓝牙等动作不影响语音路由。

### 检查语音路由是否设置为扬声器外放 (isSpeakerphoneEnabled)

```
public boolean isSpeakerphoneEnabled();
```

该方法检查语音路由是否设置为扬声器外放。

### 启用说话者音量提示 (enableAudioVolumeIndication)

```
public int enableAudioVolumeIndication (int interval, int smooth);
```

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>interval</code></td>
<td><p>指定音量提示的时间间隔。</p>
<div><ul>
<li>&lt;=0: 禁用音量提示功能</li>
<li>&gt;0: 提示间隔，单位为毫秒。建议设置到大于 200 毫秒</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>smooth</code></td>
<td>平滑系数。默认可以设置为 3</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 将自己静音 (muteLocalAudioStream)

```
public int muteLocalAudioStream (boolean muted);
```

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。

> 当设置为 True 时, 该方法不禁用麦克风，因此不影响正在进行的录制。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: 设置本地静音，即麦克风静音</li>
<li>False: 取消本地静音</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 静音所有远端音频 (muteAllRemoteAudioStreams)

```
public int muteAllRemoteAudioStreams (boolean mute);
```

该方法用于允许/禁止播放远端用户的音频流。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: 停止播放所有收到的音频流</li>
<li>False: 允许播放所有收到的音频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 静音指定用户音频 (muteRemoteAudioStream)

```
public int muteRemoteAudioStream (int uid, boolean mute);
```

该方法用于允许/禁止播放远端用户的音频流。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>指定用户的 UID</td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止播放指定用户的音频流</li>
<li>False: 允许播放指定用户的音频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 查询 SDK 版本号 (getSdkVersion)

```
public static String getSdkVersion();
```

该方法返回 SDK 版本号字符串。

### 获取错误描述 (getErrorDescription)

```
public static String getErrorDescription(int error);
```

该方法获取详细的错误描述。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>error</code></td>
<td><code>onWarning</code> 或 <code>onError</code> 里返回的警告代码或错误代码。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 更新 Token (renewToken)

```
public abstract int renewToken(String token);
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   发生 `onTokenPrivilegeWillExpire` 回调时，
-   `onError` 回调报告 ERR_TOKEN_EXPIRED (109) 时，
-   `onRequestToken` 回调报告 ERR_TOKEN_EXPIRED (109) 时，


应用程序应重新获取 Token，然后调用该 API 更新 Token，否则 SDK 无法和服务器建立连接。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>token</code></td>
<td>指定要更新的 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 开始客户端录音 (startAudioRecording)

```
public int startAudioRecording( String filePath );
```

Agora SDK 支持通话过程中在客户端进行录音，且录音文件格式可以为:

-   .wav : 文件大，音质保真度高
-   .aac : 文件小，有一定的音质保真度损失


确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `leaveChannel` 时还在录音，录音会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filePath</code></td>
<td>录音文件路径名。该文件名字符串为 UTF-8 编码</td>
</tr>
<tr><td><code>quality</code></td>
<td><p>录音音质:</p>
<ul>
<li>AUDIO_RECORDING_QUALITY_LOW = 0: 低音质。录制 10 分钟的文件大小为 1.2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_MEDIUM = 1: 中音质。录制 10 分钟的文件大小为 2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_HIGH = 2: 高音质。录制 10 分钟的文件大小为 3.75 M 左右</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止客户端录音 (stopAudioRecording)

```
public int stopAudioRecording();
```

该方法停止客户端录音。该接口需要在 `leaveChannel` 之前调用，如果没有调用，在 `leaveChannel` 时会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 开始客户端本地混音 (startAudioMixing)

```
public int startAudioMixing (String filePath,
                             boolean loopback,
                             boolean replace,
                             int cycle
                             int playTime);
```

指定本地音频文件来和麦克风采集的音频流进行混音和替换 (用音频文件替换麦克风采集的音频流)，可以通过参数选择是否让对方听到本地播放的音频和指定循环播放的次数。

> -   如需调用该方法，请确保使用 Android 4.2 或以上设备，且 API Level >=16。
> -   请在频道内调用该方法，如果在频道外调用该方法可能会出现问题。


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filePath</code></td>
<td><p>指定需要混音的本地音频文件名和文件路径名:</p>
<ul>
<li>如果用户提供的目录以 /assets/ 开头，则去 assets 里面查找该文件</li>
<li>如果用户提供的目录不是以 /assets/ 开头，一律认为是在绝对路径里查找该文件</li>
</ul>
<p>支持以下音频格式: mp3，aac，m4a, 3gp, wav, flac</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>loopback</code></td>
<td><ul>
<li>True: 只有本地可以听到混音或替换后的音频流</li>
<li>False: 本地和对方都可以听到混音或替换后的音频流</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>replace</code></td>
<td><ul>
<li>True: 音频文件内容将会替换本地录音的音频流</li>
<li>False: 音频文件内容将会和麦克风采集的音频流进行混音</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>cycle</code></td>
<td><p>指定音频文件循环播放的次数:</p>
<ul>
<li>正整数: 循环的次数</li>
<li>-1：无限循环</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止客户端本地混音 (stopAudioMixing)

```
public int stopAudioMixing();
```

该方法停止播放伴奏。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停播放伴奏 (pauseAudioMixing)

```
public int pauseAudioMixing();
```

该方法暂停播放伴奏。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 恢复播放伴奏 (resumeAudioMixing)

```
public int resumeAudioMixing();
```

该方法恢复混音，继续播放伴奏。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 调节伴奏音量 (adjustAudioMixingVolume)

```
public int adjustAudioMixingVolume(int volume);
```

该方法调节混音里伴奏的音量大小。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td>伴奏音量范围为 0~100，默认 100 为原始文件音量</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取伴奏时长 (getAudioMixingDuration)

```
public int getAudioMixingDuration();
```

该方法获取伴奏时长，单位为毫秒。请在频道内调用该方法。如果返回值 <0，表明调用失败。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取伴奏播放进度 (getAudioMixingCurrentPosition)

```
public int getAudioMixingCurrentPosition();
```

该方法获取当前伴奏播放进度，单位为毫秒。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 调节录音信号音量 (adjustRecordingSignalVolume)

```
public int adjustRecordingSignalVolume(int volume);
```

该方法调节录音信号音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td><p>录音信号音量可在 0~400 范围内进行调节:</p>
<ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的 4 倍(自带溢出保护)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 调节播放信号音量 (adjustPlaybackSignalVolume)

```
public int adjustPlaybackSignalVolume(int volume);
```

该方法调节播放信号音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td><p>播放信号音量可在 0~400 范围内进行调节:</p>
<ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的 4 倍(自带溢出保护)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 获取音效管理器 (getAudioEffectManager)

```
public IAudioEffectManager getAudioEffectManager();
```

该方法获取与当前 rtc 引擎相关联的 IAudioEffectManager 对象。

### 查询媒体引擎版本号 (getMediaEngineVersion)

```
public static String getMediaEngineVersion();
```

该方法返回媒体引擎版本号。

<a id = "iaudioeffectmanager"></a>
## IAudioEffectManager 接口类

### 设置仅限语音模式 (setVoiceOnlyMode)

```
public int setVoiceOnlyMode(boolean enable);
```

该方法设置仅限语音模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enable</code></td>
<td><ul>
<li>True: 启用 <em>仅限语音</em> 模式</li>
<li>False: 禁用 <em>仅限语音</em> 模式</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置远端用户的语音位置 (setRemoteVoicePosition)

```
public int setRemoteVoicePosition(int uid, double pan, double gain);
```

该方法设置远端用户的语音位置。

此方法可以实现听声辨位功能。

> 该方法只在耳机模式下有效，在外放模式下无效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>远端用户的 UID</td>
</tr>
<tr><td><code>pan</code></td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1, 1]:</p>
<ul>
<li>0: 表示该音效出现在正前方</li>
<li>-1: 表示该音效出现在左边</li>
<li>1: 表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td><p>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。</p>
<p>默认值为 100.0。取值越小，音效的音量越低。</p>
</td>
</tr>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 设置本地语音音调 (setLocalVoicePitch)

```
public int setLocalVoicePitch(double pitch);
```

该方法改变本地说话人声音的音调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>pitch</code></td>
<td>语音频率可以在 [0.5, 2.0] 范围内设置。默认值为 1.0 。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取音效音量 (getEffectsVolume)

```
double getEffectsVolume();
```

该方法获取音量的音量，范围为 [0.0, 1.0]。

### 设置音效音量 (setEffectsVolume)

```
public int setEffectsVolume(double volume);
```

该方法设置音效的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td>取值范围为 [0.0, 100.0]。 100.0 为默认值</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 实时调整音效音量 (setVolumeOfEffect)

```
public int setVolumeOfEffect(int soundId, double volume);
```

该方法实时调整指定音效的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td><code>volume</code></td>
<td>取值范围为 [0.0, 100.0]。 100.0 为默认值</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 播放指定音效 (playEffect)

```
public int playEffect(uint32_t soundId, const char *pszFilePath, bool loop, double pitch, double pan, double gain);
```

该方法播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID <sup>[1]</sup></td>
</tr>
<tr><td><code>filePath</code></td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td><code>loop</code></td>
<td><p>设置是否循环播放音效:</p>
<ul>
<li>True: 是的，循环播放音效</li>
<li>False: 不循环播放(默认)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pitch</code></td>
<td><p>设置音效的音效</p>
<p>取值范围为 [0.5，2]。默认值 1.0，表示不需要修改音调。取值越小，音调越低</p>
</td>
</tr>
<tr/>
<tr><td><code>pan</code></td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1, 1]:</p>
<ul>
<li>0: 表示该音效出现在正前方</li>
<li>-1: 表示该音效出现在左边</li>
<li>1: 表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。默认值为 100.0。取值越小，音效的音量越低</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [1] 如果你已通过 `preloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `preloadEffect` 设置的 `soundId` 相同。

### 停止播放指定音效 (stopEffect)

```
public int stopEffect(uint32_t soundId);
```

该方法停止播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止播放所有音效 (stopAllEffects)

```
public int stopAllEffects();
```

该方法停止播放所有音效。

### 预加载音效 (preloadEffect)

```
public int preloadEffect(uint32_t soundId, const char *pszFilePath);
```

该方法将指定音效文件 (压缩的语音文件)预加载至内存。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td><code>filePath</code></td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 释放音效 (unloadEffect)

```
public int unloadEffect(uint32_t soundId);
```

该方法将指定预加载的音效从内存里释放出来。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 暂停音效播放 (pauseEffect)

```
public int pauseEffect(uint32_t soundId);
```

该方法暂停播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 暂停所有音效播放 (pauseAllEffects)

```
public int pauseAllEffects();
```

该方法暂停播放所有音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 恢复播放指定音效 (resumeEffect)

```
public int resumeEffect(uint32_t soundId);
```

该方法恢复播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 恢复播放所有音效 (resumeAllEffects)

```
public int resumeAllEffects();
```

该方法恢复播放所有音效。

<a id = "irtcengineeventhandler"></a>
## IRtcEngineEventHandler 抽象类

### 加入频道回调 (onJoinChannelSuccess)

```
public void onJoinChannelSuccess(String channel, int uid, int elapsed);
```

表示客户端已经登入服务器，且分配了频道 ID 和用户 ID。频道 ID 的分配是根据 `joinChannel` API 中指定的频道名称。如果调用 `joinChannel` 时并未指定用户 ID，服务器就会分配一个。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID。如果 <code>joinChannel</code> 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从 <code>joinChannel</code> 开始到该事件产生的延迟（毫秒)</td>
</tr>
</tbody>
</table>



### 重新加入频道回调 (onRejoinChannelSuccess)

```
public void onRejoinChannelSuccess(String channel, int uid, int elapsed);
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK 会进行自动重连，自动重连成功后触发此回调方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID。如果 <code>joinChannel</code> 里已经指定了 UID，返回该 UID。如果没有，返回 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从 <code>joinChannel</code> 开始到该事件产生的延迟（毫秒)</td>
</tr>
</tbody>
</table>



### 发生警告回调 (onWarning)

```
public void onWarning(int warn);
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）警告。通常情况下，SDK上报的警告信息应用程序可以忽略，SDK 会自动恢复。 例如和服务器失去连接时，SDK 可能会上报 ERR_OPEN_CHANNEL_TIMEOUT 警告，同时自动尝试重连。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>warn</code></td>
<td>警告代码</td>
</tr>
<tr><td><code>msg</code></td>
<td>警告消息</td>
</tr>
</tbody>
</table>



### 发生错误回调 (onError)

```
public void onError(int err);
```

表示 SDK 运行时出现了（网络或媒体相关的）错误。

通常情况下，SDK 上报的错误意味着 SDK 无法自动恢复，需要 APP 干预或提示用户。 例如启动通话失败时，SDK 会上报 ERR_START_CALL 错误。APP 可以提示用户启动通话失败，并调用 `leaveChannel` 退出频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



### 声音质量回调 (onAudioQuality)

```
public void onAudioQuality(int uid, int quality, short delay, short lost);
```

在通话中，该回调方法每两秒触发一次，报告当前通话的（嘴到耳）音频质量。默认启用。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>说话方的用户 ID</td>
</tr>
<tr><td><code>quality</code></td>
<td><p>语音质量评分:</p>
<div><ul>
<li>QUALITY_EXCELLENT( = 1)</li>
<li>QUALITY_GOOD( = 2)</li>
<li>QUALITY_POOR( = 3)</li>
<li>QUALITY_BAD( = 4)</li>
<li>QUALITY_VBAD( = 5)</li>
<li>QUALITY_DOWN( = 6)</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>delay</code></td>
<td>延迟(毫秒)</td>
</tr>
<tr><td><code>lost</code></td>
<td>丢包率(百分比)</td>
</tr>
</tbody>
</table>



### 离开频道回调 (onleaveChannel)

```
public void onLeaveChannel(IRtcEngineEventHandler.RtcStats stats);
```

应用程序调用 `leaveChannel` 方法时，SDK提示应用程序离开频道成功。在该回调方法中，应用程序可以得到此次通话的总通话时长、SDK 收发数据的流量等信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>stats</code></td>
<td><p>通话相关的统计信息:</p>
<div><ul>
<li><code>totalDuration</code>: 通话时长（秒），累计值</li>
<li><code>txBytes</code>: 发送字节数（bytes), 累计值</li>
<li><code>rxBytes</code>: 接收字节数（bytes), 累计值</li>
<li><code>txKBitRate</code>: 发送码率（kbps), 瞬时值</li>
<li><code>rxKBitRate</code>: 接收码率（kbps), 瞬时值</li>
<li><code>lastmileQuality</code>: 客户端接入网络质量</li>
<li><code>cpuTotalQuality</code>: 当前系统的CPU使用率（%）</li>
<li><code>cpuAppQuality</code>: 当前应用程序的CPU使用率（%）</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



```
struct SessionStat {
    int totalDuration;
    int txBytes;
    int rxBytes;
    int txKBitRate;
    int rxKBitRate;
    int lastmileQuality;
    int cpuTotalUsage;
    int cpuAppUsage;
    };
```

### 其他用户加入当前频道回调 (onUserJoined)

```
public void onUserJoined (int uid, int elapsed);
```

提示有用户加入了频道。如果该客户端加入频道时已经有人在频道中，SDK也会向应用程序上报这些已在频道中的用户。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从调用 <code>joinChannel</code> 到本回调触发的时间间隔(毫秒)</td>
</tr>
</tbody>
</table>



### 其他用户离开当前频道回调 (onUserOffline)

```
public void onUserOffline (int uid, int reason);
```

提示有用户离开了频道（或掉线）。

SDK 判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15秒）没有收到对方的任何数据包，判定为对方掉线。在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

### 语音路由已变更回调 (onAudioRouteChanged)

```
public void onAudioRouteChanged(int routing);
```

该回调返回当前的语音路由已切换至听筒，外放 (扬声器)，耳机或蓝牙。

### 上下麦回调 (onClientRoleChanged)

```
public void onClientRoleChanged(CLIENT_ROLE_TYPE oldRole, CLIENT_ROLE_TYPE newRole)
```

直播场景下，当用户上下麦时会触发此回调，即主播切换为观众时，或观众切换为主播时。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>oldRole</code></td>
<td>切换前的角色</td>
</tr>
<tr><td><code>newRole</code></td>
<td>切换后的角色</td>
</tr>
</tbody>
</table>



```
enum CLIENT_ROLE_TYPE
{
    CLIENT_ROLE_BROADCASTER = 1,
    CLIENT_ROLE_AUDIENCE = 2,
};
```

### 其他用户已停发/已重发视频流回调 (onUserMuteVideo)

```
public void onUserMuteVideo(int uid, boolean muted);
```

提示有其他用户暂停/恢复了视频流的发送。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID，提示是哪个用户的视频流</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>true: 该用户已暂停发送视频流</li>
<li>false: 该用户已恢复发送视频流</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Rtc Engine 统计数据回调 (onRtcStats)

```
public void onRtcStats(IRtcEngineEventHandler.RtcStats stats);
```

该回调定期上报 Rtc Engine 的运行时的状态，每两秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>stats</code></td>
<td>请参考 <code>onLeaveChannel</code> 的描述</td>
</tr>
</tbody>
</table>



### 频道内网络质量报告回调 (onNetworkQuality)

```
public void onNetworkQuality(int uid, int txQuality, int rxQuality);
```

该回调每 2 秒触发，向APP报告频道内用户当前的上行、下行网络质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID。表示该回调报告的是持有该ID的用户的网络质量。当 uid 为 0 时，返回的是本地用户的网络质量。</td>
</tr>
<tr><td><code>txQuality</code></td>
<td><p>该用户的上行网络质量:</p>
<div><ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>rxQuality</code></td>
<td><p>该用户的下行网络质量:</p>
<div><ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



### 伴奏播放结束回调 (onAudioMixingFinished)

```
public void onAudioMixingFinished();
```

当伴奏文件播放结束时会触发该回调。

### Token 服务即将过期回调 (onTokenPrivilegeWillExpire)

```
public void onTokenPrivilegeWillExpire(String token);
```

在调用 `joinChannel` 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中如果 Token 即将失效，SDK 会提前 30 秒触发该回调，提醒应用程序更新 Token。 当收到该回调时，用户需要重新在服务端生成新的 Token，然后调用 `renewToken` 将新生成的 Token 传给 SDK。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>token</code></td>
<td>即将服务失效的 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



## 错误代码和警告代码 - Interactive Gaming SDK

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_game.md)。

