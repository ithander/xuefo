<template>
	<view>
		<form @submit="save">
			<view style="align-content: center;padding-bottom: 10rpx;padding-top: 100rpx;">
				    <label>功课名称：</label>
					<input v-bind:value="value" name="work" style="background-color: #D4D4D4;min-height: 100upx;font-size: 45upx;" />
			</view>
						
			<view style="align-content: center;padding-left: 50rpx;padding-bottom: 50rpx;padding-right: 50rpx;display: flex;padding-top: 100rpx;">
				<button type="warn" @tap="del">删 除</button>
				<button type="warn" @tap="clear">清除计数</button>
			</view>
			<view style="align-content: center;padding-left: 150rpx;padding-bottom: 100rpx;padding-right: 150rpx;">
				 <button type="primary" form-type="submit">保 存</button>
			</view>
		</form>
	</view>
</template>

<script>
	
	export default {
		onLoad:function(option) {
			this.work=option.work
			this.value=option.work
		},
		onShow:function(){
			
		},
		data(){
			return {
				work:"",
				value:""
			}
		},
		methods:{
			del:function(){
			    var works=uni.getStorageSync('works')||[]
			    if(works){
			    	var index=0;
			    	for(var i in works){
			    		if(works[i]==this.work){
			    			index=i;
			    		}
			    	}
			    	works.splice(index, 1);
			    	uni.setStorageSync('works',works)
			    }
			    uni.navigateBack()	
			},
			clear:function(){
				var doworks=uni.getStorageSync('doworks')||[]
				var newworks=[];
				if(doworks){
					for(var i in doworks){
						if(this.work!=doworks[i].work){
							newworks.push(doworks[i])
						}
					}
					uni.setStorageSync('doworks',newworks)
				}
			},
			save:function(e){
				this.value=e.target.value.work.trim();
				var works=uni.getStorageSync('works')||[]
				if(works){
					for(var i in works){
						if(works[i]==this.work){
							console.log(this.$mp)
							works[i]=this.value
							this.work=this.value
						}
					}
					uni.setStorageSync('works',works)
				}
				 uni.navigateBack()	
			}
		}
	}
</script>

<style>
</style>
