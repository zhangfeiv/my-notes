### mescroll-ui 高性能的下拉刷新上拉加载组件

```
<template>
		<mescroll-body ref="mescrollRef" :topbar="true" @init="mescrollInit" @down="downCallback" @up="upCallback"
			:up="upOption">
			<view class="discuss-list-content">
				<view class="discuss-list">
					<view class="discuss-item" v-for="item in discussList" :key="item.id" @click="toDetail(item.id)">
						<view>{{item.title}}</view>
					</view>
				</view>
			</view>
		</mescroll-body>
	</view>
</template>

<script>
	import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js';
	export default {
		mixins: [MescrollMixin], // 使用mixin
		data() {
			return {
				discussList: [],
				siteId: '',
				upOption: {
					textNoMore: '--已经到底啦--'
				}
			};
		},
		onLoad(options) {
			this.siteId = options.siteId
		},
		onShow() {
			// 刷新
			this.mescroll.resetUpScroll(false)
		},
		methods: {
			upCallback(page) {
				let pageNum = page.num; // 页码, 默认从1开始
				let pageSize = page.size; // 页长, 默认每页10条

				this.$http.get({
					url: this.$api.forum.getDiscussList,
					data: {
						current: pageNum,
						size: pageSize,
						siteId: this.siteId
					}
				}).then(res => {
					if (res.code === 200) {
						let data = res.data.records || []
						if (pageNum == 1) {
							this.discussList = data
						} else {
							this.discussList = this.discussList.concat(data)
						}
						this.mescroll.endSuccess(data.length, res.data.total)
					}
				}).catch(() => {
					this.mescroll.endErr()
				})
			}
		}
	}
</script>


```

### 富文本图片超出宽度

```
<view class="detail-content" v-html="richtext"></view>
 
let richtext = res.data.content;
const regex = new RegExp('<img', 'gi');
richtext = richtext.replace(regex,'<img style="max-width: 100%;"');
```

