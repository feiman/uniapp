<template>
	<view class="hbody">
		<view class="top bgwhite">
			<view class="ic_image ic_search fl"><img src="static/icons/ic_search.svg"></img></view>
			<view class="ic_image ic_message fr"><img src="static/icons/ic_message.svg"></img></view>
			<view class="ic_image ic_signin fr"><img src="static/icons/ic_signin.svg"></img></view>
			<view class="ic_logo"><img src="static/images/index_logo.png"></img></view>
		</view>
		<view class="uni-slide">
			<swiper :indicator-dots="true" :autoplay="true" :interval="5000" :duration="1000">
				<swiper-item v-for="(item,index) in itemList" :key="index">
					<img :src="item.url" :alt="item.title"></img>
				</swiper-item>
			</swiper>
		</view>
		<view class="uni-swiper-msg bgwhite mb20">
			<view class="uni-swiper-msg-icon">
				<img src="static/icons/ic-notification.svg"></img>
			</view>
			<swiper vertical="true" autoplay="true" circular="true" interval="3000">
				<swiper-item v-for="(item, index) in msg" :key="index" class="uni-swiper-msg-con">
					<navigator>{{item}}</navigator>
				</swiper-item>
			</swiper>
		</view>
		<view class="content mb20">
			<view class="bgwhite uni-nav">
				<view class="uni-nav-con" v-for="(nav,index) in navList" :key="index">
					<navigator :url="nav.path">
						<view class="image-view">
							<img class="uni-nav-image" :src="nav.url"></img>
						</view>
						<text class="uni-nav-title">{{nav.title}}</text>
					</navigator>
				</view>
			</view>
		</view>
		<view class="content mb20">
			<view class="title mb10">待办事项</view>
			<view v-for="(item,index) in eventList" :key="index">
				<view class="bgwhite event-box mb20">
					<view class="event-box-top">
						<view class="event-box-toptitle" @click="goDetail(item)">{{item.title}}</view>
						<view class="type" :class="{'fontbg1':item.type=='1','fontbg':item.type=='2'}">
							<text v-if="item.type==1">巡检</text>
							<text v-if="item.type==2">其它</text>
						</view>
						<view class="alarmType" :class="{'bg1':item.alarmType=='1','bg2':item.alarmType=='2','bg3':item.alarmType=='3'}"></view>
					</view>
					<view class="event-box-con">
						<view class="event-box-conleft">
							<text>{{item.code}}</text>
							<view>
								<text class="mr10">{{item.user}}</text><text>{{item.time}}</text>
							</view>
							<text class="fontcolor999">{{item.createTime}}</text>
						</view>
						<view class="event-box-conright">
							<text>{{item.content}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import { mapState } from 'vuex'
	export default {
		computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		onLoad() {
			if (!this.hasLogin) {
                uni.showModal({
                    title: '未登录',
                    content: '您未登录，需要登录后才能继续',
                    /**
                     * 如果需要强制登录，不显示取消按钮
                     */
                    showCancel: !this.forcedLogin,
                    success: (res) => {
                        if (res.confirm) {
							/**
							 * 如果需要强制登录，使用reLaunch方式
							 */
                            if (this.forcedLogin) {
                                uni.reLaunch({
                                    url: '/pages/login/index'
                                });
                            } else {
                                uni.navigateTo({
                                    url: '/pages/login/index'
                                });
                            }
                        }
                    }
                });
            }
		},
		onReachBottom() {
			console.log('上拉加载刷新');
		},
		onPullDownRefresh() {
			console.log('下拉刷新');
		},
		data() {
			return {
				itemList: [
					{title:'img1',url:'static/images/slide.jpg'},
					{title:'img2',url:'static/images/slide.jpg'},
				],
				msg : [
					'uni-app行业峰会频频亮相，开发者反响热烈',
					'DCloud完成B2轮融资，uni-app震撼发布',
					'36氪热文榜推荐、CSDN公号推荐 DCloud CEO文章36氪热文榜推荐、CSDN公号推荐 DCloud CEO文章'
				],
				navList:[
					{title:'值班管理',url:'static/icons/ic_duty.svg',path:'/pages/env/index'},
					{title:'巡检管理',url:'static/icons/ic_polling.svg',path:'/pages/env/index'},
					{title:'工单管理',url:'static/icons/ic_workorder.svg',path:'/pages/env/index'},
					{title:'资产管理',url:'static/icons/ic_property.svg',path:'/pages/env/index'},
					{title:'我的关注',url:'static/icons/ic_myfocus.svg',path:'/pages/env/index'},
					{title:'知识管理',url:'static/icons/ic_knowledge.svg',path:'/pages/env/index'},
					{title:'容量管理',url:'static/icons/ic_capacity.svg',path:'/pages/env/index'},
					{title:'更多',url:'static/icons/ic_more.svg',path:'/pages/env/index'},
				],
				eventList:[
					{id:'1',title:'UPS欠压排查维修',user:'张三',time:'2018-11-13 12:30',createTime:'1小时前',code:'WOT-201807130001',type:'1',alarmType:'1',content:'待审批'},
					{id:'2',title:'机房001配电系统例行巡检',user:'张三',time:'2018-11-13 12:30',createTime:'1小时前',code:'WOT-201807130001',type:'1',alarmType:'2',content:'待执行'},
					{id:'3',title:'机房003配电系统例行巡检',user:'张三',time:'2018-11-13 12:30',createTime:'1小时前',code:'WOT-201807130001',type:'2',alarmType:'3',content:'待通过'},
				]
			}
		},
		methods: {
			goDetail:function(item){
				uni.navigateTo({
					url: "/pages/workOrder/detail?detail=" + encodeURIComponent(JSON.stringify(item))
				})
			},
		}
	}
</script>

<style>
	.top{
		height: 60px;
		padding: 60px 40px 20px 40px;
		position:relative;
	}
	.ic_image,
	.ic_image img{
		width: 40px;
		height: 40px;
	}
	.ic_message{
		margin-left: 25px;
	}
	.ic_logo{
		position:absolute;
		left: 50%;
		margin-left: -110px;
		width: 220px;
		height: 40px;
	}
	.ic_logo img{
		width:100%;
		height:40px;
	}
	.uni-slide uni-swiper{
		height: 450px;
	}
	.uni-slide img{
		width:100%;
		height:100%;
	}
	.uni-swiper-msg-icon img{
		width:80%;
		height:80%;
	}
	.uni-swiper-msg{
		width: auto;
		height: 60px;
		padding-left: 20px;
		padding-right: 20px;
	}
	.uni-swiper-msg-con{
		overflow:hidden;
	}
	.uni-nav{
		 display: flex;
		 flex-wrap:wrap;
		 justify-content:center;
		 padding-top: 15px;
	}
	.uni-nav-con{
		width: 25%;
		text-align:center;
		margin-bottom: 15px;
	}
	.image-view{
		width: 60px;
		height: 60px;
		margin: 10px auto;
	}
	.image-view img{
		width:100%;
		height: 100%;
	}
	.event-box{
		padding: 0 20px;
	}
	.event-box-top,
	.event-box-con{
		display:flex;
		position:relative;
	}
	.event-box-con{
		color:#666;
		padding-bottom: 20px;
	}
	.event-box-toptitle{
		max-width: 65%;
		font-size:28px;
		overflow:hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		padding: 10px 0;
	}
	.type{
		margin: 15px 5px;
		padding: 0 10px;
		border-radius: 10px;
	}
	.event-box-conleft{
		width: 80%;
	}
	.event-box-conright{
		width: 20%;
		text-align:right;
		padding-top: 20px;
		color:#6FD8A5;
		font-size: 28px;
	}
	.alarmType{
		width: 30px;
		height: 50px;
		position:absolute;
		right:0;
		top:0;
	}
	.alarmType:after{
		content:'';
		width:0;
		height:0;
		font-size:0;
		line-height:0;
		border-left:15px solid transparent;
		border-right: 15px solid transparent;
		border-bottom:15px solid #fff;
		position:absolute;
		left:0;
		bottom:0;
	}
</style>
