<template>
    <view>
        <view>
        	<uni-list>
        		<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in shows"  :key="index" @tap="toDetail(item.name)">
        				<uni-list-item :show-badge="true" :show-arrow="true" :title="item.name" :note="fmtValue(item.value)" ></uni-list-item>
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
        onShow() {
			this.works=uni.getStorageSync('works')||[]
			this.doworks=uni.getStorageSync('doworks')||[]
			this.shows=[]
			if(this.works){
				for(var m in this.works){
				    this.shows.push({'name':this.works[m],'value':'0'})	
				}
			}
			
            if(this.doworks&&this.shows){
				for(var i in this.shows){
					for(var j in this.doworks){
						if(this.shows[i].name==this.doworks[j].work){
							this.shows[i].value=(parseInt(this.shows[i].value)+parseInt(this.doworks[j].value)).toString()
						}
					}
				}
			    //console.log(this.shows)	
			}
			
        },
		data() {
			return {
				shows:[],
				works:uni.getStorageSync('works')||[],
				doworks:uni.getStorageSync('doworks')||[]
			}
		},
		methods:{
			getShow(work){
				if(this.shows){
					for(var i in this.shows){
						if(this.shows[i].name==work){
							return this.shows[i]
						}
					}
				}
				
				return [{'name':work,'value':'0'}]
			},
			fmtValue(val){
				return "至今功课累计达："+val+" 部"
			},
			toDetail:function(work,date){
				uni.navigateTo({
					url: 'detail?work='+work
				})
			}
		}
		
    }
</script>

<style>
   
</style>
