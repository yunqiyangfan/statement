<template>
	<div>
		<div class="frame" v-for="(value, index) in finalDatas" :style="index==0?'border-radius: 5px 5px 0 0;':''">
			<div class="title">拉链（<span style="color: red; font-weight: bold;">{{name[index]}}</span>）</div>
			<el-row :gutter="20" style="text-align: center;">
				<el-col :span="8"><div class="grid-content">
					<div class="box" style="margin-top: 33px;">	
						<div class="span1">总在岗人数<br>日同比
							<span :class="RegExp(/-/).exec(value[2]['总在岗人数'])?'spanDown':(value[2]['总在岗人数'] == '0%'?'span2':'spanUp')">{{value[2]['总在岗人数']}}</span><br>
						</div>
					</div>
					<div class="box">
						<div class="span1">总在职人数<br>日同比
							<span :class="RegExp(/-/).exec(value[2]['总在职人数'])?'spanDown':(value[2]['总在职人数'] == '0%'?'span2':'spanUp')">{{value[2]['总在职人数']}}</span><br>
						</div>
					</div>
					<div class="box">
						<div class="span1">非生产性人数<br>日同比
							<span :class="RegExp(/-/).exec(value[2]['非生产性人数'])?'spanDown':(value[2]['非生产性人数'] == '0%'?'span2':'spanUp')">{{value[2]['非生产性人数']}}</span><br>
						</div>
					</div>
					<div class="box">
						<div class="span1">生产性人数<br>日同比
							<span :class="RegExp(/-/).exec(value[2]['生产性人数'])?'spanDown':(value[2]['生产性人数'] == '0%'?'span2':'spanUp')">{{value[2]['生产性人数']}}</span>
						</div>
					</div>
				</div></el-col>
				<el-col :span="16"><div class="grid-content">
					<div class="height" :id="'myChart' + index"></div>
				</div></el-col>
			</el-row>
			<br>
		</div>
	</div>
</template>

<style scoped>
	@media screen and (min-width: 320px) and (max-width: 750px){
		* {
			-webkit-tap-highlight-color:rgba(255,0,0,0);
		}

		.height {
			height: 220px;
		}

		.span1 {
			margin-bottom: 14px;
			font-size: 6px;
		}

		.span2 {
			font-size: 6px;
			color: blue;
		}

		.spanDown {
			font-size: 6px;
			color: #00bda5;
		}

		.spanUp {
			font-size: 6px;
			color: red;
		}

		.title {
			margin-left: 16px;
			margin-top: 16px;
			font-size: 14px;
			color: black;
		}

		.box {
			margin-top: 5px;
		}

		.frame {
			width: 90%;
			background-color: white; 
			margin: 0 auto;
			margin-bottom: 2px;
			box-shadow: 0 14px 18px 0 black;
			overflow: hidden;
		}
	}

	@media only screen and (min-width: 751px){
		.height {
			height: 400px;
		}

		.span1 {
			font-size: 12px;
			margin-bottom: 25px;
		}

		.span2 {
			font-size: 12px;
			color: blue;
		}

		.spanDown {
			font-size: 12px;
			color: green;
		}

		.spanUp {
			font-size: 12px;
			color: red;
		}

		.title {
			margin-left: 42px;
			margin-top: 32px;
			font-size: 24px;
			color: black;
		}

		.box {
			margin-top: 30px;
		}

		.frame {
			width: 90%;
			background-color: white; 
			margin: 0 auto;
			margin-bottom: 2px;
			box-shadow: 0 14px 18px 0 black;
			overflow: hidden;
		}
	}

	.el-dropdown-link {
	    cursor: pointer;
	    color: #409EFF;
	}

	.el-icon-arrow-down {
	    font-size: 12px;
	}

	.demonstration {
	    display: block;
	    color: #8492a6;
	    font-size: 14px;
	    margin-bottom: 20px;
	}

	a { 
		text-decoration:none; 
		color:#333; 
	}
</style>

<script>
import $ from 'jquery';
import  'echarts/theme/macarons.js'

export default {

	mounted () {
		this.draw()
	},

	data () {
		return {
			attrs1: ['当日', '日同期', '日同比', '当月', '月同期', '月同比', '当年', '年同期', '年同比'],
			attrs2: ['总在岗人数', '总在职人数', '非生产性人数', '生产性人数'],
			name: ['当日', '当月', '当年']
		}
	},

	computed: {
		finalDatas () {
			var datas = new Array()
			for (var i = 0; i < this.attrs1.length; i++) {
				datas[this.attrs1[i]] = new Array()
				for (var j = 0; j < this.attrs2.length; j++) {
					datas[this.attrs1[i]][this.attrs2[j]] = this.obj[j][this.attrs1[i]]
				}
			}
			var finalDatas = new Array()
			finalDatas[0] = [datas['当日'], datas['日同期'], datas['日同比']]
			finalDatas[1] = [datas['当月'], datas['月同期'], datas['月同比']]
			finalDatas[2] = [datas['当年'], datas['年同期'], datas['年同比']]
			return finalDatas
		}
	},

  	props: ['obj'],

	watch: {
		obj () {
			this.draw()
		}
	},

	methods: {
		draw () {
			// 实例化echarts对象
			for (var i = 0; i < this.finalDatas.length; i++) {
				var datas1 = new Array()
				datas1[0] = -this.finalDatas[i][0]['生产性人数']
				datas1[1] = -this.finalDatas[i][0]['非生产性人数']
				datas1[2] = -this.finalDatas[i][0]['总在职人数']
				datas1[3] = -this.finalDatas[i][0]['总在岗人数']

				var datas2 = new Array()
				datas2[0] = this.finalDatas[i][1]['生产性人数']
				datas2[1] = this.finalDatas[i][1]['非生产性人数']
				datas2[2] = this.finalDatas[i][1]['总在职人数']
				datas2[3] = this.finalDatas[i][1]['总在岗人数']

	            var myChart = this.$echarts.init(document.getElementById('myChart' + i), 'macarons')
	            
	            // 绘制条形图
	            myChart.setOption({
				    legend: {
				        data: [this.attrs1[i*3], this.attrs1[i*3+1]],
				        itemWidth: window.screen.width>750?14:7,
				        itemHeight: window.screen.width>750?14:7,
				        textStyle: {
				        	fontSize: window.screen.width>750?14:7
				        },
				        y: 0
				    },
				    calculable: true,
				    xAxis: [
				        {
				            type: 'value',
				            show: false
				         //    axisLabel: {
				         //    	textStyle: {
				         //    		fontSize: window.screen.width>750?14:6,
				         //    		color: 'black'
				         //    	},
				         //    	formatter: function (value) {
				         //    		return Math.abs(value)
				         //    	}
				         //    },
					        // axisTick: {
					        // 	show: false
					        // },
					        // splitLine: {
					        // 	show: false
					        // },
					        // splitArea: {
					        // 	show: false
					        // },
					        // axisLine: {
					        // 	lineStyle: {
					        // 		color: '#013a81'
					        // 	}
					        // }
				        }
				    ],
				    yAxis: [
				        {
				            type: 'category',
				            show: false
				        }
				    ],
				    grid: {  
					    left: '10%',  
					    right: '10%',
					    top: '10%',
					    bottom: '10%'
					},
				    series: [
				        {
				            name: this.attrs1[i*3],
				            type: 'bar',
				            stack: '总量',
				            barWidth: 15,
				            itemStyle: {
				            	normal: {
				            		color: function (value) {
					            		return '#2ab9f1';
					            	},
					                label: {
					                	show: true, 
					                	position: 'left',
						                formatter: function (value) {
					            			return -value.data
					            		},
					            		textStyle: { //数值样式
											color: 'black',
											fontSize: window.screen.width>750?14:7
										}
				            		} 
					            }
					        },
					        data: datas1
					    },
				        {
				            name: this.attrs1[i*3+1],
				            type: 'bar',
				            stack: '总量',
				            barWidth: 15,
				            itemStyle: {
				            	normal: {
				            		color: function (value) {
					            		return '#013a81';
					            	},
					                label: {
					                	show: true, 
					                	position: 'right',
					                	textStyle: { //数值样式
											color: 'black',
											fontSize: window.screen.width>750?14:7
										}
					                }
					            }
				        	},
				            data: datas2
				        }
				    ]
	            })
			}
		}
	}

}
</script>