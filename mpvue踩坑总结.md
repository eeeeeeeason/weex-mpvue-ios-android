# mpvue开发所遇问题及h5转化方案

- 项目结构
    |---build
        |---pages.js文件目录
    |---src
        |---component子组件
        |---pages
            |---业务页面
        |---store，vuex储存
        |---utils
            |---请求api.js
            |---format格式化插件，小程序中不能使用vue自带的格式化只能手动修改后台返回的时间戳，价格，订单状态等
            |---request封装fly进行请求响应拦截
            |---wx.js
- 环境及依赖

  - less-loader,提供嵌套样式，谁用谁知道

  - [flyio](https://github.com/wendux/fly)提供请求便于请求模块的快速转化h5(flyio提供了h5,小程序的请求封装，[参考mpvue中提供的一个例子][1]写拦截器，用于处理后台返回未登录状态跳转页面)，自己在小程序中wx.request封装也一样，只是转h5又需要做一个axios。

  - 腾讯地图[qqMap](http://lbs.qq.com/qqmap_wx_jssdk/method-reverseGeocoder.html)提供的reverseGeocoder(wx.getLocation只提供了经纬度定位，而产品需要的是确认定位后获取城市，进行同城商品检索)

  - [阿里云oss对象储存处理文件上传](https://www.jianshu.com/p/34d6dcbdc2e5)，比较意外的是腾讯对阿里云的oss域名前缀进行了封禁后台不能配置，解决方案是让后台将该域名进行服务器域名代理。提一嘴，最开始用七牛云没有出现这个问题，嫌麻烦的可以用七牛
![](https://user-gold-cdn.xitu.io/2018/6/16/164089c18cf3cf96?w=818&h=333&f=png&s=22987)
  - 富文本处理，使用mpvue例子中提供的[mpvue-wxparse][3]，当然你也可以自己写

- 小程序开发过程遇到的问题
    -    使用mpvue是一个非常爽的过程，vue的语法基本能正常使用但有几个需要注意的问题
    -    页面传递参数类似get请求?key=value，下一个页面采用$mp.query.key获取但是在同类型的页面如商品详情多次key的切换由于页面缓存key会保持不变，根据业务不同情况可能不同，我采取的方案是在onUnload中清除key，虽然mpvue文档官方不推荐用小程序的周期，但需要多次切换key的页面目前能够实现先这么处理吧。
    - 最开始仅打算做小程序，后来需要补充app，需要在开放平台关联小程序后使用unionId以使三端用户相同，没有做好产品定位，需要重新进行表结构设置。获取方法为利用wx.login获取iv，sessionkey解析encryptedData
    - 图片验证码需要带session，因此不能直接用img标签发送get请求，而是需要要通过filedownload请求地址下载二进制文件后转链接绑定给img
    - input函数触发聚焦需要先设置:focus先为false再为true进行聚焦
    - 子组件因为只会绑定一次不会触发OnShow周期，父组件在onshow周期获取获取不到this.$children
    - 小程序在关闭5分钟内不会被清除部分页面还是需要下拉刷新这个功能，可以在main.js开启enablePullDownRefresh: true，但会与scroll-view中的下拉冲突，只能二选一
    - 我们可以通过swiper嵌套scroll-view进行tab栏组件制作，swiper这个组件的高度需要用js写定高度，高度通过getSystemInfo获取，如果通过弹性布局flex:1;可能导致部分ios旧版本高度撑不开,嵌套比较多就不贴代码了
![](https://user-gold-cdn.xitu.io/2018/6/16/164089baf7ad06bd?w=489&h=454&f=png&s=44947)
    - 默认各种小程序原生自带图标是白色，如果背景色是白色，那么你可能一下子发现不了加载图消失的原因.window里配置backgroundTextStyle:'dark'
    - 小程序有不少保留字需要注意不要重复，[查看Q&A][5]
    - 前几次提交审核似乎是机器审核，代码异常也可以通过。
    - watch全局vuex会在非当前页面执行，如果你拿了$mp里面的参数可能会全局报错
---------------------------
- 转h5实践
    - 小程序与h5需要替换的大概30%页面渲染轻松，但组件替换需要花一定时间，比较复杂的包括以下vue项目常用部分，如果有一两个vue项目相信早就操作过这些部分，替换这些组件也就改改业务逻辑，采用的ui框架是接近weui的vux
      - wx这个对象我们可以结合router和vux封装一下其中的navigateTo，redirectTo等路由及模态框和toast，并在webpack.base.conf配置wx指向该文件,这样我们就能直接使用wx这个对象面的方法不用修改
      ```js
      /*webpack.base.conf*/
      resolve: {
        extensions: ['.js', '.vue', '.json'],
        alias: {
          'vue$': 'vue/dist/vue.esm.js',
          '@': resolve('src'),
          'wx': resolve('src/utils/wxSimulate.js')
        }
      },
      /*模拟wx的自己写的wxsimilate.js*/
        import router from '../router'
        import Vue from 'vue'
        import { ConfirmPlugin, ToastPlugin } from 'vux'
        Vue.use(ConfirmPlugin)
        Vue.use(ToastPlugin)
        const wx = {
          navigateTo ({ url }) {
            console.log(url)
            router.push({ path: url })
          },
          redirectTo ({url}) {
            router.replace({ path: url })
          },
          navigateBack () {
            router.go(-1)
          },
          showToast ({title}) {
            Vue.$vux.toast.show({
              // 组件除show外的属性
              text: title
            })
          },
          // 模态框显示
          showModal ({title, content, success}) {
            Vue.$vux.confirm.show({
              title,
              content,
              // 组件除show外的属性
              onConfirm () {
                success && success({confirm: true, cancel: false})
              },
              onCancel () {
                success && success({confirm: false, cancel: true})
              }
            })
          }
        }
        window.wx = wx
        export default wx

      ```
      - 地图(采用[vue-amap][6])，不多说，面向api编程
      - 上下拉加载采用[betterscroll][7]封装一个[scroll组件进行slot][8],slot文章参考点左边
      - rpx采用less+[flexible][9]中@rpx代替，只需要把所有rpx换成@rpx即可，不懂的小伙伴可以去看一下rem相关
        ```css
        /*mpvue*/
          <style scoped lang="less">
          #index {padding:100rpx 20rpx 110rpx;}
        /*vue*/
          <style scoped lang="less">
          @charset "utf-8";
          @rpx: 117.188rem;
          #index {padding:100/@rpx 20/@rpx 110/@rpx;}
        ```
      - flyio从mpvue搬过来基本不变
      - 刚才mpvue中提到的阿里云oss上传需要修改配置文件中的uploadFile为h5中的FormData进行文件上传
      - 两者的input聚焦函数业务不同，h5中ios不允许函数吊起聚焦需要用户自己手点，安卓还是可以的el.focus()进行。
     
- 最后感谢美团的各位大佬，让我这个菜鸡舒舒服服的开发了小程序、业绩也达标了，对文章有问题的大佬请指正，希望大家都能顺顺利利开开心心的开发小程序，最近看到京东出了个taro又准备开启新的填坑之路啦。


  [1]: https://github.com/F-loat/ithome-lite/blob/master/src/utils/request.js
  [2]: /img/bVbcn4M
  [3]: https://github.com/F-loat/mpvue-wxParse
  [4]: /img/bVbcoaW
  [5]: http://mpvue.com/qa/
  [6]: https://elemefe.github.io/vue-amap/
  [7]: https://ustbhuangyi.github.io/better-scroll/#/
  [8]: https://www.cnblogs.com/xiaohaifengke/p/7308943.html
  [9]: https://github.com/amfe/lib-flexible
  [10]: /img/bVbcob5
  [11]: /img/bVbcob6
