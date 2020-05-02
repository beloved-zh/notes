# 1、uni-app

官网：https://uniapp.dcloud.io/

## 1.1、介绍

`uni-app` 是一个使用 [Vue.js](https://vuejs.org/) 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉）等多个平台![uni-app-frame-0310](http://image.beloved.ink/Typora/uni-app-frame-0310.png)

## 1.2、开发工具

HBuilderX：https://www.dcloud.io/hbuilderx.html

微信开发者工具：https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

## 1.3、开发规范

为了实现多端兼容，综合考虑编译速度、运行性能等因素，`uni-app` 约定了如下开发规范：

- 页面文件遵循 [Vue 单文件组件 (SFC) 规范](https://vue-loader.vuejs.org/zh/spec.html)
- 组件标签靠近小程序规范，详见[uni-app 组件规范](https://uniapp.dcloud.io/component/README)
- 接口能力（JS API）靠近微信小程序规范，但需将前缀 `wx` 替换为 `uni`，详见[uni-app接口规范](https://uniapp.dcloud.io/api/README)
- 数据绑定及事件处理同 `Vue.js` 规范，同时补充了App及页面的生命周期
- 为兼容多端运行，建议使用flex布局进行开发

## 1.4、目录结构

一个uni-app工程，默认包含如下目录及文件：

```
┌─components            uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                存放本地网页的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，详见
└─pages.json            配置页面路由、导航条、选项卡等页面类信息，详见
    
```

**注意：**

- `static` 目录下的 `js` 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。
- `css`、`less/scss` 等资源同样不要放在 `static` 目录下，建议这些公用的资源放在 `common` 目录下。
- HbuilderX 1.9.0+ 支持在根目录创建 `ext.json` `sitemap.json` 文件。

| 有效目录  |     说明     |
| :-------: | :----------: |
| app-plus  |     App      |
|    h5     |      H5      |
| mp-weixin |  微信小程序  |
| mp-alipay | 支付宝小程序 |
| mp-baidu  |  百度小程序  |

## 1.5、资源路径说明

### 1.5.1、模板内引入静态资源

> `template`内引入静态资源，如`image`、`video`等标签的`src`属性时，可以使用相对路径或者绝对路径，形式如下

```html
<!-- 绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="/static/logo.png"></image>
<image class="logo" src="@/static/logo.png"></image>
<!-- 相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

**注意**

- `@`开头的绝对路径以及相对路径会经过base64转换规则校验
- 引入的静态资源在非h5平台，均不转为base64。
- H5平台，小于4kb的资源会被转换成base64，其余不转。
- 自`HBuilderX 2.6.6-alpha`起`template`内支持`@`开头路径引入静态资源，旧版本不支持此方式
- App平台自`HBuilderX 2.6.9-alpha`起`template`节点中引用静态资源文件时（如：图片），调整查找策略为【基于当前文件的路径搜索】，与其他平台保持一致
- 支付宝小程序组件内 image 标签不可使用相对路径

### 1.5.2、js文件引入

> `js`文件或`script`标签内（包括renderjs等）引入`js`文件时，可以使用相对路径和绝对路径，形式如下

```js
// 绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// 相对路径
import add from '../../common/add.js'
```

**注意**

- ==js文件不支持使用`/`开头的方式引入==

### 1.5.3、css引入静态资源

> `css`文件或`style标签`内引入`css`文件时（scss、less文件同理），只能使用相对路径

```css
/* 绝对路径 */
@import url('/common/uni.css');
@import url('@/common/uni.css');
/* 相对路径 */
@import url('../../common/uni.css');
```

**注意**

- 自`HBuilderX 2.6.6-alpha`起支持绝对路径引入静态资源，旧版本不支持此方式

> `css`文件或`style标签`内引用的图片路径可以使用相对路径也可以使用绝对路径，需要注意的是，有些小程序端css文件不允许引用本地文件（请看注意事项）。

```css
/* 绝对路径 */
background-image: url(/static/logo.png);
background-image: url(@/static/logo.png);
/* 相对路径 */
background-image: url(../../static/logo.png);
```

**注意**

- 引入字体图标请参考，[字体图标](https://uniapp.dcloud.io/frame?id=字体图标)
- `@`开头的绝对路径以及相对路径会经过base64转换规则校验
- 不支持本地图片的平台，小于40kb，一定会转base64。（共四个平台mp-weixin, mp-qq, mp-toutiao, app v2）
- h5平台，小于4kb会转base64，超出4kb时不转。
- 其余平台不会转base64

## 1.6、样式和sass

- 支持小程序的rpx和h5的vw、vh
- 内置了sass的配置，需要安装对应的插件
- vue组件中，在`style`标签上加入属性`<style lang=scss`

### 1.6.1、RPX

- rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

| 设备         | rpx换算px (屏幕宽度/750) | px换算rpx (750/屏幕宽度) |
| :----------- | :----------------------- | :----------------------- |
| iPhone5      | 1rpx = 0.42px            | 1px = 2.34rpx            |
| iPhone6      | 1rpx = 0.5px             | 1px = 2rpx               |
| iPhone6 Plus | 1rpx = 0.552px           | 1px = 1.81rpx            |

**建议：** 开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。

**注意：** 在较小的屏幕上不可避免的会有一些毛刺，请在开发时尽量避免这种情况。

### 1.6.2、练习代码

```vu
<template>
	<view class="content">
		<view class="rpx">rpx</view>
		<view class="vw">vw</view>
		<view class="sass">sacc</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
	.rpx{
		/* rpx 小程序中的单位 750rpx = 屏幕宽度 */
		width: 750rpx;
		height: 100rpx;
		background-color: #007AFF;
	}
	.vw{
		/* vw h5单位 100vw = 屏幕宽度 100vh = 屏幕高度 */
		width: 50vw;
		height: 50vh; 
		background-color: red;
	}
	.content{
		.sass{
			background-color: #ffff7f; 
			color: $uni-color-warning;
		}
	}
</style>

```

![](http://image.beloved.ink/Typora/image-20200414103131350.png)

# 2、基本语法

## 2.1、数据展示

- 在js的`data`中定义数据
- 在template中通过`{{数据名}}`来显示
- 在标签上通过`:data-index='数据名'`

**练习测试**

```vu
<template>
	<view>
		<!-- 2.使用数据 {{}} -->
		<view>{{msg}}</view>
		<view>{{money}}</view>
		<view>{{bool}}</view>
		<view>{{person.name}}</view>
		<view>{{person.age}}</view>
		
		<!-- 在标签上通过属性的方式来使用数据 -->
		<view :data-color="color">{{color}}</view>
	</view>
</template>

<script>
	export default {
		//1.data 定义数据
		data() {
			return {
				//字符类型
				msg:"我是提示信息",
				//数字
				money:1000,
				//布尔类型
				bool:true,
				//对象
				person:{
					name:"张三",
					age:20
				},
				color:"red"
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
```

![image-20200414111301277](http://image.beloved.ink/Typora/image-20200414111301277.png)

## 2.2、数据绑定

在`data`中定义的数据，在元素上可以使用`v-bind`或`:`绑定

```js
<script>
	export default {
		data() {
			return {
				"src":"https://www.bing.com/th?id=OHR.PinkMoon_ZH-CN9026483067_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
			}
		},
		methods: {
			
		}
	}
</script>
```

使用`v-bind`绑定

```html
<image v-bind:src="src"></image>
```

使用`:`缩写

```html
<image :src="src"></image>
```

## 2.3、数据循环

- 通过`v-for`来指定要循环的值
- `item`和`index`分别为循环项和循环索引
- `:key`指定唯一的属性，用来提高循环效率

**练习测试**

```vue
<template>
	<view>
		<view v-for="(fruit,index) in fruits" :key="fruit.id">
			{{index}}--{{fruit.id}}--{{fruit.name}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				fruits:[
					{
						id:1,
						name:"🍎"
					},
					{
						id:2,
						name:"🍌"
					},
					{
						id:3,
						name:"🍒"
					},
					{
						id:4,
						name:"🍑"
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
```

![image-20200414112822617](http://image.beloved.ink/Typora/image-20200414112822617.png)

## 2.4、条件判断

- 通过`v-if`来决定显示和隐藏，不适合做频繁的切换显示
- 通过`v-show`来决定显示和隐藏，适合做频繁的切换显示

**练习测试**

```vue
<template>
	<view>
		<view v-if="type==='A'">优秀</view>
		<view v-else-if="type==='B'">良好</view>
		<view v-else-if="type==='C'">一般</view>
		<view v-else>差</view>
		<view>======================================</view>
		<view v-if="false">v-if</view>
		<view v-show="true">v-show</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:'D'
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
```

![image-20200414113752260](http://image.beloved.ink/Typora/image-20200414113752260.png)

## 2.5、计算属性

- 可以理解为是对`data`中的数据提供了一种**加工**或者**过滤的能力**
- 通过`computed`来定义计算属性

**练习测试**

```vue
<template>
	<view>
		<view v-for="fruit in filterFruits">
			{{fruit.id}}--{{fruit.name}}
		</view>
		<view>===========================</view>
		<view v-for="fruit in updateFruits">
			{{fruit.id}}--{{fruit.name}}
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				money:1000,
				fruits:[
					{
						id:1,
						name:"🍎"
					},
					{
						id:2,
						name:"🍌"
					},
					{
						id:3,
						name:"🍒"
					},
					{
						id:4,
						name:"🍑"
					}
				]
			}
		},
		methods: {
			
		},
		//计算属性
		computed:{
			//过滤 id<2 不显示
			filterFruits(){
				return this.fruits.filter(v => v.id > 2);
			},
			
			//修改  吧updateFruits 看成data中普通的数据一样来使用
			updateFruits(){   // v 当前属性
				return this.fruits.map(v => {
					v.id = v.id+1;
					return v;
				});
			}
		}
	}
</script>

<style>

</style>
```

![image-20200414120800132](http://image.beloved.ink/Typora/image-20200414120800132.png)

# 3、事件

- 注册事件`v-on:click="事件名"`或者简写`@click="事件名"`
- 定义事件监听函数 需要在`methods`中定义
- `$event`可以传入当前对象

![image-20200414122536675](http://image.beloved.ink/Typora/image-20200414122536675.png)

**练习测试**

```vue
<template>
	<view>
		<view data-index="222" v-on:click="sayHi(1,$event)">点击我</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				message: "hello uni-app! "
			}
		},
		methods: {
			sayHi(index,event){
				console.log(this.message);
				console.log(index);
				console.log(event);
				console.log(event.currentTarget.dataset.index);
			}
		}
	}
</script>

<style>

</style>
```

![image-20200414122733195](http://image.beloved.ink/Typora/image-20200414122733195.png)

# 4、自定义组件

## 4.1、组件的使用

### 4.1.1、组件的定义

- 在根目录下新建文件夹components用来存放组件
- 在components目录下新建组件*.vue

**注意：components中的vue页面可以不用在pages.json中注册**

**img-border.vue**

```vue
<template>
	<view>
		<image class="img-border" src="https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.img-border{
		border-radius: 50%;
	}
</style>

```

### 4.1.2、组件的引入

- 在页面中引入组件`import 组件名 from '组件路径'`

### 4.1.3、组件的注册

- 在页面中的实例中，新增属性`components`
- 属性`components`是一个对象，把组件放进入注册

### 4.1.4、组件的使用

- 在页面的标签中，直接使用引入的组件`<组件名></组件名>`

```vue
<template>
	<view>
		<!-- 使用组件 -->
		<!-- <imgBorder></imgBorder> -->
		<img-border></img-border>
	</view>
</template>

<script>
	//引入组件
	import imgBorder from "@/components/img-border.vue";
	
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		},
		//注册组件
		components:{
			/* 完整写法 属性名：属性值  如果一样只写一个 */
			imgBorder
		}
	}
</script>

<style>

</style>

```

![image-20200414135034923](http://image.beloved.ink/Typora/image-20200414135034923.png)

## 4.2、组件传参

### 4.2.1、父向子传递参数

**通过属性的方法**

- 父页面向子组件通过属性名传递一个参数
- 子组件通过`props`进行接收数据

**img-border.vue**

```vue
<template>
	<view>
		<!-- 吧props中的src当成data中的变量使用 -->
		<image class="img-border" :src="src"></image>
	</view>
</template>

<script>
	export default {
		// 声明接收父组件传递来的参数的类型
		props:{
			src : String
		},
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.img-border{
		border-radius: 50%;
	}
</style>
```

**component.vue**

```vue
<template>
	<view>
		<!-- 使用组件 -->
		<!-- <imgBorder></imgBorder> -->
		<img-border v-for="src in srcs" :src="src"></img-border>
	</view>
</template>

<script>
	//引入组件
	import imgBorder from "@/components/img-border.vue";
	
	export default {
		data() {
			return {
				srcs:[
					"https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://www.bing.com/th?id=OHR.BWFlipper_ZH-CN1813139386_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://www.bing.com/th?id=OHR.SpiritSiblings_ZH-CN7023585837_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
				]
			}
		},
		methods: {
			
		},
		//注册组件
		components:{
			/* 完整写法 属性名：属性值  如果一样只写一个 */
			imgBorder
		}
	}
</script>

<style>

</style>
```

![image-20200414133540064](http://image.beloved.ink/Typora/image-20200414133540064.png)

![image-20200414135052403](http://image.beloved.ink/Typora/image-20200414135052403.png)

### 4.2.2、子向父传递参数

**通过触发事件**

- 子组件通过**触发事件**的方式向父组件传递数据
- 父组件通过**监听事件**的方式接收数据

**img-border.vue**

```vue
<template>
	<view>
		<!-- 吧props中的src当成data中的变量使用 -->
		<image @click="getSrc" class="img-border" :src="src"></image>
	</view>
</template>

<script>
	export default {
		// 声明接收父组件传递来的参数的类型
		props:{
			src : String
		},
		data() {
			return {
				
			}
		},
		methods: {
			getSrc(){
				// 子向父传递数据 通过触发事件
				//this.$emit("事件名","参数");
				this.$emit("srcChange",this.src);
			}
		}
	}
</script>

<style>
	.img-border{
		border-radius: 50%;
	}
</style>
```

**component.vue**

```vue
<template>
	<view>
		<!-- 使用组件 -->
		<!-- <imgBorder></imgBorder> -->
		<view>子组件传递过来的路径{{src}}</view>
		<img-border @srcChange="handleSrcChange" v-for="src in srcs" :src="src"></img-border>
	</view>
</template>

<script>
	//引入组件
	import imgBorder from "@/components/img-border.vue";
	
	export default {
		data() {
			return {
				src:"",
				srcs:[
					"https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://www.bing.com/th?id=OHR.BWFlipper_ZH-CN1813139386_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://www.bing.com/th?id=OHR.SpiritSiblings_ZH-CN7023585837_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
				]
			}
		},
		methods: {
			handleSrcChange(src){
				this.src = src
			}
		},
		//注册组件
		components:{
			/* 完整写法 属性名：属性值  如果一样只写一个 */
			imgBorder
		}
	}
</script>

<style>

</style>
```

![image-20200414135256609](http://image.beloved.ink/Typora/image-20200414135256609.png)

### 4.2.3、使用全局数据传递参数

#### 4.2.3.1、通过挂载vue的原型上

**有大写Vue的地方都可以**

**main.js**

```javas
//定义全局数据 通过vue原型实现
Vue.prototype.myUrl = "www.baidu.com";
```

使用

```javas
//onLoad 页面加载完毕就会触发 声明周期
onLoad() {
	console.log(this.myUrl);
}
```

![image-20200414140628242](http://image.beloved.ink/Typora/image-20200414140628242.png)

#### 4.2.3.2、通过globalData的方式（微信小程序独有的的）

**App.vue**

```vue
<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		globalData:{
			myUrl:"www.beloved.ink"
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
```

使用

```javascript
//onLoad 页面加载完毕就会触发 声明周期
onLoad() {
    console.log(this.myUrl);
    //获取globalData
    console.log(getApp().globalData.myUrl);
}
```
![image-20200414141310317](http://image.beloved.ink/Typora/image-20200414141310317.png)

## 4.3、组件插槽

- 标签也数据中的一种，想实现动态的给子组件传递标签，就可以使用插槽slot
- 通过slot来实现占位符

**my-form.vue**

```vue
<template>
	<view class="form">
		<view class="form_title">标题</view>
		<view class="from_content">
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
```

**slot.vue**

```vue
<template>
	<view>
		<my-form>
			<!-- input没有边框 -->
			<input type="text" />
			<checkbox></checkbox>
			<checkbox></checkbox>
			<radio></radio>
			<radio></radio>
		</my-form>
	</view>
</template>

<script>
	import myForm from "@/components/my-form.vue"
	
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		},
		components:{
			myForm
		}
	}
</script>

<style>

</style>
```

![image-20200414142849412](http://image.beloved.ink/Typora/image-20200414142849412.png)

![image-20200414142904955](http://image.beloved.ink/Typora/image-20200414142904955.png)

# 5、生命周期

uni-app：[https://uniapp.dcloud.io/frame?id=%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f](https://uniapp.dcloud.io/frame?id=生命周期)

vue：[https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)

微信小程序：https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html

## 5.1、介绍

- uni-app框架的生命周期结合了vue和微信小程序的生命周期
- 全局的APP中使用`onLaunch`表示应用启动时
- 页面中使用`onLoad`和`onShow`分别表示页面加载完毕时和页面显示时
- 组件中使用`mounted`组件挂载完毕时

## 5.2、应用生命周期

`uni-app` 支持如下应用生命周期函数：

| 函数名            | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| onLaunch          | 当`uni-app` 初始化完成时触发（全局只触发一次）               |
| onShow            | 当 `uni-app` 启动，或从后台进入前台显示                      |
| onHide            | 当 `uni-app` 从前台进入后台                                  |
| onError           | 当 `uni-app` 报错时触发                                      |
| onUniNViewMessage | 对 `nvue` 页面发送的数据进行监听，可参考 [nvue 向 vue 通讯](https://uniapp.dcloud.io/use-weex?id=nvue-向-vue-通讯) |

**注意**

- 应用生命周期仅可在`App.vue`中监听，在其它页面监听无效。
- onlaunch里进行页面跳转，如遇白屏报错，请参考https://ask.dcloud.net.cn/article/35942

## 5.3、页面生命周期

`uni-app` 支持如下页面生命周期函数：

| 函数名                              | 说明                                                         | 平台差异说明                                         | 最低版本 |
| :---------------------------------- | :----------------------------------------------------------- | :--------------------------------------------------- | :------- |
| onLoad                              | 监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参），参考[示例](https://uniapp.dcloud.io/api/router?id=navigateto) |                                                      |          |
| onShow                              | 监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |                                                      |          |
| onReady                             | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 |                                                      |          |
| onHide                              | 监听页面隐藏                                                 |                                                      |          |
| onUnload                            | 监听页面卸载                                                 |                                                      |          |
| onResize                            | 监听窗口尺寸变化                                             | App、微信小程序                                      |          |
| onPullDownRefresh                   | 监听用户下拉动作，一般用于下拉刷新，参考[示例](https://uniapp.dcloud.io/api/ui/pulldown) |                                                      |          |
| onReachBottom                       | 页面上拉触底事件的处理函数                                   |                                                      |          |
| onTabItemTap                        | 点击 tab 时触发，参数为Object，具体见下方注意事项            | 微信小程序、百度小程序、H5、App（自定义组件模式）    |          |
| onShareAppMessage                   | 用户点击右上角分享                                           | 微信小程序、百度小程序、字节跳动小程序、支付宝小程序 |          |
| onPageScroll                        | 监听页面滚动，参数为Object                                   |                                                      |          |
| onNavigationBarButtonTap            | 监听原生标题栏按钮点击事件，参数为Object                     | 5+ App、H5                                           |          |
| onBackPress                         | 监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack ；详细说明及使用：[onBackPress 详解](http://ask.dcloud.net.cn/article/35120) | App、H5                                              |          |
| onNavigationBarSearchInputChanged   | 监听原生标题栏搜索输入框输入内容变化事件                     | App、H5                                              | 1.6.0    |
| onNavigationBarSearchInputConfirmed | 监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发。 | App、H5                                              | 1.6.0    |
| onNavigationBarSearchInputClicked   | 监听原生标题栏搜索输入框点击事件                             | App、H5                                              | 1.6.0    |

`onPageScroll` 参数说明：

| 属性      | 类型   | 说明                                 |
| --------- | ------ | ------------------------------------ |
| scrollTop | Number | 页面在垂直方向已滚动的距离（单位px） |

`onTabItemTap` 参数说明：

| 属性     | 类型   | 说明                         |
| -------- | ------ | ---------------------------- |
| index    | String | 被点击tabItem的序号，从0开始 |
| pagePath | String | 被点击tabItem的页面路径      |
| text     | String | 被点击tabItem的按钮文字      |

**注意**

- onTabItemTap常用于点击当前tabitem，滚动或刷新当前页面。如果是点击不同的tabitem，一定会触发页面切换。
- 如果想在App端实现点击某个tabitem不跳转页面，不能使用onTabItemTap，可以使用[plus.nativeObj.view](http://www.html5plus.org/doc/zh_cn/nativeobj.html)放一个区块盖住原先的tabitem，并拦截点击事件。
- onTabItemTap在App端，从HBuilderX 1.9 的自定义组件编译模式开始支持。

`onNavigationBarButtonTap` 参数说明：

| 属性  | 类型   | 说明                     |
| ----- | ------ | ------------------------ |
| index | Number | 原生标题栏按钮数组的下标 |

`onBackPress` 回调参数对象说明：

| 属性 | 类型   | 说明                                                         |
| ---- | ------ | ------------------------------------------------------------ |
| from | String | 触发返回行为的来源：'backbutton'——左上角导航栏按钮及安卓返回键；'navigateBack'——uni.navigateBack() 方法。 |

```javascript
export default {
    data() {
        return {};
    },
    onBackPress(options) {
        console.log('from:' + options.from)
    }
}
```

**注意**

- nvue 页面支持的生命周期参考：[nvue 生命周期介绍](https://uniapp.dcloud.io/use-weex?id=生命周期)。

### 5.3.1、下拉刷新

可以在`globalStyle`中配置全局的下拉刷新

也可以在`pages`页面节点中配置开启单页面下拉刷新

#### 5.3.1.1、监听下拉刷新

**onPullDownRefreshonPullDownRefresh**

在 js 中定义 onPullDownRefresh 处理函数（和onLoad等生命周期函数同级），监听该页面用户下拉刷新事件。

- 需要在 `pages.json` 里，找到的当前页面的pages节点，并在 `style` 选项中开启 `enablePullDownRefresh`。
- 当处理完数据刷新后，`uni.stopPullDownRefresh` 可以停止当前页面的下拉刷新。

#### 5.3.1.2、事件调用下拉刷新

**uni.startPullDownRefresh(OBJECT)**

开始下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。

**OBJECT 参数说明**

| 参数名   | 类型     | 必填 | 说明                                             |
| :------- | :------- | :--- | :----------------------------------------------- |
| success  | Function | 否   | 接口调用成功的回调                               |
| fail     | Function | 否   | 接口调用失败的回调函数                           |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success 返回参数说明**

| 参数   | 类型   | 说明         |
| :----- | :----- | :----------- |
| errMsg | String | 接口调用结果 |

#### 5.3.1.3、关闭下拉刷新

**uni.stopPullDownRefresh()**

停止当前页面下拉刷新。

**实例**

`pages.json`

```json
{
    "path" : "pages/list/list",
    "style" : {
        "enablePullDownRefresh":true
    }
}
```

`list.vue`

```vue
<template>
	<view>
		<view>列表页</view>
		<view v-for="l in list">{{l}}</view>
		<button @click="pullDown">点击下拉刷新</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"list":['Java','css','html']
			}
		},
		methods: {
			pullDown(){
				uni.startPullDownRefresh();
			}
		},
		onPullDownRefresh(){
			console.log("触发了下拉刷新")
			//延迟一秒加载
			setTimeout(()=>{
				this.list.push("uni-app");
				//停止下拉刷新
				uni.stopPullDownRefresh()
			},1000);
			
		}
	}
</script>

<style>

</style>
```

### 5.3.2、页面滚动到底部触发事件

**onReachBottom**页面滚动到底部的事件（不是scroll-view滚到底），常用于下拉下一页数据。

`onReachBottom`使用注意 可在pages.json里定义具体页面底部的触发距离[onReachBottomDistance](https://uniapp.dcloud.io/collocation/pages)，比如设为50，那么滚动页面到距离底部50px时，就会触发onReachBottom事件。

`pages.json`

```json
{
    "path" : "pages/list/list",
    "style" : {
        "enablePullDownRefresh":true,
        //页面触底距离
        "onReachBottomDistance":30
    }
}
```

`list.vue`

```vue
<template>
	<view>
		<view>列表页</view>
		<view class="box-list" v-for="l in list">{{l}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"list":['Java','css','html','Python','Spring','MySQL','mybatis','Spring MVC','shiro']
			}
		},
		onReachBottom(){
			console.log("页面触底了")
			//自己拼接自己
			this.list = [...this.list,...this.list];
		}
	}
</script>

<style>
	.box-list{
		height: 100px;
		line-height: 100px;
	}
</style>

```

## 5.4、组件生命周期

**官方文档：**[https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e7%bb%84%e4%bb%b6%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=组件生命周期)

`uni-app` 组件支持的生命周期，与vue标准组件的生命周期相同。这里没有页面级的onLoad等生命周期：

| 函数名        | 说明                                                         | 平台差异说明 | 最低版本 |
| :------------ | :----------------------------------------------------------- | :----------- | :------- |
| beforeCreate  | 在实例初始化之后被调用。[详见](https://cn.vuejs.org/v2/api/#beforeCreate) |              |          |
| created       | 在实例创建完成后被立即调用。[详见](https://cn.vuejs.org/v2/api/#created) |              |          |
| beforeMount   | 在挂载开始之前被调用。[详见](https://cn.vuejs.org/v2/api/#beforeMount) |              |          |
| mounted       | 挂载到实例上去之后调用。[详见](https://cn.vuejs.org/v2/api/#mounted) 注意：此处并不能确定子组件被全部挂载，如果需要子组件完全挂载之后在执行操作可以使用`$nextTick`[Vue官方文档](https://cn.vuejs.org/v2/api/#Vue-nextTick) |              |          |
| beforeUpdate  | 数据更新时调用，发生在虚拟 DOM 打补丁之前。[详见](https://cn.vuejs.org/v2/api/#beforeUpdate) | 仅H5平台支持 |          |
| updated       | 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。[详见](https://cn.vuejs.org/v2/api/#updated) | 仅H5平台支持 |          |
| beforeDestroy | 实例销毁之前调用。在这一步，实例仍然完全可用。[详见](https://cn.vuejs.org/v2/api/#beforeDestroy) |              |          |
| destroyed     | Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。[详见](https://cn.vuejs.org/v2/api/#destroyed) |              |          |

# 6、全局配置和页面配置 

**官网文档：**https://uniapp.dcloud.io/collocation/pages

## 6.1、通过globalStyle进行全局配置

**官方文档：**https://uniapp.dcloud.io/collocation/pages?id=globalstyle

用于设置应用的状态栏、导航条、标题、窗口背景色等。

| 属性                         | 类型     | 默认值  | 描述                                                         |
| :--------------------------- | :------- | :------ | :----------------------------------------------------------- |
| navigationBarBackgroundColor | HexColor | #F7F7F7 | 导航栏背景颜色（同状态栏背景色）                             |
| navigationBarTextStyle       | String   | white   | 导航栏标题颜色及状态栏前景颜色，仅支持 black/white           |
| navigationBarTitleText       | String   |         | 导航栏标题文字内容                                           |
| backgroundColor              | HexColor | #ffffff | 下拉显示出来的窗口的背景色                                   |
| backgroundTextStyle          | String   | dark    | 下拉 loading 的样式，仅支持 dark / light                     |
| enablePullDownRefresh        | Boolean  | false   | 是否开启下拉刷新，详见[页面生命周期](https://uniapp.dcloud.io/use?id=页面生命周期)。 |
| onReachBottomDistance        | Number   | 50      | 页面上拉触底事件触发时距页面底部距离，单位只支持px，详见[页面生命周期](https://uniapp.dcloud.io/use?id=页面生命周期) |

```json
{
	"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
			"path": "pages/index/index",
			"style": {
			}
		}
	],
	"globalStyle": {
		"navigationBarTextStyle": "black",// 导航栏标题字体颜色 black（黑）/white（白）
		"navigationBarTitleText": "巴学园",// 标题内容   如果在pages中单个页面设置，会被覆盖
		"navigationBarBackgroundColor": "#aaffff",// 导航栏背景颜色
		"backgroundColor": "#ffff7f",// 下拉窗口背景颜色
		"enablePullDownRefresh":true,// 是否开启下拉
		"backgroundTextStyle":"light"// 下拉样式 dark（暗） / light（亮）
	}
}
```

![image-20200414222128272](http://image.beloved.ink/Typora/image-20200414222128272.png)

## 6.2、通过pages配置页面

**官方文档：**https://uniapp.dcloud.io/collocation/pages?id=pages

`uni-app` 通过 pages 节点配置应用由哪些页面组成，pages 节点接收一个数组，数组每个项都是一个对象，其属性值如下：

| 属性  | 类型   | 默认值 | 描述                                                         |
| :---- | :----- | :----- | :----------------------------------------------------------- |
| path  | String |        | 配置页面路径                                                 |
| style | Object |        | 配置页面窗口表现，配置项参考 [pageStyle](https://uniapp.dcloud.io/collocation/pages?id=style) |

**Tips：**

- pages节点的第一项为应用入口页（即首页）
- **应用中新增/减少页面**，都需要对 pages 数组进行修改
- 文件名**不需要写后缀**，框架会自动寻找路径下的页面资源

```json
{
	"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
		    "path" : "pages/message/message",
		    "style" : {
				// pages中设置的样式会覆盖全局样式
				"navigationBarTitleText": "信息页",
				"navigationBarBackgroundColor": "#f4eeff",
				// 只对h5平台生效
				"h5":{
					"pullToRefresh":{
						"color":"#ff007f"
					}
				}
			}
		},
		{
			"path": "pages/index/index",
			"style": {
			}
		}
    ],
	"globalStyle": {
		"navigationBarTextStyle": "black",// 导航栏标题字体颜色 black（黑）/white（白）
		"navigationBarTitleText": "巴学园",// 标题内容   如果在pages中单个页面设置，会被覆盖
		"navigationBarBackgroundColor": "#aaffff",// 导航栏背景颜色
		"backgroundColor": "#ffff7f",// 下拉窗口背景颜色
		"enablePullDownRefresh":true,// 是否开启下拉
		"backgroundTextStyle":"light"// 下拉样式 dark（暗） / light（亮）
	}
}
```

## 6.3、配置tabBar

**官方文档：**https://uniapp.dcloud.io/collocation/pages?id=tabbar

如果应用是一个多 tab 应用，可以通过 tabBar 配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页。

**Tips**

- 当设置 position 为 top 时，将不会显示 图标只能显示文字
- tabBar 中的 list 是一个数组，只能配置最少2个、最多5个 tab，tab 按数组的顺序排序。
- 顶部的 tabbar 目前仅微信小程序上支持。

**属性说明：**

| 属性            | 类型     | 必填 | 默认值 | 描述                                                         |
| :-------------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| color           | HexColor | 是   |        | tab 上的文字默认颜色                                         |
| selectedColor   | HexColor | 是   |        | tab 上的文字选中时的颜色                                     |
| backgroundColor | HexColor | 是   |        | tab 的背景色                                                 |
| borderStyle     | String   | 否   | black  | tabbar 上边框的颜色，可选值 black/white                      |
| blurEffect      | String   | 否   | none   | iOS 高斯模糊效果，可选值 dark/extralight/light/none（参考:[使用说明](https://ask.dcloud.net.cn/article/36617)） |
| list            | Array    | 是   |        | tab 的列表，详见 list 属性说明，最少2个、最多5个 tab         |
| position        | String   | 否   | bottom | 可选值 bottom、top                                           |
| fontSize        | String   | 否   | 10px   | 文字默认大小                                                 |
| iconWidth       | String   | 否   | 24px   | 图标默认宽度（高度等比例缩放）                               |
| spacing         | String   | 否   | 3px    | 图标和文字的间距                                             |
| height          | String   | 否   | 50px   | tabBar 默认高度                                              |
| midButton       | Object   | 否   |        | 中间按钮 仅在 list 项为偶数时有效                            |

其中 list 接收一个数组，数组中的每个项都是一个对象，其属性值如下：

| 属性             | 类型   | 必填 | 说明                                                         |
| :--------------- | :----- | :--- | :----------------------------------------------------------- |
| pagePath         | String | 是   | 页面路径，必须在 pages 中先定义                              |
| text             | String | 是   | tab 上按钮文字，在 App 和 H5 平台为非必填。例如中间可放一个没有文字的+号图标 |
| iconPath         | String | 否   | 图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px，当 postion 为 top 时，此参数无效，不支持网络图片，不支持字体图标 |
| selectedIconPath | String | 否   | 选中时的图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px ，当 postion 为 top 时，此参数无效 |

```json
"tabBar":{
		"color":"#ff896c",  //未选择的字体颜色
		"selectedColor":"#4CD964",  //选中的字体颜色
		"backgroundColor":"#C8C7CC",  //背景色
		"borderStyle":"white",  // 上边框颜色 black（黑）/white（白）
		"position":"bottom", //bottom、top   如果是top不能显示图标
		"list":[
			{
				"text":"首页",
				"pagePath":"pages/index/index",
				"iconPath":"static/tabs/home.png",
				"selectedIconPath":"static/tabs/home-active.png"
			},
			{
				"text":"信息",
				"pagePath":"pages/message/message",
				"iconPath":"static/tabs/message.png",
				"selectedIconPath":"static/tabs/message-active.png"
			},
			{
				"text":"我们",
				"pagePath":"pages/contact/contact",
				"iconPath":"static/tabs/contact.png",
				"selectedIconPath":"static/tabs/contact-active.png"
			}
		]
	}
```

![image-20200414232254370](http://image.beloved.ink/Typora/image-20200414232254370.png)

## 6.4、condition启动模式配置

启动模式配置，仅开发期间生效，用于模拟直达页面的场景，如：小程序转发后，用户点击所打开的页面。

**属性说明：**

| 属性    | 类型   | 是否必填 | 描述                             |
| :------ | :----- | :------- | :------------------------------- |
| current | Number | 是       | 当前激活的模式，list节点的索引值 |
| list    | Array  | 是       | 启动模式列表                     |

**list说明：**

| 属性  | 类型   | 是否必填 | 描述                                                         |
| :---- | :----- | :------- | :----------------------------------------------------------- |
| name  | String | 是       | 启动模式名称                                                 |
| path  | String | 是       | 启动页面路径                                                 |
| query | String | 否       | 启动参数，可在页面的 [onLoad](https://uniapp.dcloud.io/use?id=页面生命周期) 函数里获得 |

**注意：** 在 App 里真机运行可直接打开配置的页面，微信开发者工具里需要手动改变编译模式，如下图：

```json
"condition":{
		"current":0,
		"list":[
			{
				"name":"详情页",
				"path":"pages/detail/detail",
				"query":"id=10"
			}
		]
	}
```

![image-20200414233135527](http://image.beloved.ink/Typora/image-20200414233135527.png)

# 7、组件的基本使用

**官网文档：**https://uniapp.dcloud.io/component/README

## 7.1、什么是组件

- 组件是视图层的基本组成单元。
- 一个组件包括开始标签和结束标签，标签上可以写属性，并对属性赋值。内容则写在两个标签之内。

```html
<template>
    <view>
        <tagname property="value">
            content
        </tagname>
    </view>
</template>
```

**Tips**

- 所有组件与属性名都是小写，单词之间以连字符`-`连接。
- 根节点为 `，这个 ` 下只能且必须有一个根``组件。这是vue单文件组件规范。
- **平台差异说明**若无特殊说明，则表示所有平台均支持。

## 7.2、常用基本组件

### 7.2.1、text文本组件

**属性说明**

| 属性名     | 类型    | 默认值 | 说明         | 平台差异说明        |
| :--------- | :------ | :----- | :----------- | :------------------ |
| selectable | Boolean | false  | 文本是否可选 |                     |
| space      | String  |        | 显示连续空格 | App、H5、微信小程序 |
| decode     | Boolean | false  | 是否解码     | App、H5、微信小程序 |

**space 值说明**

| 值   | 说明                   |
| :--- | :--------------------- |
| ensp | 中文字符空格一半大小   |
| emsp | 中文字符空格大小       |
| nbsp | 根据字体设置的空格大小 |

**Tips**

- `` 组件内只支持嵌套 ``，不支持其它组件或自定义组件，否则会引发在不同平台的渲染差异。。
- decode 可以解析的有 ` &nbsp;` `&lt;` `&gt;` `&amp;` `&apos;` ` &ensp;` ` &emsp;`。分别对应 ` ` `<` `>` `&` `'` ` ` ` `。
- 各个操作系统的空格标准并不一致。
- 除了文本节点以外的其他节点都无法长按选中。
- 支持 `\n` 方式换行。
- 如果使用 `` 组件编译时会被转换为 ``。

```vue
<template>
	<view>
		<view>
			<text>我是不可选文本组件</text>
		</view>
		<view>
			<text selectable>我是可选文本组件</text>
		</view>
		<view>
			<text space="ensp">我是    文本组件</text>
		</view>
		<view>
			<text space="emsp">我是    文本组件</text>
		</view>
		<view>
			<text space="nbsp" style="font-size: 30rpx;">我是    文本组件</text>
		</view>
		<view>
			<text decode>&gt;</text>
		</view>
	</view>
</template>
```

![image-20200414235220499](http://image.beloved.ink/Typora/image-20200414235220499.png)

### 7.2.2、view视图容器组件

**属性说明**

| 属性名                 | 类型    | 默认值 | 说明                                                         |
| :--------------------- | :------ | :----- | :----------------------------------------------------------- |
| hover-class            | String  | none   | 指定按下去的样式类。当 hover-class="none" 时，没有点击态效果 |
| hover-stop-propagation | Boolean | false  | 指定是否阻止本节点的祖先节点出现点击态                       |
| hover-start-time       | Number  | 50     | 按住后多久出现点击态，单位毫秒                               |
| hover-stay-time        | Number  | 400    | 手指松开后点击态保留时间，单位毫秒                           |

**Tips**

- 如果需要使用滚动视图，请使用 [scroll-view](https://uniapp.dcloud.io/component/scroll-view)
- 如果使用 `` 组件编译时会被转换为 ``。

```vue
<template>
	<view>
		<view class="box2" hover-start-time="2000" hover-stay-time="1000" hover-class="box2-active">
			<view class="box" hover-class="box-active" hover-stop-propagation>容器view</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.box{
		width: 200rpx;
		height: 200rpx;
		background-color: #007AFF;
	}
	.box-active{
		background-color: #ff0000;
	}
	.box2{
		width: 400rpx;
		height: 400rpx;
		background-color: #ffff7f;
	}
	.box2-active{
		background-color: #aaffff;
	}
</style>
```

### 7.2.3、button组件

**官方文档：**https://uniapp.dcloud.io/component/button

**属性说明**

| 属性名      | 类型    | 默认值       | 说明                                                         |
| :---------- | :------ | :----------- | :----------------------------------------------------------- |
| size        | String  | default      | 按钮的大小                                                   |
| type        | String  | default      | 按钮的样式类型                                               |
| plain       | Boolean | false        | 按钮是否镂空，背景色透明                                     |
| disabled    | Boolean | false        | 是否禁用                                                     |
| loading     | Boolean | false        | 名称前是否带 loading 图标                                    |
| form-type   | String  |              | 用于 `` 组件，点击分别会触发 `` 组件的 submit/reset 事件     |
| hover-class | String  | button-hover | 指定按钮按下去的样式类。当 hover-class="none" 时，没有点击态效果 |

```vue
<template>
	<view>
		<button hover-class="but-active">按钮</button>
		<button size="mini">按钮</button>
		<button type="primary">按钮</button>
		<button type="warn">按钮</button>
		<button plain>按钮</button>
		<button disabled>按钮</button>
		<button loading>按钮</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.but-active{
		background-color: #ff0000;
	}
</style>
```

![image-20200415001651177](http://image.beloved.ink/Typora/image-20200415001651177.png)

### 7.2.4、image组件

| 属性名                 | 类型        | 默认值        | 说明                                                         |
| :--------------------- | :---------- | :------------ | :----------------------------------------------------------- |
| src                    | String      |               | 图片资源地址                                                 |
| mode                   | String      | 'scaleToFill' | 图片裁剪、缩放的模式                                         |
| lazy-load              | Boolean     | false         | 图片懒加载。只针对page与scroll-view下的image有效             |
| fade-show              | Boolean     | true          | 图片显示动画效果                                             |
| webp                   | boolean     | false         | 默认不解析 webP 格式，只支持网络资源                         |
| show-menu-by-longpress | boolean     | false         | 开启长按图片显示识别小程序码菜单                             |
| @error                 | HandleEvent |               | 当错误发生时，发布到 AppService 的事件名，事件对象event.detail = {errMsg: 'something wrong'} |
| @load                  | HandleEvent |               | 当图片载入完毕时，发布到 AppService 的事件名，事件对象event.detail = {height:'图片高度px', width:'图片宽度px'} |

**Tips**

- `组件默认宽度 300px、高度 225px；`app-nvue平台，暂时默认为屏幕宽度`
- `src` 仅支持相对路径、绝对路径，支持 base64 码；
- 页面结构复杂，css样式太多的情况，使用 image 可能导致样式生效较慢，出现 “闪一下” 的情况，此时设置 `image{will-change: transform}` ,可优化此问题。
- 自定义组件里面使用 ``时，若 `src` 使用相对路径可能出现路径查找失败的情况，故建议使用绝对路径。
- webp格式的图片在Android上是内置支持的。iOS上不同平台不一样，具体如下：app-vue下，iOS不支持；app-nvue下，iOS支持；微信小程序2.9.0起，iOS支持。

**mode 有效值：**

mode 有 13 种模式，其中 4 种是缩放模式，9 种是裁剪模式。

| 模式 | 值           | 说明                                                         |
| :--- | :----------- | :----------------------------------------------------------- |
| 缩放 | scaleToFill  | 不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素  |
| 缩放 | aspectFit    | 保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。 |
| 缩放 | aspectFill   | 保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取。 |
| 缩放 | widthFix     | 宽度不变，高度自动变化，保持原图宽高比不变                   |
| 缩放 | heightFix    | 高度不变，宽度自动变化，保持原图宽高比不变 **微信小程序 2.10.3 支持** |
| 裁剪 | top          | 不缩放图片，只显示图片的顶部区域                             |
| 裁剪 | bottom       | 不缩放图片，只显示图片的底部区域                             |
| 裁剪 | center       | 不缩放图片，只显示图片的中间区域                             |
| 裁剪 | left         | 不缩放图片，只显示图片的左边区域                             |
| 裁剪 | right        | 不缩放图片，只显示图片的右边区域                             |
| 裁剪 | top left     | 不缩放图片，只显示图片的左上边区域                           |
| 裁剪 | top right    | 不缩放图片，只显示图片的右上边区域                           |
| 裁剪 | bottom left  | 不缩放图片，只显示图片的左下边区域                           |
| 裁剪 | bottom right | 不缩放图片，只显示图片的右下边区域                           |

```vue
<template>
	<view>
		<!-- 默认宽度 300px、高度 225px -->
		<image show-menu-by-longpress src="https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"></image>
		<image mode="aspectFit" src="https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"></image>
		<image mode="aspectFill" src="https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"></image>
		<image mode="top" src="https://www.bing.com/th?id=OHR.EastereggsBerlin_ZH-CN7293755224_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
</style>
```

![image-20200415002809041](http://image.beloved.ink/Typora/image-20200415002809041.png)![image-20200415002827314](http://image.beloved.ink/Typora/image-20200415002827314.png)

### 7.2.5、swiper轮播图

滑块视图容器。一般用于左右滑动或上下滑动，比如banner轮播图，注意是滑动切换而不是滚动。swiper下的每个swiper-item是一个滑动切换区域，不能停留在2个滑动区域之间。

**属性说明**

| 属性名                         | 类型        | 默认值            | 说明                                                         | 平台差异说明                                                 |
| :----------------------------- | :---------- | :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| indicator-dots                 | Boolean     | false             | 是否显示面板指示点                                           |                                                              |
| indicator-color                | Color       | rgba(0, 0, 0, .3) | 指示点颜色                                                   |                                                              |
| indicator-active-color         | Color       | #000000           | 当前选中的指示点颜色                                         |                                                              |
| active-class                   | String      |                   | swiper-item 可见时的 class                                   | 支付宝小程序                                                 |
| changing-class                 | String      |                   | acceleration 设置为 {{true}} 时且处于滑动过程中，中间若干屏处于可见时的class | 支付宝小程序                                                 |
| autoplay                       | Boolean     | false             | 是否自动切换                                                 |                                                              |
| current                        | Number      | 0                 | 当前所在滑块的 index                                         |                                                              |
| current-item-id                | String      |                   | 当前所在滑块的 item-id ，不能与 current 被同时指定           | 支付宝小程序不支持                                           |
| interval                       | Number      | 5000              | 自动切换时间间隔                                             |                                                              |
| duration                       | Number      | 500               | 滑动动画时长                                                 | app-nvue不支持                                               |
| circular                       | Boolean     | false             | 是否采用衔接滑动                                             |                                                              |
| vertical                       | Boolean     | false             | 滑动方向是否为纵向                                           |                                                              |
| previous-margin                | String      | 0px               | 前边距，可用于露出前一项的一小部分，接受 px 和 rpx 值        | app-nvue、字节跳动小程序不支持                               |
| next-margin                    | String      | 0px               | 后边距，可用于露出后一项的一小部分，接受 px 和 rpx 值        | app-nvue、字节跳动小程序不支持                               |
| acceleration                   | Boolean     | false             | 当开启时，会根据滑动速度，连续滑动多屏                       | 支付宝小程序                                                 |
| disable-programmatic-animation | Boolean     | false             | 是否禁用代码变动触发 swiper 切换时使用动画。                 | 支付宝小程序                                                 |
| display-multiple-items         | Number      | 1                 | 同时显示的滑块数量                                           | app-nvue、支付宝小程序不支持                                 |
| skip-hidden-item-layout        | Boolean     | false             | 是否跳过未显示的滑块布局，设为 true 可优化复杂情况下的滑动性能，但会丢失隐藏状态滑块的布局信息 | App、微信小程序                                              |
| disable-touch                  | Boolean     | false             | 是否禁止用户 touch 操作                                      | App 2.5.5+、H5 2.5.5+、支付宝小程序、字节跳动小程序（只在初始化时有效，不能动态变更） |
| touchable                      | Boolean     | true              | 是否监听用户的触摸事件，只在初始化时有效，不能动态变更       | 字节跳动小程序（uni-app 2.5.5+ 推荐统一使用 disable-touch）  |
| easing-function                | String      | default           | 指定 swiper 切换缓动动画类型，有效值：default、linear、easeInCubic、easeOutCubic、easeInOutCubic | 微信小程序                                                   |
| @change                        | EventHandle |                   | current 改变时会触发 change 事件，event.detail = {current: current, source: source} |                                                              |
| @transition                    | EventHandle |                   | swiper-item 的位置发生改变时会触发 transition 事件，event.detail = {dx: dx, dy: dy}，支付宝小程序暂不支持dx, dy | App、H5、微信小程序、支付宝小程序、字节跳动小程序、QQ小程序  |
| @animationfinish               | EventHandle |                   | 动画结束时会触发 animationfinish 事件，event.detail = {current: current, source: source} | 字节跳动小程序不支持                                         |

change 事件返回 detail 中包含一个 source 字段，表示导致变更的原因，可能值如下：

- autoplay 自动播放导致swiper变化。
- touch 用户划动引起swiper变化。
- 其他原因将用空字符串表示。

**swiper做左右拖动的长列表问题**

- swiper是单页组件，适合做banner图轮播和简单列表左右滑动。
- 因为性能问题，用swiper做复杂长列表，需要较高的优化技巧以及接受一些限制。
- 这是一个范例，[插件市场新闻模板示例](https://ext.dcloud.net.cn/plugin?id=103)，它在App端使用了nvue的原生渲染，实现高性能的左右拖动长列表；并支持可自定义的任何形式的下拉刷新。它在非App端使用的模式是只缓存左右一共3列的数据，dom中的数据过多时，它会自动释放。就是说App上，只要看过这一页，再进去时内容是还在的。而在非App上，只能做到缓存3页数据，其他页即便看过，再进去也会重新加载。并且非App的这种情况下，不再提供下拉刷新。虽然插件市场也有其他前端模拟的下拉刷新，但性能不佳。一般小程序的大厂案例里，提供左右拖长列表的，都是这种做法。

**Tips**

- 使用竖向滚动时，需要给 `` 一个固定高度，通过 css 设置 height。
- 注意：其中只可放置 `` 组件，否则会导致未定义的行为。
- 如果遇到current、current-item-id属性设置不生效的问题参考：[组件属性设置不生效解决办法](https://uniapp.dcloud.io/use?id=常见问题)
- banner图的切换效果和指示器的样式，有多种风格可自定义，可在[uni-app插件市场](https://ext.dcloud.net.cn/search?q=轮播)搜索
- swiper在App的vue中、百度支付宝头条QQ小程序中，不支持内嵌video、map等原生组件。在微信基础库2.4.4起和App nvue2.1.5起支持内嵌原生组件。竖向的swiper内嵌视频可实现抖音、映客等视频垂直拖动切换效果。
- 同时监听 change transition，开始滑动时触发transition, 放开手后，在ios平台触发顺序为 transition... change，Android/微信小程序/支付宝为 transition... change transition...

#### swiper-item

仅可放置在 `` 组件中，宽高自动设置为100%。

| 属性名  | 类型   | 默认值 | 说明                    |
| :------ | :----- | :----- | :---------------------- |
| item-id | String |        | 该 swiper-item 的标识符 |

**示例** [查看演示](https://uniapp.dcloud.io/h5/pages/component/swiper/swiper)

#### 联系代码

```vue
<template>
	<view>
		<swiper indicator-dots autoplay circular interval="2000">
			<swiper-item v-for="image in images">
				<image :src="image" mode="aspectFill"></image>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				images : []
			}
		},
		methods: {
			
		},
		onLoad() {
			uni.request({
				url:"https://api.bailiban.com/api/admin/open/v1/api/banner",
				method:"GET",
				dataType:"json",
				success:(res)=> {
					console.log(res);
					if(res.data.status == 200){
						for(let obj of res.data.data){
							console.log(obj.imageDisplay)
							this.images.push(obj.imageDisplay)
						} 
					}
				}
			});
		}
	}
</script>

<style>

</style>
```

![image-20200415210236333](http://image.beloved.ink/Typora/image-20200415210236333.png)

# 8、uni-app样式

- rpx即响应式px，一种根据屏幕宽度自适应的动态单位，以750宽的屏幕为基准，750rpx为屏幕宽度，屏幕变宽，rpx实际显示效果会等比放大。

- 使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用；表示语句结束

- 支持基本常用的选择器class、id、element等

- 在`uni-app`中不能使用`*`选择器

- `page`相当于`body`节点

- 定义在App.vue中的样式为全局样式，作用于每一个页面。在pages目录下的vue文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖App.vue中相同的选择器

- `uni-app` 支持使用字体图标，使用方式与普通 `web` 项目相同，需要注意以下几点：

  - 支持 base64 格式字体图标。

  - 支持网络路径字体图标。

  - 小程序不支持在css中使用本地文件，包括本地的背景图和字体文件。需以base64方式方可使用。App端在v3模式以前，也有相同限制。v3编译模式起支持直接使用本地背景图和字体。

  - 网络路径必须加协议头 `https`。

  - 使用本地路径图标字体需注意：

    1. 为方便开发者，在字体文件小于 40kb 时，`uni-app` 会自动将其转化为 base64 格式；

    2. 字体文件大于等于 40kb，仍转换为 base64 方式使用的话可能有性能问题，如开发者必须使用，则需自己将其转换为 base64 格式使用，或将其挪到服务器上，从网络地址引用；

    3. 字体文件的引用路径推荐使用以 ~@ 开头的绝对路径。

       ```css
        @font-face {
            font-family: test1-icon;
            src: url('~@/static/iconfont.ttf');
        }
       ```

  `nvue`中不可直接使用css的方式引入字体文件，需要使用以下方式在js内引入。nvue内不支持本地路径引入字体，请使用网络链接或者`base64`形式。**`src`字段的`url`的括号内一定要使用单引号。**

  ```js
  var domModule = weex.requireModule('dom');
  domModule.addRule('fontFace', {
    'fontFamily': "fontFamilyName",
    'src': "url('https://...')"
  })
  ```

  在阿里巴巴矢量图标库中下载好字体图标素材https://www.iconfont.cn/

  导入以下文件到项目，demo-index.html可以查看详情

![image-20200415010910090](http://image.beloved.ink/Typora/image-20200415010910090.png)

在App.vue中引入iconfont.css文件

```css
@import url("./static/fonts/iconfont.css");
```

修改iconfont.css文件中的url，要使用`~@/绝对路径`

```css
@font-face {font-family: "iconfont";
  src: url('~@/static/fonts/iconfont.eot?t=1586883307490'); /* IE9 */
  src: url('~@/static/fonts/iconfont.eot?t=1586883307490#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('~@/static/fonts/iconfont.woff?t=1586883307490') format('woff'),
  url('~@/static/fonts/iconfont.ttf?t=1586883307490') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('~@/static/fonts/iconfont.svg?t=1586883307490#iconfont') format('svg'); /* iOS 4.1- */
}
```

在demo_index.html页面中查看对应图标的class

![image-20200415011805440](http://image.beloved.ink/Typora/image-20200415011805440.png)

使用

```vue
<view class="iconfont icon-gerenyouxiang_cebianlan_fasong"></view>
```

![image-20200415011851169](http://image.beloved.ink/Typora/image-20200415011851169.png)



# 9、网络请求uni.request

**官方文档：**https://uniapp.dcloud.io/api/request/request

> 在各个小程序平台运行时，网络相关的 API 在使用前需要配置域名白名单。

## 9.1、**OBJECT 参数说明**

| 参数名       | 类型                      | 必填 | 默认值 | 说明                                               | 平台差异说明                                   |
| :----------- | :------------------------ | :--- | :----- | :------------------------------------------------- | :--------------------------------------------- |
| url          | String                    | 是   |        | 开发者服务器接口地址                               |                                                |
| data         | Object/String/ArrayBuffer | 否   |        | 请求的参数                                         | App（自定义组件编译模式）不支持ArrayBuffer类型 |
| header       | Object                    | 否   |        | 设置请求的 header，header 中不能设置 Referer。     | H5端会自动带上cookie不可手动覆盖               |
| method       | String                    | 否   | GET    | 有效值详见下方说明                                 |                                                |
| timeout      | Number                    | 否   | 30000  | 超时时间，单位 ms                                  | 微信小程序（2.10.0）、支付宝小程序             |
| dataType     | String                    | 否   | json   | 如果设为 json，会尝试对返回的数据做一次 JSON.parse |                                                |
| responseType | String                    | 否   | text   | 设置响应的数据类型。合法值：text、arraybuffer      | App和支付宝小程序不支持                        |
| sslVerify    | Boolean                   | 否   | true   | 验证 ssl 证书                                      | 仅App安卓端支持（HBuilderX 2.3.3+）            |
| success      | Function                  | 否   |        | 收到开发者服务成功返回的回调函数                   |                                                |
| fail         | Function                  | 否   |        | 接口调用失败的回调函数                             |                                                |
| complete     | Function                  | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）   |                                                |

## 9.2、**method 有效值**

必须大写，有效值在不同平台差异说明不同。

| method  | App  |  H5  | 微信小程序 | 支付宝小程序 | 百度小程序 | 字节跳动小程序 |
| :-----: | :--: | :--: | :--------: | :----------: | :--------: | :------------: |
|   GET   |  √   |  √   |     √      |      √       |     √      |       √        |
|  POST   |  √   |  √   |     √      |      √       |     √      |       √        |
|   PUT   |  √   |  √   |     √      |      x       |     √      |       √        |
| DELETE  |  √   |  √   |     √      |      x       |     √      |       x        |
| CONNECT |  √   |  √   |     √      |      x       |     x      |       x        |
|  HEAD   |  √   |  √   |     √      |      x       |     √      |       x        |
| OPTIONS |  √   |  √   |     √      |      x       |     √      |       x        |
|  TRACE  |  √   |  √   |     √      |      x       |     x      |       x        |

## 9.3、**success 返回参数说明**

| 参数       | 类型                      | 说明                                    |
| :--------- | :------------------------ | :-------------------------------------- |
| data       | Object/String/ArrayBuffer | 开发者服务器返回的数据                  |
| statusCode | Number                    | 开发者服务器返回的 HTTP 状态码          |
| header     | Object                    | 开发者服务器返回的 HTTP Response Header |

## 9.4、**data 数据说明**

最终发送给服务器的数据是 String 类型，如果传入的 data 不是 String 类型，会被转换成 String。转换规则如下：

- 对于 `GET` 方法，会将数据转换为 query string。例如 `{ name: 'name', age: 18 }` 转换后的结果是 `name=name&age=18`。
- 对于 `POST` 方法且 `header['content-type']` 为 `application/json` 的数据，会进行 JSON 序列化。
- 对于 `POST` 方法且 `header['content-type']` 为 `application/x-www-form-urlencoded` 的数据，会将数据转换为 query string。

**示例**

```javascript
uni.request({
    url: 'https://www.example.com/request', //仅为示例，并非真实接口地址。
    data: {
        text: 'uni.request'
    },
    header: {
        'custom-header': 'hello' //自定义请求头信息
    },
    success: (res) => {
        console.log(res.data);
        this.text = 'request success';
    }
});
```

**返回值**

如果希望返回一个 `requestTask` 对象，需要至少传入 success / fail / complete 参数中的一个。例如：

```javascript
var requestTask = uni.request({
    url: 'https://www.example.com/request', //仅为示例，并非真实接口地址。
    complete: ()=> {}
});
requestTask.abort();
```

如果没有传入 success / fail / complete 参数，则会返回封装后的 Promise 对象：[Promise 封装](https://uniapp.dcloud.io/api/README?id=promise-封装)

通过 `requestTask`，可中断请求任务。

## 9.5、**requestTask 对象的方法列表**

| 方法               | 参数 | 说明                                                         |
| :----------------- | :--- | :----------------------------------------------------------- |
| abort              |      | 中断请求任务                                                 |
| offHeadersReceived |      | 取消监听 HTTP Response Header 事件，仅`微信小程序平台`支持，[文档详情](https://developers.weixin.qq.com/miniprogram/dev/api/RequestTask.offHeadersReceived.html) |
| onHeadersReceived  |      | 监听 HTTP Response Header 事件。会比请求完成事件更早，仅`微信小程序平台`支持，[文档详情](https://developers.weixin.qq.com/miniprogram/dev/api/RequestTask.onHeadersReceived.html) |

**示例**

```javascript
const requestTask = uni.request({
    url: 'https://www.example.com/request', //仅为示例，并非真实接口地址。
    data: {
        name: 'name',
        age: 18
    },
    success: function(res) {
        console.log(res.data);
    }
});

// 中断请求任务
requestTask.abort();
```

## 9.6、**Tips**

- 请求的 `header` 中 `content-type` 默认为 `application/json`。
- 网络请求的 `超时时间` 可以统一在 `manifest.json` 中配置 [networkTimeout](https://uniapp.dcloud.io/collocation/manifest?id=networktimeout)。
- H5 端本地调试需注意跨域问题，参考：[调试跨域问题解决方案](https://ask.dcloud.net.cn/article/35267)
- H5端 cookie 受跨域限制（和平时开发网站时一样），但 uni.request 目前未支持 withCredentials 配置，可以直接使用 xhr 对象或者其他类库。
- localhost、127.0.0.1等服务器地址，只能在电脑端运行，手机端连接时不能访问。请使用标准IP并保证手机能连接电脑网络
- 单次网络请求数据量建议控制在50K以下（仅指json数据，不含图片），过多数据应分页获取，以提升应用体验。

## 9.7、练习

```vue
<template>
	<view>
		<button @click="get">发送get请求</button>
		<image v-show="b" :src="src"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"b":false,
				"src":""
			}
		},
		methods: {
			get(){
				
				uni.request({
					url:"https://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1",
					method:"GET",
					dataType:"json",
					success:(res)=> {
						console.log(res);
						var src = res.data.images[0].url;
						src = "https://www.bing.com"+src;
						console.log(src);
						this.src = src;
						this.b = true;
					}
				});
			}
		}
	}
</script>

<style>

</style>
```

![image-20200415133010251](http://image.beloved.ink/Typora/image-20200415133010251.png)

# 10、数据缓存

**官方文档：**https://uniapp.dcloud.io/api/storage/storage?id=setstorage

## 10.1、uni.setStorage(OBJECT)

将数据存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个异步接口。

**OBJECT 参数说明**

| 参数名   | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| key      | String   | 是   | 本地缓存中的指定的 key                                       |
| data     | Any      | 是   | 需要存储的内容，只支持原生类型、及能够通过 JSON.stringify 序列化的对象 |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**示例**

```javascript
setStorage(){
    uni.setStorage({
        key:"name",
        data:"Beloved",
        success() {
            console.log("存储成功");
        }
    })
}
```

## 10.2、uni.setStorageSync(KEY,DATA)

将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个同步接口。

**参数说明**

| 参数 | 类型   | 必填 | 说明                                                         |
| :--- | :----- | :--- | :----------------------------------------------------------- |
| key  | String | 是   | 本地缓存中的指定的 key                                       |
| data | Any    | 是   | 需要存储的内容，只支持原生类型、及能够通过 JSON.stringify 序列化的对象 |

```javascript
setStorage(){
    uni.setStorageSync("age",20);
},
```

![image-20200415135306264](http://image.beloved.ink/Typora/image-20200415135306264.png)

## 10.3、uni.getStorage(OBJECT)

从本地缓存中异步获取指定 key 对应的内容。

**OBJECT 参数说明**

| 参数名   | 类型     | 必填 | 说明                                             |
| :------- | :------- | :--- | :----------------------------------------------- |
| key      | String   | 是   | 本地缓存中的指定的 key                           |
| success  | Function | 是   | 接口调用的回调函数，res = {data: key对应的内容}  |
| fail     | Function | 否   | 接口调用失败的回调函数                           |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success 返回参数说明**

| 参数 | 类型 | 说明           |
| :--- | :--- | :------------- |
| data | Any  | key 对应的内容 |

**示例**

```javascript
getStorage(){
    uni.getStorage({
        key:"name",
        success:function(res){
            console.log("name==="+res.data)
        }
    })
}
```

## 10.4、uni.getStorageSync(KEY)

从本地缓存中同步获取指定 key 对应的内容。

**参数说明**

| 参数 | 类型   | 必填 | 说明                   |
| :--- | :----- | :--- | :--------------------- |
| key  | String | 是   | 本地缓存中的指定的 key |

**示例**

```javascript
getStorage(){
    const age = uni.getStorageSync("age");
    console.log("age==="+age);
},
```

![image-20200415135911146](http://image.beloved.ink/Typora/image-20200415135911146.png)

## 9.5、uni.getStorageInfo(OBJECT)

异步获取当前 storage 的相关信息。

**OBJECT 参数说明**

| 参数名   | 类型     | 必填 | 说明                                             |
| :------- | :------- | :--- | :----------------------------------------------- |
| success  | Function | 是   | 接口调用的回调函数，详见返回参数说明             |
| fail     | Function | 否   | 接口调用失败的回调函数                           |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success 返回参数说明**

| 参数        | 类型            | 说明                         |
| :---------- | :-------------- | :--------------------------- |
| keys        | Array＜String＞ | 当前 storage 中所有的 key    |
| currentSize | Number          | 当前占用的空间大小, 单位：kb |
| limitSize   | Number          | 限制的空间大小, 单位：kb     |

**示例**

```javascript
getStorageInfo(){
    uni.getStorageInfo({
        success:function(res){
            console.log("当前 storage 中所有的 key==="+res.keys);
            console.log("当前占用的空间大小, 单位：kb==="+res.currentSize)
            console.log("限制的空间大小, 单位：kb==="+res.limitSize)
        }
    })
}
```

## 10.6、uni.getStorageInfoSync()

同步获取当前 storage 的相关信息。

**示例**

```javascript
getStorageInfo(){
    const res = uni.getStorageInfoSync();
    console.log(res.keys);
    console.log(res.currentSize);
    console.log(res.limitSize);
}
```

![image-20200415140546005](http://image.beloved.ink/Typora/image-20200415140546005.png)

## 10.7、uni.removeStorage(OBJECT)

从本地缓存中异步移除指定 key。

**OBJECT 参数说明**

| 参数名   | 类型     | 必填 | 说明                                             |
| :------- | :------- | :--- | :----------------------------------------------- |
| key      | String   | 是   | 本地缓存中的指定的 key                           |
| success  | Function | 是   | 接口调用的回调函数                               |
| fail     | Function | 否   | 接口调用失败的回调函数                           |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例**

```javascript
removeStorage(){
    uni.removeStorage({
        key:"name",
        success:function(){
            console.log("删除成功")
        }
    })
}
```

## 10.8、uni.removeStorageSync(KEY)

从本地缓存中同步移除指定 key。

**参数说明**

| 参数名 | 类型   | 必填 | 说明                   |
| :----- | :----- | :--- | :--------------------- |
| key    | String | 是   | 本地缓存中的指定的 key |

**示例**

```javascript
removeStorage(){
    uni.removeStorageSync("age");
}
```

## 10.9、uni.clearStorage()

清理本地数据缓存。

**示例**

```javascript
uni.clearStorage();
```

## 10.10、uni.clearStorageSync()

同步清理本地数据缓存。

**示例**

```javascript
try {
    uni.clearStorageSync();
} catch (e) {
    // error
}
```

# 11、图片的上传和预览

**官方文档：**https://uniapp.dcloud.io/api/media/image

## 11.1、图片上传

**uni.chooseImage(OBJECT)**

从本地相册选择图片或使用相机拍照。

App端如需要更丰富的相机拍照API（如直接调用前置摄像头），参考[plus.camera](https://www.html5plus.org/doc/zh_cn/camera.html)

**OBJECT 参数说明**

| 参数名     | 类型          | 必填 | 说明                                                         | 平台差异说明                              |
| :--------- | :------------ | :--- | :----------------------------------------------------------- | :---------------------------------------- |
| count      | Number        | 否   | 最多可以选择的图片张数，默认9                                | 见下方说明                                |
| sizeType   | Array<String> | 否   | original 原图，compressed 压缩图，默认二者都有               | App、微信小程序、支付宝小程序、百度小程序 |
| sourceType | Array<String> | 否   | album 从相册选图，camera 使用相机，默认二者都有。如需直接开相机或直接选相册，请只使用一个选项 |                                           |
| success    | Function      | 是   | 成功则返回图片的本地文件路径列表 tempFilePaths               |                                           |
| fail       | Function      | 否   | 接口调用失败的回调函数                                       | 小程序、App                               |
| complete   | Function      | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |                                           |

**Tips**

- count 值在 H5 平台的表现，基于浏览器本身的规范。目前测试的结果来看，只能限制单选/多选，并不能限制数量。并且，在实际的手机浏览器很少有能够支持多选的。
- sourceType 在H5端对应`input`的`capture`属性，设置为`['album']`无效，依然可以使用相机。
- 可以通过用户授权API来判断用户是否给应用授予相册或摄像头的访问权限https://uniapp.dcloud.io/api/other/authorize
- App端如需选择非媒体文件，可在插件市场搜索[文件选择](https://ext.dcloud.net.cn/search?q=文件选择)，其中Android端可以使用Native.js，无需原生插件，而iOS端需要原生插件。

**注：文件的临时路径，在应用本次启动期间可以正常使用，如需持久保存，需在主动调用 [uni.saveFile](https://uniapp.dcloud.io/api/file/file?id=savefile)，在应用下次启动时才能访问得到。**

**success 返回参数说明**

| 参数          | 类型          | 说明                                       |
| :------------ | :------------ | :----------------------------------------- |
| tempFilePaths | Array<String> | 图片的本地文件路径列表                     |
| tempFiles     | Array<Object> | 图片的本地文件列表，每一项是一个 File 对象 |

**File 对象结构如下**

| 参数 | 类型   | 说明                           |
| :--- | :----- | :----------------------------- |
| path | String | 本地文件路径                   |
| size | Number | 本地文件大小，单位：B          |
| name | String | 包含扩展名的文件名称，仅H5支持 |

**示例**

```javascript
uni.chooseImage({
    count: 6, //默认9
    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
    sourceType: ['album'], //从相册选择
    success: function (res) {
        console.log(JSON.stringify(res.tempFilePaths));
    }
});<script>
	export default {
		data() {
			return {
				imgArr : []
			}
		},
		methods: {
			chooseImg(){
				uni.chooseImage({
					count: 5, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						//将图片地址保存在数组中
						this.imgArr = res.tempFilePaths
					}
				})
			}
		}
	}
</script>
```

## 11.2、预览图片

**uni.previewImage(OBJECT)**

**OBJECT 参数说明**

| 参数名           | 类型          | 必填         | 说明                                                         | 平台差异说明 |
| :--------------- | :------------ | :----------- | :----------------------------------------------------------- | :----------- |
| current          | String/Number | 详见下方说明 | 详见下方说明                                                 |              |
| urls             | Array<String> | 是           | 需要预览的图片链接列表                                       |              |
| indicator        | String        | 否           | 图片指示器样式，可取值："default" - 底部圆点指示器； "number" - 顶部数字指示器； "none" - 不显示指示器。 | App          |
| loop             | Boolean       | 否           | 是否可循环预览，默认值为 false                               | App          |
| longPressActions | Object        | 否           | 长按图片显示操作菜单，如不填默认为**保存相册**               | App 1.9.5+   |
| success          | Function      | 否           | 接口调用成功的回调函数                                       |              |
| fail             | Function      | 否           | 接口调用失败的回调函数                                       |              |
| complete         | Function      | 否           | 接口调用结束的回调函数（调用成功、失败都会执行）             |              |

**current 参数说明**

> 1.9.5+ 支持传图片在 urls 中的索引值

current 为当前显示图片的链接/索引值，不填或填写的值无效则为 urls 的第一张。**App平台在 1.9.5至1.9.8之间，current为必填。不填会报错**

注意，当 urls 中有重复的图片链接时：

- 传链接，预览结果始终显示该链接在 urls 中第一次出现的位置。
- 传索引值，在微信/百度/字节跳动小程序平台，会过滤掉传入的 urls 中该索引值之前与其对应图片链接重复的值。其它平台会保留原始的 urls 不会做去重处理。

举例说明：

一组图片 `[A, B1, C, B2, D]`，其中 B1 与 B2 的图片链接是一样的。

- 传 B2 的链接，预览的结果是 B1，前一张是 A，下一张是 C。
- 传 B2 的索引值 3，预览的结果是 B2，前一张是 C，下一张是 D。此时在微信/百度/字节跳动小程序平台，最终传入的 urls 是 `[A, C, B2, D]`，过滤掉了与 B2 重复的 B1。

**longPressActions 参数说明**

| 参数      | 类型          | 必填 | 说明                                             |
| :-------- | :------------ | :--- | :----------------------------------------------- |
| itemList  | Array<String> | 是   | 按钮的文字数组                                   |
| itemColor | String        | 否   | 按钮的文字颜色，字符串格式，默认为"#000000"      |
| success   | Function      | 否   | 接口调用成功的回调函数，详见返回参数说明         |
| fail      | Function      | 否   | 接口调用失败的回调函数                           |
| complete  | Function      | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success 返回参数说明**

| 参数     | 类型   | 说明                     |
| :------- | :----- | :----------------------- |
| index    | Number | 用户长按图片的索引值     |
| tapIndex | Number | 用户点击按钮列表的索引值 |

**示例**

```javascript
previewImage(index){
    uni.previewImage({
        current:index,
        urls:this.imgArr
    })
}
```

## 11.3、测试

```vue
<template>
	<view>
		<view>hello uni</view>
		<button @click="chooseImg">上传图片</button>
		<image v-for="(img,index) in imgArr" :src="img" @click="previewImage(index)"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgArr : []
			}
		},
		methods: {
			previewImage(index){
				uni.previewImage({
					current:index,
					urls:this.imgArr
				})
			},
			chooseImg(){
				uni.chooseImage({
					count: 5, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						//将图片地址保存在数组中
						this.imgArr = res.tempFilePaths
					}
				})
			}
		}
	}
</script>

<style>

</style>
```

![image-20200415145019383](http://image.beloved.ink/Typora/image-20200415145019383.png)

# 12、条件注释实现跨端兼容

**官方文档：**https://uniapp.dcloud.io/platform

条件编译是用特殊的注释作为标记，在编译时根据这些特殊的注释，将注释里面的代码编译到不同平台。

**写法：**以 #ifdef 或 #ifndef 加 **%PLATFORM%** 开头，以 #endif 结尾。

- \#ifdef：if defined 仅在某平台存在
- \#ifndef：if not defined 除了某平台均存在
- **%PLATFORM%**：平台名称

| 条件编译写法                                             | 说明                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| #ifdef **APP-PLUS** 需条件编译的代码 #endif              | 仅出现在 App 平台下的代码                                    |
| #ifndef **H5** 需条件编译的代码 #endif                   | 除了 H5 平台，其它平台均存在的代码                           |
| #ifdef **H5** \|\| **MP-WEIXIN** 需条件编译的代码 #endif | 在 H5 平台或微信小程序平台存在的代码（这里只有\|\|，不可能出现&&，因为没有交集） |

**%PLATFORM%** **可取值如下：**

| 值            | 平台                                                       |
| :------------ | :--------------------------------------------------------- |
| APP-PLUS      | App                                                        |
| APP-PLUS-NVUE | App nvue                                                   |
| H5            | H5                                                         |
| MP-WEIXIN     | 微信小程序                                                 |
| MP-ALIPAY     | 支付宝小程序                                               |
| MP-BAIDU      | 百度小程序                                                 |
| MP-TOUTIAO    | 字节跳动小程序                                             |
| MP-QQ         | QQ小程序                                                   |
| MP            | 微信小程序/支付宝小程序/百度小程序/字节跳动小程序/QQ小程序 |

**支持的文件**

- .vue
- .js
- .css
- pages.json
- 各预编译语言文件，如：.scss、.less、.stylus、.ts、.pug

**注意：** 条件编译是利用注释实现的，在不同语法里注释写法不一样，js使用 `// 注释`、css 使用 `/* 注释 */`、vue/nvue 模板里使用 ``；

## 12.1、组件

```vue
<!-- #ifdef H5 -->
<view class="v_text">h5页面显示</view>
<!-- #endif -->

<!-- #ifdef MP-WEIXIN -->
<view class="v_text">微信小程序显示</view>
<!-- #endif -->
```

## 12.2、API

```js
onLoad() {
    // #ifdef H5 
    console.log("H5打印输出")
    // #endif 

    // #ifdef MP-WEIXIN
    console.log("微信小程序打印输出")
    // #endif 
}
```

## 12.3、样式

```css
<style>
	/* #ifdef H5 */
	.v_text{
		color: red;
	}
	/* #endif */
	
	
	/* #ifdef MP-WEIXIN */
	.v_text{
		color: #ffff7f;
	}
	/* #endif */
</style>
```

## 12.4、实例

![image-20200415173229614](http://image.beloved.ink/Typora/image-20200415173229614.png)



![image-20200415173239582](http://image.beloved.ink/Typora/image-20200415173239582.png)

# 13、导航跳转

## 13.1、navigator组件跳转

**官方文档：**https://uniapp.dcloud.io/component/navigator

**属性说明**

| 属性名                 | 类型    | 默认值          | 说明                                                         | 平台差异说明               |
| :--------------------- | :------ | :-------------- | :----------------------------------------------------------- | :------------------------- |
| url                    | String  |                 | 应用内的跳转链接，值为相对路径或绝对路径，如："../first/first"，"/pages/first/first"，注意不能加 `.vue` 后缀 |                            |
| open-type              | String  | navigate        | 跳转方式                                                     |                            |
| delta                  | Number  |                 | 当 open-type 为 'navigateBack' 时有效，表示回退的层数        |                            |
| animation-type         | String  | pop-in/out      | 当 open-type 为 navigate、navigateBack 时有效，窗口的显示/关闭动画效果，详见：[窗口动画](https://uniapp.dcloud.io/api/router?id=animation) | App                        |
| animation-duration     | Number  | 300             | 当 open-type 为 navigate、navigateBack 时有效，窗口显示/关闭动画的持续时间。 | App                        |
| hover-class            | String  | navigator-hover | 指定点击时的样式类，当hover-class="none"时，没有点击态效果   |                            |
| hover-stop-propagation | Boolean | false           | 指定是否阻止本节点的祖先节点出现点击态                       | 微信小程序                 |
| hover-start-time       | Number  | 50              | 按住后多久出现点击态，单位毫秒                               |                            |
| hover-stay-time        | Number  | 600             | 手指松开后点击态保留时间，单位毫秒                           |                            |
| target                 | String  | self            | 在哪个小程序目标上发生跳转，默认当前小程序，值域self/miniProgram | 微信2.0.7+、百度2.5.2+、QQ |

**open-type 有效值**

| 值           | 说明                                   | 平台差异说明                     |
| :----------- | :------------------------------------- | :------------------------------- |
| navigate     | 对应 uni.navigateTo 的功能             |                                  |
| redirect     | 对应 uni.redirectTo 的功能             |                                  |
| switchTab    | 对应 uni.switchTab 的功能              |                                  |
| reLaunch     | 对应 uni.reLaunch 的功能               | 字节跳动小程序不支持             |
| navigateBack | 对应 uni.navigateBack 的功能           |                                  |
| exit         | 退出小程序，target="miniProgram"时生效 | 微信2.1.0+、百度2.5.2+、QQ1.4.7+ |

**注意**

- navigator-hover 默认为 {background-color: rgba(0, 0, 0, 0.1); opacity: 0.7;}, `` 的子节点背景色应为透明色。**
- app-nvue 平台只有纯nvue项目（render为native）才支持 ``。非render为native的情况下，nvue暂不支持navigator组件，请使用API跳转。
- app下退出应用，Android平台可以使用[plus.runtime.quit](https://www.html5plus.org/doc/zh_cn/runtime.html#plus.runtime.quit)

- 跳转tabbar页面，必须设置open-type="switchTab"

### 实例

```vue
<!-- 普通跳转 页面不会卸载 -->
<navigator url="../api/api">跳转网络请求页</navigator>
<!-- 跳转tabBer页面需要设置open-type -->
<navigator url="../contact/contact" open-type="switchTab">跳转联系页</navigator>
<!-- 跳转页面 卸载前一个页面 -->
<navigator url="../api/api" open-type="redirect">跳转网络请求页</navigator>
```

## 13.2、编程式导航跳转API

**官方文档：**https://uniapp.dcloud.io/api/router?id=navigateto

### 13.2.1、uni.navigateTo(OBJECT)

保留当前页面，跳转到应用内的某个页面，使用`uni.navigateBack`可以返回到原页面。

**OBJECT参数说明**

| 参数              | 类型     | 必填 | 默认值 | 说明                                                         | 平台差异说明 |
| :---------------- | :------- | :--- | :----- | :----------------------------------------------------------- | :----------- |
| url               | String   | 是   |        | 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2'，path为下一个页面的路径，下一个页面的onLoad函数可得到传递的参数 | :-           |
| animationType     | String   | 否   | pop-in | 窗口显示的动画效果，详见：[窗口动画](https://uniapp.dcloud.io/api/router?id=animation) | App          |
| animationDuration | Number   | 否   | 300    | 窗口动画持续时间，单位为 ms                                  | App          |
| success           | Function | 否   |        | 接口调用成功的回调函数                                       |              |
| fail              | Function | 否   |        | 接口调用失败的回调函数                                       |              |
| complete          | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |              |

**示例**

```javascript
goApi(){
    uni.navigateTo({
        url:"../api/api"
    })
}
```

**注意：**

- 页面跳转路径有层级限制，不能无限制跳转新页面
- 跳转到 tabBar 页面只能使用 switchTab 跳转
- 路由API的目标页面必须是在pages.json里注册的vue页面。如果想打开web url，在App平台可以使用 [plus.runtime.openURL](http://www.html5plus.org/doc/zh_cn/runtime.html#plus.runtime.openURL)或web-view组件；H5平台使用 window.open；小程序平台使用web-view组件（url需在小程序的联网白名单中）。在hello uni-app中有个组件ulink.vue已对多端进行封装，可参考。

### 13.2.2、uni.redirectTo(OBJECT)

关闭当前页面，跳转到应用内的某个页面。

**OBJECT参数说明**

| 参数     | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| url      | String   | 是   | 需要跳转的应用内非 tabBar 的页面的路径，路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2' |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**示例**

```javascript
uni.redirectTo({
    url: 'test?id=1'
});
```

**注意：**

- 跳转到 tabBar 页面只能使用 switchTab 跳转

### 13.2.3、uni.reLaunch(OBJECT)

关闭所有页面，打开到应用内的某个页面。

**OBJECT参数说明**

| 参数     | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| url      | String   | 是   | 需要跳转的应用内页面路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2'，如果跳转的页面路径是 tabBar 页面则不能带参数 |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**示例**

```javascript
uni.reLaunch({
    url: 'test?id=1'
});
export default {
    onLoad: function (option) {
        console.log(option.id);
    }
}
```

Tips：

- H5端调用`uni.reLaunch`之后之前页面栈会销毁，但是无法清空浏览器之前的历史记录，此时`navigateBack`不能返回，如果存在历史记录的话点击浏览器的返回按钮或者调用`history.back()`仍然可以导航到浏览器的其他历史记录。

### 13.3.4、uni.switchTab(OBJECT)

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。

**OBJECT参数说明**

| 参数     | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| url      | String   | 是   | 需要跳转的 tabBar 页面的路径（需在 pages.json 的 tabBar 字段定义的页面），路径后不能带参数 |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**示例**

```javascript
goContact(){
    uni.switchTab({
        url:"../contact/contact"
    })
}
```

### 13.3.5、uni.navigateBack(OBJECT)

关闭当前页面，返回上一页面或多级页面。可通过 `getCurrentPages()` 获取当前的页面栈，决定需要返回几层。

**OBJECT参数说明**

| 参数              | 类型   | 必填 | 默认值  | 说明                                                         | 平台差异说明 |
| :---------------- | :----- | :--- | :------ | :----------------------------------------------------------- | :----------- |
| delta             | Number | 否   | 1       | 返回的页面数，如果 delta 大于现有页面数，则返回到首页。      |              |
| animationType     | String | 否   | pop-out | 窗口关闭的动画效果，详见：[窗口动画](https://uniapp.dcloud.io/api/router?id=animation) | App          |
| animationDuration | Number | 否   | 300     | 窗口关闭动画的持续时间，单位为 ms                            | App          |

**示例**

```javascript
// 注意：调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。见下方示例代码

// 此处是A页面
uni.navigateTo({
    url: 'B?id=1'
});

// 此处是B页面
uni.navigateTo({
    url: 'C?id=1'
});

// 在C页面内 navigateBack，将返回A页面
uni.navigateBack({
    delta: 2
});
```

Tips：

- `navigateTo`, `redirectTo` 只能打开非 tabBar 页面。
- `switchTab` 只能打开 `tabBar` 页面。
- `reLaunch` 可以打开任意页面。
- 页面底部的 `tabBar` 由页面决定，即只要是定义为 `tabBar` 的页面，底部都有 `tabBar`。
- 不能在 `App.vue` 里面进行页面跳转。
- H5端页面刷新之后页面栈会消失，此时`navigateBack`不能返回，如果一定要返回可以使用`history.back()`导航到浏览器的其他历史记录。

**参考事项**

- 页面路由拦截和管理，插件市场有很多封装好的工具类，搜索[路由](https://ext.dcloud.net.cn/search?q=路由)

## 13.3、参数传递

### 13.3.1、navigator

参数拼接在url地址后面？开始&分割

`message.vue`

```vue
<navigator url="../api/api?name=张三&age=20">跳转网络请求页</navigator>
```

`api.vue`

```js
onLoad(options) {
    console.log(options)
}
```

![image-20200415182539420](http://image.beloved.ink/Typora/image-20200415182539420.png)

### 13.3.2、Api

`message.vue`

```js
goApi(){
    uni.navigateTo({
        url:"../api/api?name=李四&age=100"
    })
},
```

`api.vue`

```js
onLoad(options) {
    console.log(options)
}
```

![image-20200415182759887](http://image.beloved.ink/Typora/image-20200415182759887.png)



























































































