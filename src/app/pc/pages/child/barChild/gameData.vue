<template>
<div class="gameDataContainer">
    <div class="activityListTitle">
        <span class="icon"></span><a href="javascript:;" @click="goBack">我的活动</a>
    </div>
    <div class="activeTabMenu">
        <div class="menu" :class="{'checked': currTab}" @click="currTab = true">传播分析</div>
        <div class="menu" :class="{'checked': !currTab}"  @click="currTab = false">玩家分析</div>
    </div>
	<div v-show="currTab">
		<div class="gameInfoBox">
			<div class="columnTitle">
				活动详细
				<div class="" style="float: right;cursor: pointer;">
					<div class="refresh-box" @click="refreshClick">
            			<div class="refresh-btn">刷新</div>
						<!-- <embed :src="refresh" class="refresh-btn" type="image/svg+xml" /> -->
					</div>
				</div>
			</div>
			<div class="gameInfo info">
				<div class="line">活动名称：{{activiyInfo.name}}</div>
				<div class="line">活动类型：按几率派奖游戏</div>
				<div class="line">活动模板：{{activiyInfo.templateName}}</div>
				<div class="line">活动时间：{{activiyInfo.startTime | formatDatetime('yyyy-MM-dd hh:mm')}}&nbsp;到&nbsp;{{activiyInfo.endTime | formatDatetime('yyyy-MM-dd hh:mm')}}</div>
				<div class="line">活动状态：<span :style="{'color': activiyInfo.isPublish ? (activiyInfo.isEnd ? '#fe896a' : '#64cea6'): '#999' }">{{activiyInfo.isPublish ? (activiyInfo.isEnd ? '已结束' : '进行中'): '未发布'}}</span></div>
				
			</div>
			<div class="awardInfo info">
				<div class="line mainColor">奖项明细：</div>
				<div class='line' v-for="(item, index) in winLotteryItems" :key="index">
					<span style='display:inline-block;'>{{item.levelName}}</span>
					<span class='amountInfo'>
						<span style='margin-left:15px;display:inline-block;'>已领：{{item.getCount}}份&nbsp;</span>
						<span style='display:inline-block;'>剩余：{{item.leftCount}}份；</span>
						<span style='display:inline-block;'>已核销：{{item.verificatedCount}}份；</span>
					</span>
				</div>
			</div>
		</div>
		<div class="page_gameData pageBox">
			<div class="gameStatDataBox">
				<div class="gameStatDataPanel">
					<!-- 整体趋势 -->
					<div class="statistical">
						<div class="toolBar columnTitle">
							<div class="title">
								整体趋势
							</div>
						</div>
						<div class="statData">
							<div class="column other">
								<div class="box">
									浏览人数：<div  class="count">{{other.visitStatistic.uvCount}}</div>
								</div>
							</div>
							<div class="column other">
								<div class="box">
									参与人数：<div class="count">{{other.visitStatistic.joindCustomerCount}}</div>
								</div>
							</div>

							<div class="column other">
								<div class="box">
									获奖人数：<div class="count">{{other.visitStatistic.winCount}}</div>
								</div>
							</div>
							<!-- <div class="column other">
								<div class="box" style="border: none;">
									分享人数：<div id="share" class="count">{{other.visitStatistic.shareCount}}</div>
								</div>
							</div> -->
						</div>
						<div class="statDiagram" style="margin: 0; padding-right: 16px;">
							<div class="statDiagramPanel">
								<div class='statDiagramContent' style="padding:50px 20px;">
									<div id="statDagram" style="width: 100%;min-height: 400px"></div>
								</div>
								<div class="statDiagramTips" style="padding-top: 30px;">注：趋势图数据并非实时更新，5分钟统计一次，以活动累计数据为准</div>
							</div>
						</div>
					</div>
					<!-- 传播层级 -->
					<div class="statDiagram">
						<div class="columnTitle ">
							传播层级
						</div>
						<div class="statDiagramPanel">
							<div class='statDiagramContent'>
								<div id="statSpreadDagram" style="width:100%;min-height:300px"></div>
							</div>
							<div class="statDiagramTips">注：传播层级数据并非实时更新，5分钟统计一次</div>
						</div>
					</div>
					<!-- 进入来源 -->
					<div class="sourceBlock sourceStat left">
						<div class="statDiagramTitleBar bottomBoder">
							<div class="lineTitle left">
								<div class="titleIcon left"></div>
								<div class="left info">进入来源</div>
							</div>
						</div>
						<div class="statDiagramPanel">
							<div id="sourceStatDagram" style="min-height:300px"></div>
							<div class="statDiagramTips" style="padding-top: 0">注：进入来源数据并非实时更新，5分钟统计一次</div>
						</div>
					</div>
					<!-- 分享去向 -->
					<!-- <div class="sourceBlock shareEnd left">
						<div class="statDiagramTitleBar bottomBoder">
							<div class="lineTitle left">
								<div class="titleIcon left"></div>
								<div class="left info">分享去向</div>
							</div>
						</div>
						<div class="statDiagramPanel">
							<div id="shareEndDagram" style="min-height:300px"></div>
							<div class="statDiagramTips" style="padding-top: 0">注：分享去向数据并非实时更新，5分钟统计一次</div>
						</div>
					</div> -->
					<div style="clear: both"></div>
				</div>
			</div>
		</div>
	</div>
	<div v-show="!currTab">
		<div class="gameInfoBox">
			<div class="columnTitle">
				玩家详细
				<div class="" style="float: right;cursor: pointer;">
					<div class="refresh-box" @click="refreshClick">
            			<div class="refresh-btn">刷新</div>
						<!-- <embed :src="refresh" class="refresh-btn" type="image/svg+xml" /> -->
					</div>
				</div>
			</div>
			<div class="gameInfo info">
				<div class="line">查看玩家总人数：{{other.visitStatistic.uvCount}}</div>
				<div class="line">参与玩家总数量：{{other.visitStatistic.joindCustomerCount}}</div>
				<div class="line">中奖玩家总数量：{{other.visitStatistic.winCount}}</div>
			</div>
			<div class="awardInfo info">
				<div class="line mainColor">奖项明细：</div>
				<div class='line' v-for="(item, index) in winLotteryItems" :key="index">
					<span style='display:inline-block;'>{{item.levelName}}</span>
					<span class='amountInfo'>
						<span style='margin-left:15px;display:inline-block;'>已领：{{item.getCount}}份&nbsp;</span>
						<span style='display:inline-block;'>剩余：{{item.leftCount}}份；</span>
						<span style='display:inline-block;'>已核销：{{item.verificatedCount}}份；</span>
					</span>
				</div>
			</div>
		</div>
		<div class="playerStat">
			<div class="statBox sexStat">
				<div class="statDiagramTitleBar">
					<div class="lineTitle left">
						<div class="titleIcon left"></div>
						<div class="left info">性别比例</div>
					</div>
				</div>
				<div class="statDiagramPanel">
					<div id="sexStatDiagram" style="width:100%;min-height:320px"></div>
				</div>
			</div>
			<div class="statBox backStat">
				<div class="statDiagramTitleBar">
					<div class="lineTitle left">
						<div class="titleIcon left"></div>
						<div class="left info" >终端网络类型</div>
					</div>
				</div>
				<div class="statDiagramPanel">
					<div id="backStatDiagram" class="pointer"  style="width:100%;min-height:320px">
					</div>
				</div>
			</div>
		</div>
		<div class="deviceStat">
			<div class="statBox sysTypeStat">
				<div class="statDiagramTitleBar">
					<div class="lineTitle left">
						<div class="titleIcon left"></div>
						<div class="left info">设备</div>
					</div>
				</div>
				<div class="statDiagramPanel">
					<div class="content">
						<div class="android left">
							<div class="type left">
								<div class="icon"></div>
								<div class="data">安卓</div>
							</div>
							<div id="deviceAndroid" class="info left">{{terminalStatistic.and}}</div>
						</div>
						<div class="apple left">
							<div class="type left">
								<div class="icon"></div>
								<div class="data">苹果</div>
							</div>
							<div id="deviceApple" class="info left">{{terminalStatistic.ios}}</div>
						</div>
						<div class="other left">
							<div class="type left">
								<div class="icon">
									<div class="spot"></div>
									<div class="spot"></div>
									<div class="spot"></div>
								</div>
								<div class="data">其他</div>
							</div>
							<div id="deviceOther" class="info left">{{terminalStatistic.other}}</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
</template>

<script>
import {getStatistic} from 'pcApi/jie.js'
import refresh from 'pcAssets/images/refresh.svg'
var echarts = require('echarts');
import {
    option,
    shareStatisticOption,
    sourceStatDagramOption,
	// shareEndDagramOption,
	sexStatDiagramOption,
	backStatDiagramOption
} from './gameData.js'
export default {
	name: "gameData",
	props: {
		id: {
			type: String
		}
	},
    data() {
        return {
			refresh,
			option,
			shareStatisticOption,
			sourceStatDagramOption,
			// shareEndDagramOption,
			sexStatDiagramOption,
			backStatDiagramOption,
            statDagram: null,
            statSpreadDagram: null,
            sourceStatDagram: null,
            // shareEndDagram: null,
            sexStatDiagram: null,
            backStatDiagram: null,
            timeValue: '',
            pickerOptions2: {
                shortcuts: [{
                        text: '今天',
                        onClick(picker) {
                            const start = new Date();
                            picker.$emit('pick', [start, start]);
                        }
                    },
                    {
                        text: '昨天',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24);
                            picker.$emit('pick', [start, end]);
                        }
                    },
                    {
                        text: '最近一周',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: '最近一个月',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                            picker.$emit('pick', [start, end]);
                        }
                    }
                ]
			},
			currTab: true,
			other: {
				visitStatistic: {
					joindCustomerCount: 3, // 参与客户数
					pvCount: 10, // 页面访问次数 
					// shareCount: 2, // 分享数
					uvCount: 5, // 独立访客次数 
					winCount: 1, // 中奖数
				}
			},
			shareStatistic: [

			],
			activiyInfo: {
				
			},
			winLotteryItems: [],
			terminalStatistic: {
				and: '',
				ios: '',
				other: ''
			}
        }
    },
    methods: {
        goBack() {
            this.$emit('toBackGameData', -1)
		},
		refreshClick() {
			getStatistic({
				id: this.id
			}).then(res => {
				if (res.errorCode == 0) {
					this.resData = res.data;
					this.other.visitStatistic = res.data.visitStatistic;
					this.activiyInfo = res.data.activiyInfo;
					this.winLotteryItems = res.data.winLotteryItems;
					this.shareStatistic = res.data.shareStatistic;

					/* 整体预览 */
					let pvCount = [] , winCount=[], joindCustomerCount= [];
					res.data.dailyVisitStatistic.forEach(el => {
						let times = new Date(el.dateTime).getTime(); 
						pvCount.push([
							times,
							el.uvCount
						])
						// shareCount.push([
						// 	times,
						// 	el.shareCount
						// ])
						winCount.push([
							times,
							el.winCount
						])
						joindCustomerCount.push([
							times,
							el.joindCustomerCount
						])
					});
					this.option.series = [
						{
						name: "浏览人数",
						type: "line",
						data: pvCount
						},
						{
						name: "参与人数",
						type: "line",
						data: joindCustomerCount
						},
						{
						name: "获奖人数",
						type: "line",
						data: winCount
						},
						// {
						// name: "分享人数",
						// type: "line",
						// data: shareCount
						// }
					];

					/* 传播层级 */
					let list = [], sharPvList = [], sharJoindList = [], sharList = [];
					this.shareStatistic.sort((a,b)=>{
						return a.shareLevel - b.shareLevel
					})
					this.shareStatistic.forEach(el => {
						sharPvList.push(el.uvCount);
						sharJoindList.push(el.joindCustomerCount);
						// sharList.push(el.shareCount);
						if (el.shareLevel == 0) {
							list.push('传播源')
						}else {
							list.push('第'+el.shareLevel+'层')
						}
					});
					this.shareStatisticOption.xAxis[0].data = list;
					this.shareStatisticOption.series = [
						  {
						name: "浏览人数",
						type: "bar",
						barGap: 0,
						data: sharPvList
						},
						{
						name: "参与人数",
						type: "bar",
						data: sharJoindList
						},
						// {
						// name: "分享人数",
						// type: "bar",
						// data: sharList
						// }
					];

					/* 进入来源 */
					let sourList = [
						{
							name:'朋友圈',
							value: 0
						},{
							name:'好友及群聊',
							value: 0
						},{
							name:'二维码',
							value: 0
						}
					];
					res.data.sourceStatistic.forEach(el => {
						if ( el.type - 1 >= 0 &&   el.type - 1 < sourList.length ) {
							sourList[el.type - 1].value = el.count
						}
					});
					this.sourceStatDagramOption.series[0].data = sourList;


					/* 分享去向 */
					// let tagList = [
					// 	{
					// 		name:'朋友圈',
					// 		value: 0
					// 	},{
					// 		name:'好友及群聊',
					// 		value: 0
					// 	}
					// ];
					// res.data.shareTargetStatistic.forEach(el => {
					// 	tagList[el.type+1].value = el.count
					// });	
					// this.shareEndDagramOption.series[0].data = tagList;
					
					/* 性别比例 */
					let sexStatisticList = [
						{
							name:'未知',
							value: 0
						},{
							name:'男',
							value: 0
						},{
							name:'女',
							value: 0
						}
					];
					res.data.sexStatistic.forEach(el => {
						sexStatisticList[el.type].value = el.count
					});	
					this.sexStatDiagramOption.series[0].data = sexStatisticList;

					/* 网络类型 */
					let networkTargetStatisticList = [
						{
							name:'WIFI',
							value: 0
						},{
							name:'运营商',
							value: 0
						}
					];
					let allNum = 0;
					res.data.networkTargetStatistic.forEach(el => {
						if (  el.type - 1 >= 0 &&  el.type - 1 < networkTargetStatisticList.length ) {
							networkTargetStatisticList[el.type - 1].value = el.count;
						}
						allNum += el.count;
					});	
					this.backStatDiagramOption.series[0].data = networkTargetStatisticList;

					
					res.data.terminalStatistic.forEach(el => {
						if (el.type == '1') {
							this.terminalStatistic.and = Math.round( el.count / allNum * 100 *100) /100 + '%';
						}else if (el.type == '2') {
							this.terminalStatistic.ios = Math.round( el.count / allNum * 100 *100) /100 + '%';
						}else if (el.type == '0') {
							this.terminalStatistic.other = Math.round( el.count / allNum * 100 *100) /100 + '%';
						}
					});	
					// this.shareEndDagram.setOption(this.shareEndDagramOption);
            		this.sourceStatDagram.setOption(this.sourceStatDagramOption);
            		this.statSpreadDagram.setOption(this.shareStatisticOption);
           			this.statDagram.setOption(this.option);
				}
			})
		}
    },
    created() {
        this.$nextTick(() => {
            this.statDagram = echarts.init(document.getElementById('statDagram'));
            this.statSpreadDagram = echarts.init(document.getElementById('statSpreadDagram'));
            this.sourceStatDagram = echarts.init(document.getElementById('sourceStatDagram'));
            // this.shareEndDagram = echarts.init(document.getElementById('shareEndDagram'));
			this.refreshClick();
        })
	},
	watch: {
		currTab(newVal) {
			if (!newVal && !this.sexStatDiagram) {
				this.$nextTick(() => {
					this.sexStatDiagram = echarts.init(document.getElementById('sexStatDiagram'));
					this.backStatDiagram = echarts.init(document.getElementById('backStatDiagram'));
					this.sexStatDiagram.setOption(this.sexStatDiagramOption);
					this.backStatDiagram.setOption(this.backStatDiagramOption);
				})	
			}
		}
	}
};
</script>

<style scoped>
@import "./gameData.scss";

.gameDataContainer {
    background: none repeat scroll 0 0 #f5f5f5;
    margin-bottom: 68px;
}

.activityListTitle {
    font-size: 16px;
    line-height: 16px;
    padding: 10px 0 20px 15px;
    color: #949494;
}

.activityListTitle a {
    text-decoration: underline;
    color: #585858;
    display: inline-block;
    margin-right: 8px;
}

.activityListTitle .icon {
    display: inline-block;
    width: 8px;
    height: 8px;
    border-top: solid 1px #585858;
    border-left: solid 1px #585858;
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    -ms-transform: rotate(-45deg);
    margin-top: -3px;
    vertical-align: middle;
    margin-right: 4px;
}

.activityListTitle .name {
    display: inline-block;
    margin-left: 8px;
}

.columnTitle:before {
    content: "";
    display: inline-block;
    vertical-align: middle;
    width: 4px;
    height: 19px;
    background: #329fea;
    margin-right: 8px;
    margin-top: -2px;
}
</style>
