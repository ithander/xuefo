<template>
	<view>
		<view>
			<uni-list>
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in items"  :key="index" @tap="toPage(item)">
						<uni-list-item :show-badge="true" :title="item"  :show-arrow="true"></uni-list-item>
				</view>
			</uni-list>
		</view>
		<view>
			<uni-popup ref="popup" type="center">
				无网络连接，请检查网络
				<button @click="closePopup">关闭</button>
			</uni-popup>
		</view>
	</view>
</template>

<script>
	
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	import uniPopup from "@/components/uni-popup/uni-popup.vue"
	export default {
		components: {
			uniList,
			uniListItem,
			uniPopup
		},
		onShow() {
			
			
		},
		data() {
			return {
				items: ["佛经","佛咒","偈语","感应"]
			}
		},
		methods: {
			toPage:function(page){
				
				uni.getNetworkType({
					success: function (res) {
						if('none'!=res.networkType){
							if('佛经'==page){
								uni.navigateTo({
									url:"list"
								})
							}else if('佛咒'==page){
								
							}else if('偈语'==page){
								
							}else if('感应'==page){
								
							}
						}else{
							this.openPopup()
						}
					}
				});
				
				
			},
			openPopup(){
                this.$refs.popup.open()
			},
			closePopup(){
				this.$refs.popup.close()
			}
		}
	}
	
	
</script>

<style>
</style>
