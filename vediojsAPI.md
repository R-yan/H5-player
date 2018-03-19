# vediojsAPI
[official Doc](http://docs.videojs.com/tutorial-options.html#videojs-specific-options)

（author: Jason）

## options
|名称|类型|说明|默认值|可选值|
|:----:|:----:|:----:|:----:|:----:|
|autoplay|`boolean`|是否自动播放|false||
|controls|`boolean`|是否有控件|true||
|height|`string/number`|播放器高度|有默认值||
|width|`string/number`|播放器宽度|有默认值|
|loop|`boolean`|是否循环播放|false||
|muted|`boolean`|初始是否静音|false||
|poster|`string`|封面图片资源路径|null||
|preload|`string`|是否预加载(浏览器自动选择是否预加载/预加载视频元信息/不预加载)||`auto/metadata/none`|
|src|`string`|视频资源路径|null||

## specific options(如果不作设置均默认为`undefined`)
|名称|类型|说明|
|:----:|:----:|:----:|
|fluid|`boolean`|如果为true，播放器将自动扩展至它的容器大小，如果容器本身为fluit布局，则这个值自动为true|
|inactivityTimeout|`number`|设置认定用户已离开的时间阈值|
|notSupportedMessage|`string`|不支持视频格式时的提示信息|
|playbackRates|`Array`|设置n倍速播放，数组不能为空|
|sources|`Array`|相当于source标签，为具有src和type属性的对象数组|
|nativeAudioTracks|`boolean`|可以设置为false以禁用本机音频跟踪支持|
|nativeVideoTracks|`boolean`|可以设置为false以禁用本地视频跟踪支持|

## 实现断点续播功能
当页面刷新或组件关闭时，将`player.currentTime()`的值保存至`window.sessionStorage`中，再次打开组件时，获取到该值，然后调用`player.currentTime(获取到的值)`
* 在以下两个地方操作存值
1. `beforeRouterLeave(to,from,next){}`
2. `window.addEventListener("beforeunload",function(){})`