因为webpack打包的原因,在css或html直接写文件路径无效



### 错误示范

```html
<-- html -->
<div style="background-image: url('xxx.png')"></div>

```





### 正确示范



```html
<-- html -->
<div class="imgOrder_block" :class="[doctorInfoDetail.imgTextSwitch===0?'':'order_bg_filter']" :style="{backgroundImage: 'url('+imgOrderBg+')'}" @click="toImgOrder">
</div>
```



```js
export default {
  // ...
  data () {
    // 这里存放数据
    return {
      // 在这里引入背景图
      imgOrderBg:require('@/assets/images/personal/img_order.png'),
      
    }
  }
}
```

