<template>
	<view style="padding-top: 50rpx;">
		<view style="padding-left: 100upx;padding-right: 100upx;">
		    <button type="primary" @tap="checkUpdate"  v-show="enabled">检测更新</button>
			<text>{{text}}</text>
		</view>
		
		 <view style="padding-top: 150upx;padding-left: 100upx;padding-right: 100upx;">
            <view class="progress-box" v-show="showProgress">
                <progress  v-bind:percent="100" active stroke-width="50" backgroundColor="#999" show-info="true" active-mode="backwards" @activeend="reset"/>
            </view>
        </view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				enabled:true,
				showProgress:false,
				num:100,
				text:""
			}
		},
		methods: {
			
			checkUpdate:function(){
				var localVersion=2;
				uni.request({
					url: 'http://www.yifankb.com/update.json', //仅为示例，并非真实接口地址。
					header:{
						"content-type":"application/json"
					},
					method: 'GET',  
					success: (res) => {
						if(res.data.version>localVersion){
							this.enabled=false;
							
							uni.showToast({  
                                title: '有新的版本发布，程序已启动自动更新。新版本下载完成后将自动弹出安装程序。',  
                                mask: false,  
                                duration: 5000,  
                                icon:"none"  
                            });  
							this.text="正在努力下载..."
							//this.showProgress=true
							
							
							var dtask = plus.downloader.createDownload( "http://www.yifankb.com/xuefo.apk", {}, function ( d, status ) {  
                                    // 下载完成  
                                    if ( status == 200 ) {   
                                        plus.runtime.install(plus.io.convertLocalFileSystemURL(d.filename),{},{},function(error){  
											this.enabled=true;
                                            uni.showToast({  
                                                title: '安装失败',  
                                                mask: false,  
                                                duration: 1500  
                                            });  
                                        })  
                                    } else {  
										this.enabled=true;
                                         uni.showToast({  
                                            title: '更新失败',  
                                            mask: false,  
                                            duration: 1500  
                                         });  
                                    }    
                                });  
                            dtask.start();   
						}else{
							uni.showToast({  
							    title: '己是最新版本',  
							    mask: false,  
							    duration: 5000,  
							    icon:"none"  
							});  
						}
					}
				});
			},
			updateNum:function(){
				if(this.num<100){
					this.num=this.num+1;
					window.setTimeout(function(){
							this.updateNum();
					},500)
				}
			}
		}
	}
</script>

<style>

</style>
