#### canvas 绘制圆角矩形

```jsx
/**
   * 绘制圆角矩形
   * @param {*} x 起始点x坐标
   * @param {*} y 起始点y坐标
   * @param {*} w 矩形宽
   * @param {*} h 矩形高
   * @param {*} r 圆角半径
   * @param {*} ctx 画板上下文
   */
darwRoundRect(x, y, w, h, r, ctx) {
  ctx.save()
  ctx.beginPath()

  // 左上弧线
  ctx.arc(x + r, y + r, r, 1 * Math.PI, 1.5 * Math.PI)
  // 左直线
  ctx.moveTo(x, y + r)
  ctx.lineTo(x, y + h - r)
  // 左下弧线
  ctx.arc(x + r, y + h - r, r, 0.5 * Math.PI, 1 * Math.PI)
  // 下直线
  ctx.lineTo(x + r, y + h)
  ctx.lineTo(x + w - r, y + h)
  // 右下弧线
  ctx.arc(x + w - r, y + h - r, r, 0 * Math.PI, 0.5 * Math.PI)
  // 右直线
  ctx.lineTo(x + w, y + h - r)
  ctx.lineTo(x + w, y + r)
  // 右上弧线
  ctx.arc(x + w - r, y + r, r, 1.5 * Math.PI, 2 * Math.PI)
  // 上直线
  ctx.lineTo(x + w - r, y)
  ctx.lineTo(x + r, y)

  ctx.setFillStyle('white')
  ctx.fill()
}
```

#### 防抖和节流

- ##### 防抖

```

function debounce(fn, delay) {
	let timer = null
	return function () {
		if (timer) {
			clearTimeout(timer)
		}
		timer = setTimeout(() => {
			fn.apply(this, arguments)
		}, delay)
	}
}
```

- ##### 节流

```
function throttle(fn, delay) {
	let flag = true
	return function () {
		if (flag) {
			fn.apply(this, arguments)
			flag = false
			setTimeout(() => {
				flag = true
			}, delay)
		}
	}
}
```

#### Day.js格式化时间

```
// 基础用法 
dayjs(date).format('YYYY-MM-DD HH:mm:ss')
```

#### eslint 修复格式问题

```
// 自动修复所有格式问题
npm run lint --fix
// 克隆代码时不修改回车换行
git config --global core.autocrlf false
```

#### 文档在线预览

- [百度文档服务doc](https://cloud.baidu.com/product/doc.html) 无法播放ppt动画，可以监听翻页
- [永中DCS文档](https://www.yozodcs.com/) 可以播放ppt动画，无法监听翻页
- [微软office](https://view.officeapps.live.com/op/view.aspx?src=文件地址) 可以播放ppt动画，无法监听翻页
- [Office Web 365](http://officeweb365.com/Default/Viewview) 可以播放ppt动画，可以监听翻页

#### 校验手机号码

```
const reg = /^[1][3-9][0-9]{9}$/
```

#### 引入其他文件夹中的所有文件

```
// vuex中引入modules文件夹下的所有模块
// index.js
const files = require.context("./modules", false, /\.js$/);
const modules = {};

files.keys().forEach(key => {
  console.log('key', key)
  modules[key.replace(/(\.\/|\.js)/g, "")] = files(key).default;
});

export default {
  namespaced: true,
  modules
};

// modules/home.js
export default {
	namespaced: true,
	state: {},
	mutations: {},
	actions: {}
}
```

#### iframe通信

同域情况下可直接相互修改样式、调用方法

不同域情况下可通过 postMessage 通信

```
// fatherPage.html
<body>
  <iframe id="ifr1" name="ifr1" src="/sonPage.html" width="600px" height="300px"></iframe>
  <script>
    var iframe = document.getElementById("ifr1");
    var iwindow = iframe.contentWindow // 获得子页面的window对象，等同于window.frames['ifr1'].window

    iframe.onload = function() {
      iwindow.sonFn() // 调用子页面的方法
      iwindow.document.body.style.backgroundColor = 'green' // 修改子页面的样式
      iframe.contentWindow.postMessage('MessageFromFather', '*'); //向子页面发送消息，可跨域通信
    }

    //监听来自子页面的消息，可跨域通信
    window.addEventListener("message", function () {
      console.log('receiveMessageFromSon', event)
    });

    function fatherFn() {
      console.log('fatherFn')
    }
  </script>
</body>
// sonPage.html
<body>
  <div class="son-box">son</div>
  <script>
    console.log(window.parent) // 获得父页面的window对象
    window.parent.fatherFn() // 调用父页面的方法
    window.parent.document.body.style.backgroundColor = 'pink' // 修改父页面的样式

    //监听来自父页面的消息，可跨域通信
    window.addEventListener("message", function() {
      console.log('receiveMessageFromFather', event)
    });

    //向父页面发送消息,可跨域通信
    window.parent.postMessage({ msg: 'MessageFromSon' }, '*');

    function sonFn() {
      console.log('sonFn')
    }
  </script>
</body>

```

