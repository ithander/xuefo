<template>
	<view>
	
		<view>
			<uni-list>
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in works"  :key="index" @tap="toEdit(item)">
						<uni-list-item :show-badge="true" :title="item"  :show-arrow="true"></uni-list-item>
				</view>
			</uni-list>
		</view>
		<view>
			<uni-fab
				:pattern="pattern"
				:horizontal="horizontal"
				:vertical="vertical"
				:content="content"
				:direction="direction"
				@trigger="trigger"
			></uni-fab>
        </view>
		
		<view>
			
			<uni-popup ref="popup" type="center">
				<view style="align-content: center;">
					<form @submit="formSubmit">
						<view style="">
					        <input type="text" name="work"  style="border: #000000;background-color: #EBEDF0;min-height: 100upx;min-width: 200upx;font-size: 35rpx;" maxlength="-1"  placeholder="输入功课名称" @input="changeUse"/>
						</view>
						<view style="align-content: center;">
							<view style="display: flex;padding-top: 50upx;padding: 30upx;">
								<button type="default" @click="closePopup" style="min-width: 20upx;" >取消</button>
					            <button type="primary" form-type="submit" style="min-width: 20upx;" v-bind:disabled="use">确定</button>
							</view>
						</view>
					</form>
				</view>
			</uni-popup>
		</view>
		
		
		
	</view>
	
</template>
<script>

import uniList from '@/components/uni-list/uni-list.vue'
import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
import uniFab from '@/components/uni-fab/uni-fab.vue';
import uniPopup from "@/components/uni-popup/uni-popup.vue"

	export default {
		components: {
			uniList,
			uniListItem,
			uniFab,
			uniPopup
		},
		onShow() {
			this.works=uni.getStorageSync('works')||[]
		},
		data() {
			return {
				title: 'form',
			    works:uni.getStorageSync('works')||[],
				doworks:uni.getStorageSync('doworks')||[],
				use:true,
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'vertical',
				pattern: {
					color: '#7A7E83',
					backgroundColor: '#fff',
					selectedColor: '#007AFF',
					buttonColor:"#007AFF"
				},
				content: [
                {
                    iconPath: '/static/img/a_book.png',
                    selectedIconPath: '/static/img/a_book.png',
                    text: '功课',
                    active: false
					
                }]
			}
		},
		onNavigationBarButtonTap:function(){
			uni.navigateTo({
				url:"../setup/setup"
			})
		},
		methods: {
			formSubmit:function(e){
				this.closePopup()
				var val=e.target.value.work.trim();
				if(this.works){
					for(var i in this.works){
						if(this.works[i]==val){
							return
						}
					}
				}
				
				this.works.push(val)
				this.doworks.push({"work":val,"date":this.getDate(),"value":"0"})
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
			},
			trigger:function(e){
				
				switch(e.index){
					case 0:{
						this.openPopup();
					}break;
				}
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
	.btns{
			display: flex;
			padding: 50upx 50upx 50upx 50upx;
			align-items: center;
			text-align: center;
		}
</style>

<!-- <style>
	@font-face {
		font-family: texticons;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_984210_5cs13ndgqsn.ttf') format('truetype');
	}


	page,
	view {
		display: flex;
	}

	page {
		background-color: #f8f8f8;
	}

  

	.center {
		flex-direction: column;
	}

	.logo {
		width: 750upx;
		height: 240upx;
		padding: 20upx;
		box-sizing: border-box;
		background-color: #2F85FC;
		flex-direction: row;
		align-items: center;
	}

	.logo-hover {
		opacity: 0.8;
	}

	.logo-img {
		width: 150upx;
		height: 150upx;
		border-radius: 150upx;
	}

	.logo-title {
		height: 150upx;
		flex: 1;
		align-items: center;
		justify-content: space-between;
		flex-direction: row;
		margin-left: 20upx;
	}

	.uer-name {
		height: 60upx;
		line-height: 60upx;
		font-size: 38upx;
		color: #FFFFFF;
	}

	.go-login.navigat-arrow {
		font-size: 38upx;
		color: #FFFFFF;
	}

	.login-title {
		height: 150upx;
		align-items: self-start;
		justify-content: center;
		flex-direction: column;
		margin-left: 20upx;
	}

	.center-list {
		background-color: #FFFFFF;
		margin-top: 20upx;
		width: 750upx;
		flex-direction: column;
	}

	.center-list-item {
		height: 90upx;
		width: 750upx;
		box-sizing: border-box;
		flex-direction: row;
		padding: 0upx 20upx;
	}

	.border-bottom {
		border-bottom-width: 1upx;
		border-color: #c8c7cc;
		border-bottom-style: solid;
	}

	.list-icon {
		width: 40upx;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #2F85FC;
		text-align: center;
		font-family: texticons;
		margin-right: 20upx;
	}

	.list-text {
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		flex: 1;
		text-align: left;
	}

	.navigat-arrow {
		height: 90upx;
		width: 40upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		text-align: right;
		font-family: texticons;
	}
</style>
 -->