<template>
	<view>
			<view>
				<uni-list>
					<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in todays"  :key="index" @tap="toDetail(item.work,item.date)">
							<uni-list-item :show-badge="true" :title="item.work"  :badge-text="item.value"></uni-list-item>
					</view>
				</uni-list>
			</view>
				
	</view>
</template>

<script>
import uniList from '@/components/uni-list/uni-list.vue'
import uniListItem from '@/components/uni-list-item/uni-list-item.vue'

	export default {
		components: {uniList,uniListItem},
		onReady:function(){
			/* var date=this.getDate();
			if(this.works){
				for(var i in this.works){
					if(this.doworks){
						var len=this.todays.length;
						for(var j in this.doworks){
							if(this.works[i]==this.doworks[j].work&&date==this.doworks[j].date){
								var work={"work":this.doworks[j].work,"date":date,"value":this.doworks[j].value}
								this.todays.push(work)
							}
						}
						if(len==this.todays.length){
							var work={"work":this.works[i],"date":date,"value":"0"}
							this.doworks.push(work)
							this.todays.push(work)
						}
					}else{
						var work={"work":this.works[i].work,"date":date,"value":"0"}
						this.doworks.push(work)
						this.todays.push(work)
					}
					
				}
			} */
			
		},
		onShow:function(){
			
			var date=this.getDate();
			this.works=uni.getStorageSync('works')||[],
			this.doworks=uni.getStorageSync('doworks')||[],
			this.todays=[]
			if(this.works){
				for(var i in this.works){
					if(this.doworks){
						var len=this.todays.length;
						for(var j in this.doworks){
							if(this.works[i]==this.doworks[j].work&&date==this.doworks[j].date){
								var work={"work":this.doworks[j].work,"date":date,"value":this.doworks[j].value}
								this.todays.push(work)
							}
						}
						if(len==this.todays.length){
							var work={"work":this.works[i],"date":date,"value":"0"}
							this.doworks.push(work)
							this.todays.push(work)
						}
					}else{
						var work={"work":this.works[i],"date":date,"value":"0"}
						this.doworks.push(work)
						this.todays.push(work)
					}
					
				}
			}
		},
		data() {
			return {
				works:uni.getStorageSync('works')||[],
				doworks:uni.getStorageSync('doworks')||[],
				todays:[]
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
						toDetail:function(work,date){
							uni.navigateTo({
								url: 'detail?work='+work+'&date='+date
							})
						}
		}
	}
</script>

<style>

</style>
