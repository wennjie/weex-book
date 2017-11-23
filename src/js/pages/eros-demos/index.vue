<template>
	<div class="page">
		<bui-header title="看书软件">
			<!-- <bui-icon slot="left" name="left" size="45px" color="#ffffff" class="pdl10"></bui-icon> -->
			<bui-icon slot="right" name="setting" size="45px" color="#ffffff" class="pdl10" @click="go('index',1)"></bui-icon>
		</bui-header>
		<wxc-searchbar ref="wxc-searchbar" v-on:searchbarCancelClick="searchbarCancelClick" v-on:searchbarInputOninput="searchbarInputOninput" v-on:searchbarCloseClick="searchbarCloseClick" v-on:searchbarInputOnfocus="searchbarInputOnfocus" v-on:searchbarInputOnblur="searchbarInputOnblur"></wxc-searchbar>
		<list class="container" style='background-color: #E5E3DF;'>
			<cell class="cell" :key='index' v-for="(n,index) in lists" style="color:#ffffff" @click='go("info",n)'>
				<bui-image v-bind:src="imgs(n.cover)" width="100px" height="100px" class='img'></bui-image>
				<div class="c">
					<text style="color;#ffffff">{{n.title}}</text>
					<text class='line'>{{n.shortIntro.slice(0,10)}}...</text>
				</div>
				<div class="r">
					<bui-icon name="right" size="45px" color="#999999" class="pdl10"></bui-icon>
				</div>
			</cell>
		</list>

	</div>
</template>
<style lang="sass">
	.page {
		flex: 1;
	}
	
	.cell {
		flex-direction: row;
		height: 120px;
		align-items: center;
		justify-content: center;
		padding-left: 20px;
		padding-right: 20px;
		background-color: #FFFFFF;
		border-bottom-width: 1;
		border-bottom-color: #E5E5E5;
		border-bottom-style: solid;
	}
	
	.line {
		overflow: hidden;
		height: 40px;
	}
	
	.c {
		flex: 1;
		padding-left: 40px;
	}
</style>

<script>
	import 'Config'
	import { buiHeader, buiIcon, buiImage } from 'Eros/bui'
	import { WxcSearchbar } from 'Eros/weex-ui'
	const modal = weex.requireModule('modal');

	export default {
		globalEvent: {
			appActive() {
				console.log('active')
			},
			appDeactive() {
				console.log('deactive')
			}
		},
		created() {
			console.log('#######', weex.config.eros)
			//			this.fetch()
		},
		data() {
			return {
				name: '',
				password: '',
				lists: [],
				val: ''
			}
		},
		components: {
			buiHeader,
			buiIcon,
			WxcSearchbar,
			buiImage
		},
		computed: {

		},
		methods: {
			go(name,params) {
				this.$router.open({
					name,
					params
				})
			},
			imgs(path) {
				return 'http://devel.skylinuav.com:8005/image?url=' + decodeURI(path)
			},
			fetch() {
				this.$fetch({
					method: 'GET',
					url: 'search', //当前是在apis中配置的别名，你也可以直接绝对路径请求 http://xx.xx.com/xxx/xxx
					data: {
						key: this.val
					}
				}).then(resData => {
					// 成功回调
					console.log(resData)
					//					this.lists=[1,2,3,4,5]

				}, error => {
					// 错误回调
					console.log(error)
					this.lists = error.books

				})

			},

			searchbarInputOnfocus() {

			},
			searchbarInputOnblur() {
				if(this.val == '') return
				this.fetch()
			},

			searchbarInputOninput(e) {
				this.val = e.value;
			},
			searchbarCancelClick() {

			}

		}
	}
</script>