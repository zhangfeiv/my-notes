#### canvas

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
},、
```