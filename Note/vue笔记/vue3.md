---
#cli笔记
###assets(饿赛zi)：里面一般都存放静态文件，比如图片
###components(康婆闷si)：一般存放组建
###APP.vue(是一个组件)
###组件是什么：一个功能强大的标签。比如定义一个HelloWorld标签，然后输入这个标签就能实现这个效果
tag可以指定标签是什么 tag=‘div’。
:to='msg'
path:'*',redirect:''
制定跳转
this.$router.replace('/menu')
this.$router.replace({name：'menulink'})

//嵌套
+，children：[
    {path:''}
]