<template>
	<view>
		    <view>
		    	<uni-list>
		    		<scroll-view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in shows"  :key="index" >
		    				<uni-list-item :show-badge="true" :note="item.date" :show-arrow="false" :title="item.work"  :badge-text="item.value"></uni-list-item>
		    		</scroll-view>
		    	</uni-list>
		    </view>
			<view>
				<uni-list>
					<scroll-view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in months"  :key="index" >
							<uni-list-item :show-badge="true" :note="item.date" :show-arrow="false" :title="item.work"  :badge-text="item.value"></uni-list-item>
					</scroll-view>
				</uni-list>
			</view>
    </view>
	
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	export default {
			components: {uniList,uniListItem},
			onShow:function(){
				this.doworks=uni.getStorageSync('doworks')||[]
				this.months=[]
			    this.shows=[]
				
				if(this.workName){
							if(this.doworks){
								for(var j in this.doworks){
									if(this.workName==this.doworks[j].work&&this.doworks[j].date){
										console.log(this.doworks[j].date)
										if(this.doworks[j].date.indexOf(this.getMonth())>-1){
										    this.shows.push(this.doworks[j])
										}else{
											this.doworks[j].date=this.doworks[j].date.substring(0,7)
											addMonth(this.doworks[j]);
										}
									}
								}
							}
				}
			},
			onLoad:function(option){
				this.workName=option.work
				this.workDate=option.date
			},
			data() {
				return {
					workName:"",
					works:uni.getStorageSync('works')||[],
					months:[],
					doworks:uni.getStorageSync('doworks')||[],
					shows:[]
				}
			},
			methods: {
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
							addValue:function(num){
								var doworks=uni.getStorageSync('doworks')||[]
								if(doworks){
									for(var i in doworks){
										if(this.workDate==doworks[i].date){
											if(this.workName==doworks[i].work){
												doworks[i].value=(parseInt(doworks[i].value)+num).toString()
												this.workValue=doworks[i].value
												this.today.value=doworks[i].value
												this.text=this.workName+'       \n'+this.workValue+' (遍/部)'+'\n'+this.workDate
												uni.setStorageSync('doworks',doworks)
											}
										}
									}
								}
							},
							back:function(){
								uni.navigateBack()
							},
							getMonth:function(){
							   var year =new Date().getFullYear();//获取完整的年份(4位,1970-????)
				               var month = new Date().getMonth() + 1;//获取当前月份(0-11,0代表1月)
				               var day = new Date().getDate();//获取当前日(1-31)
				               if (month < 10) {
				                    month ="0" + month;
				                }
				               if (day < 10) {
				                    day ="0" + day;
				                }
								var dateString = year +"-" + month ;
								return dateString;
							},
							sortWay:function(dateA,dateB){
								 var begin=parseInt(dateA.date.replace(/-/g,""));
                                 var end=parseInt(dateB.date.replace(/-/g,""));
								  //js判断日期
								  if(begin-end>0){
									 return true;
								  }
							},
							addMonth(mdata){
								if(this.months.length>0){
									for(var i in this.months){
										if(this.months[i].date==mdata.date){
											this.months[i].value=(parseInt(this.months[i].value)+parseInt(mdata.date)).toString()
											mdata=null
										}
									}
									if(null!=mdata){
										this.months.push(mdata)
									}
								}else{
									this.months.push(mdata)
								}
							}
			}
		}
	
	
</script>

<style>
	.text-box {
		margin-bottom: 20upx;
		padding: 20upx 0;
		display: flex;
		min-height: 30upx;
		background-color: #FFFFFF;
		justify-content: center;
		align-items: center;
		text-align: center;
		font-size: 30upx;
		color: #353535;
		line-height: 1.8;
	}
	.btns{
		display: flex;
		padding: 50upx 50upx 50upx 50upx;
		align-items: center;
		text-align: center;
	}
</style>
