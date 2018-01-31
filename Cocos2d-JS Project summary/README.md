# cocos2d-js 开发h5小游戏
## 选择cocos2d-js 开发的原因
### 情怀
第一份工作，在用cocos开发h5小游戏的公司做简单h5页面的开发，那时候还很low，也是什么都不知道，就背了背几个前端面试的题目，就进去了。还记得老总年会的时候跟我们说为什么面试选了我们几个，他说从我们身上看到了他自己的身影的感觉，喜欢学习。受他影响，我之后都很爱学习，不断的学习。正题，偶然的机会，两年后的现在，有做微信小游戏的找我，一样的需求，和我在的第一个公司一样，欣然选择了继续走这条路。
### 实用
cocos creator 工具很实用，有很多动画效果，粒子效果，都直接可以用creator工具编辑使用。之前做的那些很多效果，都得自己用代码去做，很麻烦，现在可视化的工具方便多了。
### 和微信小游戏对接
cocos creator 同步和微信小游戏对接了，里面的接口，微信都能用，在工具里面配置了公众号信息之后，可以直接打包上传，小游戏就可以跑起来。小游戏刚出来，很应景。

## 第一个项目，一个漂移的程序
没太多操作，纯粹为了活动效果。用户在屏幕上随意画出一条线，车子跟着跑起来，有一个漂移的效果。
### 难点1，漂移的效果
怎么改怎么不满意，因为本身就是很随意的点，还要去找出规律来，我实在是无计可施，就算把转折的点改的圆滑一点，也还是一样会出现各种状况，需求奇葩吧，给了玩家本身随意操作的机会。我想如果改成重力效果会很好吧。但是，为了不模仿，另外一个项目的漂移，所以换成了手画路线。以后，这种需求干脆就否定，不要有开始。
### 声音播放
在ios上，微信本身写了一些可以说是补丁的接口吧，需要在wx.ready的里paly，audio标签里可识别的音乐
安卓上，很OK
``` html
// 这里不要用新的let const 来定义变量
// 我在下一个项目里用到了, 在iso10 以下版本里的时候会报错, 卡这不动
// 这里在手机上无法预知的话, 可以用下面的js报错弹框看看错误
var cAudio = document.getElementById('cAudio');
document.addEventListener('WeixinJSBridgeReady', function() {
  cAudio.play();
})
```
### ios太变态，每次android上OK的，在iso上要另想办法，后面也遇到了相同的问题。

## js报错弹框
``` html
<script type="text/javascript">
  onerror=handleErr
  var txt=""

  function handleErr (msg,url,l) {
    txt="There was an error on this page.\n\n"
    txt+="Error: " + msg + "\n"
    txt+="URL: " + url + "\n"
    txt+="Line: " + l + "\n\n"
    txt+="Click OK to continue.\n\n"
    alert(txt)
    return true
  }
</script>
```

## 第二个项目，一个展示特效的程序
同样没太多操作，也是为了活动效果。用户看完一段视频，再看一下吐槽留言，就可以提交信息了，微信最近对推广，吸粉，收集信息的很多东西管得很严格，所以没有太多效果了，小动作的发红包还OK
### 难点1，视频播放
用cocos creator 自带的videoPlayer组件，ios的可以全屏播放，因为调用的是外部的播放器，而在android里，调用的是x5内核微信自己的浏览器，当视频播放完之后，会有一段广告。这在商用项目上肯定不行，因为播放完之后还得去下一个场景。
所以，只能用h5的播放了
``` html
// html
// playsinline 这个属性是ios 10中设置可以让视频在小窗内播放，也就是不是全屏播放
// webkit-playsinline="true" IOS微信浏览器支持小窗内播放
// x5-video-player-type 播放器类型 针对android 平台
// x5-video-player-fullscreen 全屏设置，设置为 true 是防止横屏
// x5-video-orientation 播放器横竖屏 landscape横屏，portraint竖屏，默认值为竖屏
<video id="cVideo" 
       playsinline  
       webkit-playsinline="true" 
       x5-video-player-type="h5" 
       x5-video-player-fullscreen="true" 
       x5-video-orientation="landscape" 
       preload="auto">
  <source src="./video/mov_bbb.mp4" type="video/mp4">
</video>
// css
body,html {
  position: relative;
  background: #000;
}
#cVideo {
  position: absolute;
  top: 50%;
  left: 0;
  margin: 0;
  padding: 0;
  width: 100%;
  height: auto;
  font-size: 0;
  border:none;
  object-fit: fill;
  background: #000;
  transform: translateY(-50%);
}
//设置完以上这些, 出现的效果是, android是可以全屏的, ios 是在微信内置浏览器上内置播放器横屏播放
```
### 难点2，视频播放完切回的时候，回到canvas，此时一些相对位置回错位，因为是横屏的时候给渲染了，也可能是cocos一个小bug
解决办法，在播放前就把这个场景放到住场景里一起渲染，切换的时候改变一下层次 zIndex即可。

