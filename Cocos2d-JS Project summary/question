开发记录一:
swiper应用在微信中, 第一个swiper-slide中三个img是层次OK的, 从第二个切换回来就错乱了. 设置z-index也不行. 用三个div抱起来, 设置z-index.这可能是swiper的原因也可能不是.

开发记录二:
设置transform, 后再keyframes里再使用, 苹果里面有时候会冲突不生效. 使用别的属性来解决, eg: translateX 就用 top

开发记录三:
web网页播放音频, 切换或者 h5 页面 在手机上播放音频 按home 还在播放. 有需求关闭声音, 重新进入, 根据初始而定.
在微信中, 代码可以是 (或见demo案例)
``` html
var audioBg = document.getElementById('audioBg');
audioBg.play();
var _bgPlayFlag = audioBg.paused ? false : true;
document.addEventListener("WeixinJSBridgeReady", function () {
  audioBg.play();
  _bgPlayFlag = audioBg.paused ? false : true;
}, false);
// 各种浏览器兼容
var hidden, state, visibilityChange; 
if (typeof document.hidden !== "undefined") {
	hidden = "hidden";
	visibilityChange = "visibilitychange";
	state = "visibilityState";
} else if (typeof document.mozHidden !== "undefined") {
	hidden = "mozHidden";
	visibilityChange = "mozvisibilitychange";
	state = "mozVisibilityState";
} else if (typeof document.msHidden !== "undefined") {
	hidden = "msHidden";
	visibilityChange = "msvisibilitychange";
	state = "msVisibilityState";
} else if (typeof document.webkitHidden !== "undefined") {
	hidden = "webkitHidden";
	visibilityChange = "webkitvisibilitychange";
	state = "webkitVisibilityState";
}

document.addEventListener(visibilityChange, function() {
  if (document[state] == 'hidden') {
    audioBg.pause();
  } else {
    if (_bgPlayFlag) {
      audioBg.play();
    } else {
      audioBg.pause();
    }
  }
}, false);
```
