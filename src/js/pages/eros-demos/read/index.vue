<template>
	<div style="flex: 1;">
		<scroller class='page'>
			<text>{{name}}</text>
		</scroller>
		<div class="bt">
			<text @click='up' class='btl' v-if='index!=0'>上一章</text>
			<text @click='down' class='btr'>下一章</text>
		</div>
	</div>
</template>

<script>
	import 'Config'
	import { buiImage, buiButton, buiIcon, buiSliderBar, buiDropdown } from 'Eros/bui'
	import { WxcPopup, WxcMinibar } from 'Eros/weex-ui';
	var storage = weex.requireModule('bmStorage')
	export default {
		created() {
			//			this.$notice.loading.show()
			this.$router.getParams().then(resData => {
				//				console.log(resData)
				this.ids = resData.id
				//				this.loade = true
				////				this.$notice.loading.hide()
				this.getBook()

			})

		},
		data() {
			return {
				loade: false,
				show: false,
				params: {},
				booksource: [],
				ids: '',
				name: '123',
				index:0
			}
		},
		components: {
			buiImage,
			buiButton,
			buiIcon,
			buiSliderBar,
			buiDropdown,
			WxcPopup
		},
		computed: {

		},
		methods: {
			up(){
				this.index--
				
				this.getBookInfos(this.booksource[this.index].link)
			},
			down(){
				this.index++
				this.getBookInfos(this.booksource[this.index].link)
			},
			go(name) {
				this.$router.open({
					name,
					params: this.ids
				})
			},
			imgs(path) {
				if(path) {
					return 'http://devel.skylinuav.com:8005/image?url=' + decodeURI(path)
				}

			},
			getBook() {
				this.$notice.loading.show()
				this.$fetch({
					method: 'GET',
					name: 'bookindex', //当前是在apis中配置的别名，你也可以直接绝对路径请求 http://xx.xx.com/xxx/xxx
					data: {
						id: this.ids
					}
				}).then(resData => {
					// 成功回调
					//					console.log(resData)
					//					this.lists=[1,2,3,4,5]

				}, error => {
					// 错误回调
					//					console.log(error)

					this.booksource = error.chapters

					this.getBookInfos(error.chapters[0].link)

				})

			},
			getBookInfos(id) {
				this.$notice.loading.show()
				this.$fetch({
					method: 'GET',
					name: 'urlInfos', //当前是在apis中配置的别名，你也可以直接绝对路径请求 http://xx.xx.com/xxx/xxx
					data: {
						url: id
					}
				}).then(resData => {
					// 成功回调
					//					console.log(resData)
					//					this.lists=[1,2,3,4,5]

				}, error => {
					// 错误回调
					console.log(error)
					var res = error.chapter.cpContent == undefined ? error.chapter.body : error.chapter.cpContent
					this.name = res.replace(/(^\s*)|(\s*$)/g, "")
					this.$notice.loading.hide()
					this.$navigator.setCenterItem({
						text: error.chapter.title, // 展示的文字 (和图片 二选一)    
						textColor: '#000000', // 文字颜色 (默认为白色)
						fontSize: '32', // 字号（默认32px）
						fontWeight: 'normal', // 是否加粗（默认不加粗）
						//				image: '' // 展示的图片url (和文字 二选一，文字优先级更高)
					}, () => {
						// 点击回调
						this.show = true
						this.$nextTick(() => {
							this.$refs.leftSliderBar.openBar();
						})
					})

				})

			},

		}

	}
</script>

<style>
	.page {
		padding-left: 20px;
		padding-right: 10px;
		padding-top: 10px;
		padding-bottom: 10px;
	}
	.bt{
		height: 80px;
		background-color: #00CC99;
		flex-direction:row;
		align-items: center;
		justify-content: space-around;
	}
	.btl{
		color: #FFFFFF;
		border-right-width:1 ;
		border-right-style:solid ;
		border-right-color:#FFFFFF ;
		flex: 1;
		text-align: center;
	}
	.btr{
		color: #FFFFFF;
		flex: 1;
		text-align: center;
	}
</style>