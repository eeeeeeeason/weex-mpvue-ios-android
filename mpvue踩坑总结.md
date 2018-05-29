#开发过程中遇到的坑
-   多次通过query获取的id进行渲染的部分，如商品详情，店铺列表均受缓存影响数据更新异常，我在每个页面的unload周期中进行重置

-   在mounted中打印this，iphone8p11.03报错JSON.stringify cannot serialize cyclic structure，去掉就正常了
    - vuex中缓存或者mapstate获取的值不会更新

    - 图形验证码二进制图片不能读取，要通过filedownload下载后转链接

    - input的自动聚焦需要:focus先为false再为true

    - 新版本需要删除后再扫才能使用？要加一个强制更新

    - 子组件不会触发show事件

    - onShow ,onLoad生命周期中触发this.$children的函数会找不到this.$children,转移到mounted页面生成后可以，但是如果有缓存又要另外处理

    - 5分钟内小程序后台缓存会释放，下拉刷新的功能还是非常必要的，否则你打开的页面可能短时间不会改变

    - 默认各种小程序原生自带图标是白色，如果背景色是白色，那么你可能一下子发现不了加载图消失的原因.window里配置backgroundTextStyle:'dark'

    - background-size不能继承父级，子类需要自己设置

    - 保留字：
      ```
      'template,script,style,element,content,slot,link,meta,svg,view,' +
      'a,div,img,image,text,span,richtext,input,switch,textarea,spinner,select,' +
      'slider,slider-neighbor,indicator,trisition,trisition-group,canvas,' +
      'list,cell,header,loading,loading-indicator,refresh,scrollable,scroller,' +
      'video,web,embed,tabbar,tabheader,datepicker,timepicker,marquee,countdown',
      true
      ```

    - swiper原生进行左右侧滑的列表不能自适应高度，需要每次上拉时重新计算整体高度进行赋值，导致触发下拉刷新困难，因此要么抛弃左右侧滑功能要么抛弃下拉刷新功能,6plus中flex:1失效，直接用计算高度给他进行赋值

    - watch全局vuex会在非当前页面执行，如果你拿了$mp里面的参数可能会全局报错

    - 纯js文件中不能导入store.

    - 导入的api,store等公共的js，css会重复打包，上传后不清楚有没修改呢。

-   转h5

    - 比较大的问题出现在scroll标签，tab栏切换需要做出取舍，目前是利用betterscroll生成一个scroll组件进行（值得一提的是/* eslint-disable */加载style标签上就行）