<template>
	<view>
		<view style="align-content: center;padding-left: 250rpx;padding-bottom: 100rpx;padding-top: 100rpx;">
			<text >{{work}}</text>
		</view>
		
		<view style="align-content: center;padding-left: 50rpx;padding-bottom: 100rpx;padding-right: 50rpx;">
			
			<button type="warn" @tap="del">删除</button>
		</view>
		<view style="align-content: center;padding-left: 50rpx;padding-bottom: 100rpx;padding-right: 50rpx;">
		     <button type="warn" @tap="clear">清除计数</button>
		</view>
		
	</view>
</template>

<script>
	
	export default {
		onLoad:function(option) {
			this.work=option.work
		},
		onShow:function(){
			
		},
		data(){
			return {
				work:""
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
			}
		}
	}
</script>

<style>
</style>
