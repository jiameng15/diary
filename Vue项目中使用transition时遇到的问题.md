### 1. 嵌套的元素,需要给每个加入对应的样式 



示例中使用 .fade-in类为嵌套元素增加动画效果

```html
<transition name="fade">
          <div class="leftSearch" v-show="ok" @click.stop>
            <div class="searchConten fade-in">
                <p class="search_item fade-in" @click="addSourceSearch">
                    <img :class="[scores?'':'right_icon_optacity','right_icon','fade-in']" src="../../assets/images/personal/right_icon.png" alt="">
                    <span :class="[scores?'text_green':'','fade-in']">评论</span>
                </p>
                <p class="search_item fade-in" @click="addServerCountSearch">
                    <img :class="[serverCount?'':'right_icon_optacity','right_icon','fade-in']" src="../../assets/images/personal/right_icon.png" alt="">
                    <span :class="[serverCount?'text_green':'','fade-in']">服务人数</span>
                </p>
            </div>
            <div class="searchReset" @click="resetList">
              重置
            </div>
          </div>
      </transition>
```





```css
.fade-enter-active{
  animation: fade-in .5s;
}
.fade-enter-active .fade-in{
  animation: fade-in .5s;
}
.fade-leave-active{
  animation: fade-in .5s reverse;
}
.fade-leave-active .fade-in{
  animation: fade-in .5s reverse;
}
@keyframes fade-in {
  0% {
    height:0px;
    opacity: 0.3  ;
  }
  100% {
  }
}
```

