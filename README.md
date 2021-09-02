# 微信小程序踩坑记录
记录平日小程序开发中出现的坑和解决方法，原本记录在 [FrontendIssues](https://github.com/mrleidesen/FrontEndIssues) 中，发现太乱了就拆出来新开一个仓库

## movable-view
`movable-view` 这个组件内部无法滚动设置了 `overflow: scroll` 样式的元素（尚未解决）

[回到顶部](#微信小程序踩坑记录)

## 关于点击穿透
例子: 当你点击一个嵌套元素，你点击的是`span`的范围，但你想获取父元素`div`的内容  
```html
<div class="test" data-index="outer">
  <span data-index="inner">Click</span>
</div>
<script>
  document.querySelector('.test').onclick = function (e) {
    console.log(e.target.dataset.index);
  }
</script>
```
> 解决方法：内部元素的样式`pointer-events`修改为`none`; 或在内部放置一个位于所有子元素顶层的`div`专门用来点击;

[回到顶部](#微信小程序踩坑记录)

## uni-app在安卓中调用前置摄像头自动拍照
可见个人博客的详细内容
[uni-app安卓调用前置摄像头并自动拍照](https://mrleidesen.github.io/posts/uni_app_using_camera/)

[回到顶部](#微信小程序踩坑记录)

## 小程序 Swiper 禁止滑动
有时候小程序项目中会需要禁止 Swiper 的滑动，有两种方法

* 禁止滑动的时候直接用 view 代替展示
* 使用 `catchtouchmove`
```html
<swiper-item 
  catchtouchmove="{{needCatch ? 'handleCatchTouchMove' : ''}}"
></swiper-item>
```

```js
data: {
  needCatch: true
},

methods: {
  handleCatchTouchMove() {
    return
  }
}
```

[回到顶部](#微信小程序踩坑记录)

## openSetting问题
假如我们在 `async/await` 的方式里使用 `wx.showModal` 调用 `wx.openSetting`
```js
async onClick() {
  const { confirm } = await wx.showModal(options) // options 为你自己的参数
  
  if (confirm) {
    wx.openSetting({})
  }
}
```
会报错
```shell
openSetting:fail can only be invoked by user TAP gesture.
```
需要改回普通方式调用
```js
onClick() {
  wx.showModal({
    success: ({ confirm }) => {
      if (confirm) {
        wx.openSetting({})
      }
    }
  })
}
```

[回到顶部](#微信小程序踩坑记录)

## swiper真机圆角问题
目前发现 iOS 真机下 swiper 无法显示圆角，查找资料后加上 `transform: translateY(0);` 可行
```css
swiper {
  overflow: hidden;
  border-radius: 100%;
  transform: translateY(0);
}
```

参考：[https://developers.weixin.qq.com/community/develop/doc/00026658428810dd8c07c062556400](https://developers.weixin.qq.com/community/develop/doc/00026658428810dd8c07c062556400)

[回到顶部](#微信小程序踩坑记录)

## rotate问题
目前发现安卓真机下，`rotate: 45deg;` 会出问题不生效，建议使用 `transform: rotate(45deg);`

其他例如：`scale` 等也建议使用 `transform`

[回到顶部](#微信小程序踩坑记录)