<template>
	<mescroll-body ref="mescrollRef" @init="mescrollInit" @down="downCallback" @up="upCallback">
		<SparepartMyUseListItem :list="beanList"></SparepartMyUseListItem>
	</mescroll-body>
</template>

<script>
	//上拉加载，下拉刷新
	import MescrollMixin from "@/components/mescroll-uni/mescroll-mixins.js"
	import SparepartMyUseListItem from "./sparepartMyUseListItem.vue"
	
	export default {
		mixins: [MescrollMixin], // 使用mixin (在main.js注册全局组件)
		components: {
			SparepartMyUseListItem
		},
		data() {
			return {
				beanList: [],//我已使用的配件列表
			}
		},
		onLoad() {
		},
		methods: {
			/**
			 * 加载我已使用的配件列表
			 */
			upCallback(page){
				var params = {
					limit: page.size,
					page: page.num,
					materialName: '',
					materialModel: '',
					materialGroupName: ''
				};
				this.$api.get("sealseservice032", params).then((res)=>{
					if(res.returnCode == 0){
						this.mescroll.endSuccess(res.rows.length, res.total);
						//设置列表数据
						if(page.num == 1) this.beanList = []; //如果是第一页需手动制空列表
						this.beanList = this.beanList.concat(res.rows); //追加新数据
					}else{
						this.mescroll.endErr();
					}
				})
			}
		}
	}
	
</script>

<style>
</style>
