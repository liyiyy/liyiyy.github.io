---
title: nuxt相关问题
date: 2017-08-18 21:24:34
tags:  vue
---

#### 1、nuxt中使用element-ui 相关配置
1、首先在nuxt.config.js中配置如下：
```
vender:[
  'element-ui'
],
babel:{
  "plugins": [["component", [
    {
      "libraryName": "element-ui",
      "styleLibraryName": "theme-default"
    },
    'transform-async-to-generator',
    'transform-runtime'
  ]]],
  comments: true
},
plugins: [
{ src: '~plugins/element-ui', ssr: true }
],
css: [
// 有些说全部引用的时候需要用到,在我使用过程中不管全局引用还是部分引用都是要配置，否则样式不生效，可能哪里还没配置好，但是写了一定会有效；
// 'element-ui/lib/theme-default/index.css'
]
```
2、在plagins目录下新建element-ui.js,引入vue
```
import Vue from 'vue'

// 全部引用，此时需要在nuxt.config.js中设置css
// if (process.BROWSER_BUILD) {
//   Vue.use(require('element-ui'))
// }
```
3、在组件中使用

```
//备注：借鉴别人的写法
import { Button } from 'element-ui'
Vue.component(Button.name, Button)      

//自己的用法
<template>
<div class='box'>
     <el-pagination
    layout="prev, pager, next"
    :total="1000">
  </el-pagination>
  </div>
</template>
<script>
    import { Pagination } from 'element-ui'
    export default{
        components:{
            el-pagination:Pagination
        }
    }
</script>
```
#### 2、在组件中使用了element-ui框架,swiper插件，修改其样式无效问题
直接写一对style标签，不加scoped属性，然后用一个组件最外层的class包裹住，就不会改到所有的组件的样式了。
```
//注意：修改样式时是修改生成的标签，而不是所写的标签。
<style lang='scss'>
    .box{
        li {
            color:red;
        }
    }
</style>
```
#### 3、在nuxt使用swiper实现轮播
1、下载使用
```
npm install vue-awesome-swiper --save
```

2、在plugins文件夹下新建文件swiper.js
```
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper/ssr'

Vue.use(VueAwesomeSwiper)
```
3、在nuxt.config.js里面配置
```
module.exports = {
  // some nuxt config...
  plugins: [
    { src: '~/plugins/swiper.js', ssr: false },
  ],
  // some nuxt config...
  css: [
    'swiper/dist/css/swiper.css'
  ],
  // some nuxt config...
}
```
4、在组件内使用
```
<template>
    <div v-swiper:mySwiper="swiperOption">
        <div class="swiper-wrapper">
            <!--<div class="swiper-slide" v-for="banner in banners">
                <img :src="banner">
            </div>-->
            <div class="swiper-slide">
                <img src="~assets/images/index/jpg201682385425.jpg">
            </div>
            <div class="swiper-slide">
                <img src="~assets/images/index/jpg201692291653.jpg">
            </div>
            <div class="swiper-slide">
                <img src="~assets/images/index/jpg201732492252.jpg">
            </div>
        </div>
        <div class="swiper-pagination swiper-pagination-bullets bullets bullets-active point-style"></div>
        <div class="swiper-button-prev"></div>
        <div class="swiper-button-next"></div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                banners: [],
                swiperOption: {
                    autoplay: 3000,
                    loop: true,
                    pagination: '.swiper-pagination',
                    paginationClickable: true,
                    nextButton: '.swiper-button-next',
                    prevButton: '.swiper-button-prev',
                    effect: 'fade',
                    paginationModifierClass:'banner-'
                }
            }
        },
        mounted() {
            this.getBannerList()
        },
         methods: {
            getBannerList () {
            }
        }
    }
</script>

<style scoped>
    .swiper-button-prev {
        position: absolute;
        left: 0;
        top: 50%;
        margin-top: -25px;
        width: 45px;
        height: 60px;
        background: url(~assets/images/common/banner_e.png) 0 0 no-repeat;
        filter: alpha(opacity=50);
        opacity: 0.5;
        display: block;
    }
    
    .swiper-button-next {
        left: auto;
        right: 0;
        top: 50%;
        position: absolute;
        margin-top: -25px;
        width: 45px;
        height: 60px;
        background: url(~assets/images/common/banner_f.png) 0 0 no-repeat;
        filter: alpha(opacity=50);
        opacity: 0.5;
        display: block;
        
    }
</style>
<style lang="scss">
	.point-style{
	    .swiper-pagination-bullet{
	        width: 9px;
            height: 9px;
	        border-radius: 50%;
	        background: rgba(0,0,0,0.3);
	    }
	    .swiper-pagination-bullet-active{
	        background: #FD7045;
	        width: 9px;
	        height: 9px;
	        border-radius: 50%;
	    }
	}
</style>
```

