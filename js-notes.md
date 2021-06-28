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
const files = require.context("./modules", false, /\.js$/);
const modules = {};

files.keys().forEach(key => {
  console.log('key', key)
  modules[key.replace(/(\.\/|\.js)/g, "")] = files(key).default;
});

console.log('modules', JSON.stringify(modules))

export default {
  namespaced: true,
  modules
};

```

