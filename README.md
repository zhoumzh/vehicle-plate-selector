# vehicle-plate-selector
车辆号牌输入组件【车辆号牌选择器】，基于vue实现，主要适用于pc端

src文件夹下面是vue的源码

如果只是使用的话，直接在你的html页面上引入dist下面的css和js即可

使用方法
  引入我的js和css后直接用我的自定义标签
  
  
  <vehicle-plate-selector v-model="carNo"></vehicle-plate-selector>
  
  v-mode就是车牌输入的结果。
  
  目前本组件只实现了单项绑定，即输入的车牌号会绑定到你的model变量上，反之mode变量更改后并不会反应到控件上。
  
  后续在优化吧，除非有感兴趣的小伙伴，可以拉我的源码把双向的绑定实现了，提交合并请求了！
