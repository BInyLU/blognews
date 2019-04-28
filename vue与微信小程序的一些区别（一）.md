**一.条件渲染**

vue:使用v-if指令，v-else表示v-if的else块，v-else-if表示v-if 的“else-if 块”

```
<div v-if="type === 'A'">
A
</div>
<div v-else-if="type === 'B'">
B
</div>
<div v-else-if="type === 'C'">
C
</div>
<div v-else>
Not A/B/C
</div>
```

微信小程序：使用wx:if,wx:else表示wx:if的else块，wx:elif表示wx:if的"else-if"块

```
<view wx:if="{{length > 5}}"> 1 </view>
<view wx:elif="{{length > 2}}"> 2 </view>
<view wx:else> 3 </view>
```

 

**二.显示隐藏元素**

VUE:`v-show="..."`

微信小程序：`hidden="{{...}}"`

 

**三.绑定class**

vue:全用v-bind，或者简写为:bind,和本有的class分开写

```
<div class="test" v-bind:class="{ active: isActive }"></div>
```

微信小程序：

```
<view class="test {{isActive ? 'active':'' }}"></view>
```

 

**四.事件处理**

VUE：使用v-on:event绑定事件，或者使用@event绑定事件

```
<button v-on:click="counter += 1">Add 1</button>
<button v-on:click.stop="counter+=1">Add1</button> //阻止事件冒泡
```

微信小程序：全用bindtap(bind+event)，或者catchtap(catch+event)绑定事件

```
<button bindtap="clickMe">点击我</button>
<button catchtap="clickMe">点击我</button> //阻止事件冒泡
```

 

**五.绑定值**

VUE:vue动态绑定一个变量的值为元素的某个属性的时候，会在变量前面加上冒号：，例：`<img :src="imgSrc"/>`

微信小程序：绑定某个变量的值为元素属性时，会用两个大括号括起来。例：`<image src="{{imgSrc}}"></image>`

 

**六.绑定事件传参**

VUE:vue绑定事件的函数传参数时，可以把参数写在函数后面的括号里

```
<div @click="changeTab(1)">哈哈</div>
```



微信小程序：微信小程序的事件我试过只能传函数名，至于函数值，可以绑定到元素中，在函数中获取

```
<view data-tab="1" catchtap="changeTab">哈哈</view>
js:
changeTab(e){
   var _tab = e.currentTarget.dataset.tab;  
}
```



**七.设置值**

VUE:设置test的值可以用，this.test = true;获取test的值可以用this.test.

微信小程序：设置test的值要用this.setData({test:true});获取test的值用this.data.test



**注：**微信小程序的表达式一般写在“{{}}”里面