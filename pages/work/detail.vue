<template>
	<view style="">
            <view class="text-box" scroll-y="true">
                <text style="align-content: center;">{{text}}</text>
            </view>
            <view class="btns">
				<button type="primary" size="mini" @tap="addValue(1000)">加1000</button>
				<button type="primary" size="mini" @tap="addValue(100)">加100</button>
				<button type="primary" size="mini" @tap="addValue(10)">加10</button>
                <button type="primary" size="mini" @tap="addValue(1)">加1</button>
            </view>
			<view>
				<button type="default" @tap="back">返回</button>
			</view>
			<view>
				<uni-list>
					<scroll-view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in months"  :key="index" @tap="toDetail(item.work,item.date)">
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
				
				var date=this.getDate();
				this.today={"work":this.workName,"date":this.workDate,"value":"-1"},
				this.doworks=uni.getStorageSync('doworks')||[]
				this.months=[]
				this.work=''
				
				if(this.works){
					//for(var i in this.works){
							if(this.doworks){
								for(var j in this.doworks){
									if(this.workName==this.doworks[j].work&&this.workDate==this.doworks[j].date){
										//var work={"work":this.doworks[j].work,"date":date,"value":this.doworks[j].value}
										this.today.value=this.doworks[j].value
									}
									if(this.workName==this.doworks[j].work&&this.doworks[j].date.indexOf(this.getMonth())>-1){
										this.months.push(this.doworks[j])
									}
								}
								if("-1"==this.today.value){
									this.today.value="0"
									this.doworks.push({"work":this.workName,"date":this.workDate,"value":"0"})
								}
							}else{
								this.today.value="0"
								this.doworks.push({"work":this.workName,"date":this.workDate,"value":"0"})
							}
					//}
				}
				//this.months=this.months.sort(this.sortWay)
				this.workValue=this.today.value
				this.text=this.workName+'       \n'+this.workValue+' (遍/部)'+'\n'+this.workDate
				uni.setStorageSync('doworks',this.doworks)
			},
			onLoad:function(option){
				this.workName=option.work
				this.workDate=option.date
			},
			data() {
				return {
					workName:"",
					workDate:"",
					workValue:"",
					text:"",
					works:uni.getStorageSync('works')||[],
					months:[],
					doworks:uni.getStorageSync('doworks')||[],
					today:{}
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
