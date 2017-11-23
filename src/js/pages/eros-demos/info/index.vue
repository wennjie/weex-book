<template>
	<div class="page" v-if="loade">
		<div class="title">
			<bui-image v-bind:src="imgs(params.cover)" width="100px" height="100px" class='img'></bui-image>
			<text>{{params.title}}</text>
		</div>
		<scroller class="infos">
			<text>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{params.shortIntro}}</text>
		</scroller>

		<div class="btns">
			<bui-button type="primary" value="选择章节" @click="go('read')"></bui-button>
		</div>
		<bui-icon name="item" size="45px" color="#000000" class="pdl10" @click=""></bui-icon>
		<wxc-popup popup-color="rgb(255, 200, 150)" :show="show" @wxcPopupOverlayClicked="show=false" pos="left" width="400">
			<scroller>
				<div v-for="(n,index) in booksource" class="item" @click="choose(n,index)" :class="{ck:n.source==true}">
					<div class="top">
						<text class='tl'>{{n.chaptersCount}}</text>
						<text class='tr'>{{n.name}}</text>
					</div>
					<div class="bottom">
						<text class='b'>{{(n.lastChapter).slice(0,12)}}...</text>
					</div>
					
					
					
				</div>
			</scroller>
		</wxc-popup>
	</div>
</template>

<script>
	import 'Config'
	import { buiImage, buiButton, buiIcon, buiSliderBar, buiDropdown } from 'Eros/bui'
	import { WxcPopup,WxcMinibar } from 'Eros/weex-ui';
	export default {
		created() {
			this.$notice.loading.show()
			this.$router.getParams().then(resData => {
				console.log(resData)
				this.params = resData
				this.loade = true
				this.$notice.loading.hide()
				this.getBookSource()
				
				
			})
			this.$navigator.setRightItem({
				text: '书源', // 展示的文字 (和图片 二选一)    
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

		},
		data() {
			return {
				loade: false,
				show: false,
				params: {},
				booksource:[],
				ids:''
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
			choose(n,index){
				
				this.show = false
				this.ids=n._id
			},
			popupOverlayBottomClick(){
				
			},
			go(name) {
				console.log(this.ids)
				this.$router.open({
					name,
					params:{id:this.ids}
				})
			},
			imgs(path) {
				if(path) {
					return 'http://devel.skylinuav.com:8005/image?url=' + decodeURI(path)
				}

			},
			getBookSource() {
				this.$fetch({
					method: 'GET',
					name: 'source', //当前是在apis中配置的别名，你也可以直接绝对路径请求 http://xx.xx.com/xxx/xxx
					data: {
						id: this.params._id
					}
				}).then(resData => {
					// 成功回调
					console.log(resData)
					//					this.lists=[1,2,3,4,5]

				}, error => {
					// 错误回调
					console.log(error)
					
					this.booksource=error
					this.ids=error[0]._id
					
					
					

				})

			},

		}

	}
</script>

<style>
	.page {
		flex: 1;
		/*padding-top: 20px;*/
		
	}
	
	.title {
		flex-direction: row;
		align-items: center;
		border-bottom-color: #E5E5E5;
		border-bottom-style: solid;
		border-bottom-width: 1;
		justify-content: center;
		padding-top: 10px;
		padding-bottom: 10px;
		margin-bottom: 30px;
	}
	
	.img {
		margin-right: 30px;
	}
	
	.pdl10 {
		position: fixed;
		right: 0;
		bottom: 10px;
	}
	.infos{
		padding-left: 20px;
		padding-right: 20px;
	}
	.item{
		flex-direction: column;
		height: 100px;
		align-items: center;
		border-bottom-color:#E5E5E5 ;
		border-bottom-style:solid ;
		border-bottom-width:1 ;
		overflow: hidden;
		width: 400px;
		padding-left: 20px;
		padding-right: 20px;
	}
	.top{
		flex-direction: row;
		justify-content: flex-start;
		
		flex: 1;
		width: 400px;
	}
	.tr{
		flex: 1;
		padding-left: 30px;
	}
	.b{
		overflow: hidden;
		height: 50px;
		width: 400px;
	}
	.ck{
		background-color: #FFFFFF;
	}
	text{
		font-size:14px ;
	}
	
</style>