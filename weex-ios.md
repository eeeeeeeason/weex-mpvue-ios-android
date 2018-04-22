# weex开发填坑之路

- 需要道具： 

  - vpn必备

  - ios环境配置（运行在手机的话需要账号）； 参考（https://zhuanlan.zhihu.com/p/25289806）

    - ios下

      ```
      npm install weex-toolkit -g
      weex create awesome-app 
      ```

    - 此时已经产生了一个目录但仅供web开发调试，

      ```
      需要weex platform add ios 添加平台环境，你会发现用xcode还打不开workspace。提示weex-sdk.h的包丢了
      ```

    - 因为缺少了一个PODs的文件夹，其中包含了这些必要的sdk，需要cocoapods

      ```
      sudo gem install -n /usr/local/bin cocoapods --pre
      注意：版本过老可能安装不了cocoapods，需要更新rvm后再更新cocoapods即可
      ```

    - 安装完毕cocoapods即可输入命令run ios时会执行pod中对应命令下载相关sdk

      ```
      weex run ios 
      ```

    - xcode打开其中的workspace文件

  - android环境（androidstudio内存杀器，反正我跑不动）；