# webpack	

- 功能

  - 把不是标准的css转为css譬如less，loader处理
  - 把js，图片等打包成js减少http请求，
  - 模块化，

- 安装项

  - npm i webpack
  - npm i webpack-dev-server —server-dev
  - npm i style-loader —save -dev  //处理css的url去挂在对应的js
  - npm i css-loader —save -dev //标签处理

- ```
  --save 保存到package.json
  --dev 开发版本
  ```

- webpack.config.js

  ```js
  const path = require('path');
  module.exports = {
      entry:{
          entry:'./src/entry.js'，
          entry2:'./src/entry2.js'	//两个入口文件必须对应两个出口
      },
      output:{
          path:path.resolve(__dirname,'dist'), //相对本机的绝对路径
          filename:'[name].js' //[name]打包后与打包前名称相同
      },
      module:{
          rules:[
              {
                  test:/\.css$/,
                  use:['style-loader','css-loader'],
                  include:,	//特别处理哪些css
                  exclude:,
                  query:,
              }
          ]
      },	//打包模块，图片压缩分模块
      plugins:[],		//插件
      devServer:{
          contentBase:path.resolve(__dirname,'dist'),//绝对路径方便多系统开发，mac,windows相对路径可能不同
          host:'192.168.0.104',	//本地ip
          compress:true,	//服务器端压缩
          port:6008
      }
  }
  ```

  - Package.json

  ```Js
  scripts:{
      'dev':webpack-dev-server,
      devDependencies:{
          webpack:3.6,
          webpack-dev-server	//自带热更新
      }
  }
  ```

  ​
