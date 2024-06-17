## Mini game document in English
https://developers.weixin.qq.com/minigame/en/dev/guide/

## Main difficulties of porting the game to WeChat mini-games
#### According to the documentation of WeChat mini-games and researching with the game, the following difficulties are mainly encountered in porting the current game to WeChat mini-games:
### 1. WeChat mini-games can NOT fully support HTML5 Web APIs. The main APIs used in the game are as follows in the table:

```diff
- The following unsupported classes can NOT be used, the compatibility work needs to do
```

ID|HTML5 Web APIs Classes|Informations|Replace Class in minigame
-|-|-|-
1|AudioContext||use InnerAudioContext to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/media/audio/wx.setInnerAudioOption.html
2|Console|**Supported**|
3|URL|**Supported**|
4|WebGLRenderingContext|**Supported**|
5|Window|**Partial Supported**： <br> atob <br> cancelAnimationFrame <br> clearInterval <br> clearTimeout <br> innerHeight <br> innerWidth <br> location <br> navigator <br> onload <br> performance <br> requestAnimationFrame <br> scrollTo <br> setInterval <br> setTimeout <br> top|
6|Storage||using wx.getStorage, wx.setStorage to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/storage/wx.getStorage.html
7|WebSocket||using SocketTask to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/network/websocket/SocketTask.html
8|Worker||using wx.createWorker to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/worker/wx.createWorker.html
9|XMLHttpRequest||using wx.request to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/network/request/wx.request.html
10|XMLHttpRequestEventTarget||
11|XMLHttpRequestUpload||
12|TouchEvent system||https://developers.weixin.qq.com/minigame/en/dev/api/base/app/touch-event/wx.onTouchStart.html
13|Performance||https://developers.weixin.qq.com/minigame/en/dev/api/base/performance/wx.getPerformance.html
14|Image||using wx.createImage to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/render/image/wx.createImage.html
15|Canvas||using wx.createCanvas to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/render/canvas/wx.createCanvas.html
16|Keyboard Input system||using wx.showKeyboard, wx.onKeyboardInput … to replace <br> https://developers.weixin.qq.com/minigame/en/dev/api/ui/keyboard/wx.showKeyboard.html
17|Animation|**NOT SUPPORT**|
18|AnimationEvent|**NOT SUPPORT**|
19|Blob|**NOT SUPPORT**|
20|Cache|**NOT SUPPORT**|
21|ChannelMergerNode|**NOT SUPPORT**|
22|Client|**NOT SUPPORT**|
23|CompositionEvent|**NOT SUPPORT**|
24|CSSStyleSheet|**NOT SUPPORT**|
25|Document|**NOT SUPPORT**|
26|DOMError|**NOT SUPPORT**|
27|DOMException|**NOT SUPPORT**|
28|Element|**NOT SUPPORT**|
29|ErrorEvent|**NOT SUPPORT**|
30|Event|**NOT SUPPORT**|
31|EventSource|**NOT SUPPORT**|
32|EventTarget|**NOT SUPPORT**|
33|FileReader|**NOT SUPPORT**|
34|FontData|**NOT SUPPORT**|
35|Headers|**NOT SUPPORT**|
36|HTMLAnchorElement|**NOT SUPPORT**|
37|HTMLCanvasElement|**NOT SUPPORT**|
38|HTMLDivElement|**NOT SUPPORT**|
39|HTMLElement|**NOT SUPPORT**|
40|HTMLFormElement|**NOT SUPPORT**|
41|HTMLIFrameElement|**NOT SUPPORT**|
42|HTMLInputElement|**NOT SUPPORT**|
43|HTMLScriptElement|**NOT SUPPORT**|
44|HTMLStyleElement|**NOT SUPPORT**|
45|HTMLTextAreaElement|**NOT SUPPORT**|
46|HTMLUnknownElement|**NOT SUPPORT**|
47|IDBDatabase|**NOT SUPPORT**|
48|IDBRequest|**NOT SUPPORT**|
49|IDBTransaction|**NOT SUPPORT**|
50|ImageData|**NOT SUPPORT**|
51|IntersectionObserver|**NOT SUPPORT**|
52|KeyboardEvent|**NOT SUPPORT**|
53|LaunchParams|**NOT SUPPORT**|
54|MediaRecorder|**NOT SUPPORT**|
55|MediaStream|**NOT SUPPORT**|
56|MessageChannel|**NOT SUPPORT**|
57|MessageEvent|**NOT SUPPORT**|
58|MessagePort|**NOT SUPPORT**|
59|MouseEvent|**NOT SUPPORT**|
60|MutationObserver|**NOT SUPPORT**|
61|Navigation|**NOT SUPPORT**|
62|Node|**NOT SUPPORT**|
63|NodeList|**NOT SUPPORT**|
64|Notification|**NOT SUPPORT**|
65|Point|**NOT SUPPORT**|
66|Range|**NOT SUPPORT**|
67|Request|**NOT SUPPORT**|
68|Response|**NOT SUPPORT**|
69|Screen|**NOT SUPPORT**|
70|StyleSheet|**NOT SUPPORT**|
71|Text|**NOT SUPPORT**|
72|TextTrack|**NOT SUPPORT**|
73|TextTrackCue|**NOT SUPPORT**|
74|TextTrackList|**NOT SUPPORT**|
75|TransitionEvent|**NOT SUPPORT**|
76|URLSearchParams|**NOT SUPPORT**|
77|fetch|**NOT SUPPORT**|

### 2. WeChat mini-games do **NOT SUPPORT** BOM and DOM, so all UI system need to be rewritten, with ONE of the following two methods:

### 2.1. Rewrite the entire UI system and use Canvas to render.
- `Now all game UI using DOM to implement, so all of them should be re-writed with canvas and TouchEvent system.`

### 2.2. Re-implement a set of DOM and related CSS using pure Javascript, then use Canvas / webGL for rendering.
- `Instead of rewriting the UI system, a set of DOM and related CSS should be implemented using pure Javascript if possible, and all current UI can running on it.`

<br><br><br>

## Supports for JavaScript
#### Operation Limits
For security reasons, JS code cannot be dynamically executed in Mini Programs. In other words:
```
JS code cannot be executed via eval.
No functions can be created with new Function.
```
Details see: https://developers.weixin.qq.com/minigame/en/dev/guide/runtime/js-support.html

### File System
#### Files are divided into two categories:

1. Code package file: A file added in the project directory.
2. Local file: A file generated locally by calling an API, or downloaded over the network and stored locally.

#### Local files are divided into three types:
1. Local temporary file: A file that is temporarily generated and will be recycled at any time. The storage space of local temporary files is unlimited.
2. Local cache file: A file generated by a Mini Program after caching a local temporary file via an API. The directory and file name cannot be customized. Normal Mini Programs can store up to 10 MB of local cache and local user files, while game-type Mini Programs can store up to 50 MB.
3. Local user file: A file generated by a Mini Program after caching a local temporary file via an API. The directory and file name can be customized. Normal Mini Programs can store up to 10 MB of local user and local cache files, while game-type Mini Programs can store up to 50 MB.

Details see: https://developers.weixin.qq.com/minigame/en/dev/guide/base-ability/file-system.html

#### Subpackage Loading Size Restrictions
The sizes of Mini Game subpackages are restricted as follows:
```
The size of all sub-packages of the entire mini game does not exceed 20M
There is no limit to the size of a single sub-package, and the main package does not exceed 4M
```
Details see: https://developers.weixin.qq.com/minigame/en/dev/guide/base-ability/subPackage/useSubPackage.html

### File Types
The types of files that can be uploaded are limited for Mini Programs. Only the following file types are supported:

Details see: https://developers.weixin.qq.com/minigame/en/dev/guide/framework/code-package.html

### Rendering
#### Canvas
Mini Games only have one on-screen canvas, but can have multiple off-screen canvases. You can use wx.createCavans to create a canvas object.

*Convention: The first time you call this API, it creates an on-screen canvas. Subsequent calls create off-screen canvases.*

#### Drawing Context and API
You can use Canvas.getContext to create a drawing context. Refer to RenderingContext for the specific type of drawing context returned by this operation.

#### Locking Frame
You can use the wx.setPreferredFramesPerSecond API to perform the frame lock operation.

#### Using Compressed Textures
Compressed textures are supported from base library version 2.5.0. iOS supports the PVR format and Android supports the ETC1 format.

### Adapter
The running environment of the mini game is JavaScriptCore on iOS and V8 on Android, They are all running environments without BOM and DOM, and without global document and window objects. Therefore, when you want to use the DOM API to create elements such as Canvas and Image, an error will be raised.

Details see: https://developers.weixin.qq.com/minigame/en/dev/guide/best-practice/adapter.html

### RenderingContext
The drawing context of the canvas object.

1. You can use the Canvas.getContext('2d') API to get the CanvasRenderingContext2D object. Most of the properties and methods defined by HTML Canvas 2D Context are implemented.
2. You can use the Canvas.getContext('webgl') API to get the WebGLRenderingContext object. All the properties, methods, and constants defined by WebGL 1.0 are implemented.
Availability of 2D APIs

### 2D properties and APIs that are **NOT SUPPORT**ed on iOS/Android:

1. globalCompositeOperation does **NOT SUPPORT** the following values: source-in, source-out, destination-atop, lighter, and copy. If any of these values is used, an error is reported, and you may get different results than expected.
2. isPointInPath

###  Availability of WebGL APIs
Types of compressed textures on different platforms

1. PVR format on iOS
2. ETC1 format on Android

Details see: https://developers.weixin.qq.com/minigame/en/dev/api/render/canvas/RenderingContext.html
