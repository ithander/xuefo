<template>
	<view>
		<!-- <view style="background-color: #00BFFF">
			<view style="display: flex;">
				<image src="../../static/img/hs.png" style="width: 35px; height: 35px; "></image>
				<text>南无阿弥陀佛</text>
			</view>
		</view> -->
		
		<view style="align-content: center;">
			<form @submit="formSubmit">
				<view style="">
			        <input type="text" name="work"  style="border: #000000;background-color: #EBEDF0;min-height: 100upx;min-width: 200upx;font-size: 35rpx;" maxlength="-1"  placeholder="输入功课名称" @input="changeUse"/>
				</view>
				<view style="align-content: center;padding-left: 150rpx;padding-bottom: 50rpx;padding-right: 150rpx;padding-top: 50rpx;">
					<view style="display: flex;align-items: flex-start;">
			            <button type="primary" form-type="submit" style="min-width: 20upx;" v-bind:disabled="use">添  加 功 课</button>
					</view>
				</view>
			</form>
		</view>
		
	
		<view>
			<uni-list>
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in works"  :key="index" @tap="toEdit(item)">
						<uni-list-item :show-badge="true" :title="item"  :show-arrow="true"></uni-list-item>
				</view>
			</uni-list>
		</view>
	</view>
	
</template>
<script>

import uniList from '@/components/uni-list/uni-list.vue'
import uniListItem from '@/components/uni-list-item/uni-list-item.vue'

	export default {
		components: {
			uniList,
			uniListItem
		},
		onShow() {
			this.works=uni.getStorageSync('works')||[]
		},
		data() {
			return {
				title: 'form',
			    works:uni.getStorageSync('works')||[],
				doworks:uni.getStorageSync('doworks')||[],
				use:true
			}
		},
		onNavigationBarButtonTap:function(){
			uni.navigateTo({
				url:"../setup/setup"
			})
		},
		methods: {
			formSubmit:function(e){
				this.works.push(e.target.value.work.trim())
				this.doworks.push({"work":e.target.value.work.trim(),"date":this.getDate(),"value":"0"})
			    uni.setStorageSync('works',this.works)
				uni.setStorageSync('doworks',this.doworks)
				uni.reLaunch({
                   url: 'user'
                });
			},
			toDel:function(index){
				this.works.splice(index, 1);
				uni.setStorageSync('works',this.works)
				uni.setStorageSync('doworks',[])
			},
			getDate:function(){
			   var year =new Date().getFullYear();//获取完整的年份(4位,1970-????)
               var month = new Date().getMonth() + 1;//获取当前月份(0-11,0代表1月)
               var day = new Date().getDate();//获取当前日(1-31)
               if (month < 10) {
                    month ="0" + month;
                }
               if (day < 10) {
                    day ="0" + day;
                }
                var dateString = year +"-" + month + "-" + day;
				return dateString;
			},
			toEdit:function(work){
				uni.navigateTo({
					url: 'edit?work='+work
				})
			},
			changeUse:function(e){
				if(''!=e.target.value.trim()){
					this.use=false
				}
			}
		}
	}
</script>

<style>
.btns{
		display: flex;
		padding: 50upx 50upx 50upx 50upx;
		align-items: center;
		text-align: center;
	}
</style>
