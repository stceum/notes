# vue router

## 二级路由配置

- 二级路由配置path时前面**不能有 ' / '**
- 在一级路径信息数组中添加一个children配置项：
```vue
       {path:'/hot',component:Hot,
             children:[
              {path:'/',redirect:'film'},                  //配置默认显示模板内容
              {path:'film',component:{template:'<p>妖猫传</p>'}},
              {path:'tv',component:{template:'<p>河神</p>'}},
              {path:'music',component:{template:'<p>缘分一道桥</p>'}}
             ]
        }
```
- 在一级路由的模板中添加`<router-link to=''></router-link>`,to中的地址需要 加上一级路由
- 供二级路由对应模板显示的`<router-view></router-view>`也不能忘！
```vue
<template id="hot">
        <div class="hotWrap">
                <h2>Hot</h2>
                <router-link to="/hot/film">film</router-link>
                <router-link to="/hot/tv">tv</router-link>
                <router-link to="/hot/music">music</router-link>
                <router-view></router-view>
        </div>
</template>
```
[原文链接](https://www.jianshu.com/p/1098daaf71ec)

## vue刷新页面404和二级以上路由刷新页面空白问题

vue刷新页面空白分两种情况：

1.在history模式下的404空白

2.在history模式下存在多级路由时，页面路由跳转是ok的，但当二级以上路由时，浏览器直接刷新页面会空白

先说解决方案：

1.在history模式下的404空白

* 后台或者nignx服务器发现404时，直接重定向到index.html就可以解决

2.在history模式下存在多级路由时，页面路由跳转是ok的，但当二级以上路由时，浏览器直接刷新页面会空白

* 如果你是老的vue项目，找到publicPath:""改成publicPath:"/"

* 如果你是最新的vue脚手架搭建的，在项目中找到这个文件node_modules/@vue/cli-service/lib/options.js，然后把里面的baseUrl:""改成baseUrl:"/"就可以了

备注：修改完配置记得重新启动下开发模式


原因：

1.在history模式下的404空白

* 这种网上已经说了很多了，你在history模式下时，你会发现这种url方式你直接回车，它是会以http get方式请求后台，但是后台没有你这个请求路径，所以会报404，而这个路径是前台自己定义的，所以要由前台处理，因为vue单页面应用，所以它只有一个页面index.html，所以一旦后台或者nignx 404了，就让它重定向到index.html，这样前台获取了这个路径，它就会根据自己路由来处理了

2.在history模式下存在多级路由时，页面路由跳转是ok的，但当二级以上路由时，浏览器直接刷新页面会空白

* 当二级以上路由刷新时，浏览器控制台只会报错误，没有具体原因，这时你看network中静态资源请求，如果你的二级路由是“/system/permisson”，这时你会发现静态资源请求路径变成了”/system/app.js“，而静态资源实际上是“/app.js”才能找到，这是因为静态资源访问路径是按照相对路径访问的，你的路由如果是“/A/B/C”，静态资源加载路径就是/A/B/app.js

* 这时候可能疑惑，我根本没设置过这个路径，那是因为webpack配置上给你设置了。

a.如果是vue老项目，你找配置文件中的publicPath这个属性，会发现是publicPath:""空字符串，所有的静态资源把publicPath路径组合起来相当于“./app.js”，这时我们需要把这里改成publicPath:"/"，所有的静态资源把publicPath路径组合起来相当于“/app.js”，从根路径开始，这样无论你在几级路由，都是从根路径加载

b.如果你是新的vue项目，你会发现没有配置publicPath的地方，因为新的vue项目publicPath是获取baseUrl字段的值，所以在这个路径下node_modules/@vue/cli-service/lib/options.js把baseUrl:""改成baseUrl:"/"


注意：hash模式下，可能需要改成相对路径，目前我没测试过。

[原文链接](https://www.jianshu.com/p/c0ee1198ee0d)