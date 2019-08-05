<template>
	<div>
		<el-dropdown trigger="click" style="position: fixed; bottom: 100px; z-index: 999;">
			<span class="el-dropdown-link" style="position: fixed; right: 10px; font-size: 40px;">
				<img class="jump" :src="img">
			</span>
			<el-dropdown-menu slot="dropdown">
			    <el-dropdown-item v-for="(value, index) in obj" :key="index"><a :href="'#myChart' + index">{{value['类别']}}</a></el-dropdown-item>
			</el-dropdown-menu>
		</el-dropdown>
		<div class="frame" v-for="(value, index) in obj" :style="index==0?'border-radius: 5px 5px 0 0;':''">
			<br>
			<div class="height" :id="'myChart' + index"></div>
			<el-row :gutter="20" style="text-align: center;">
				<el-col :span="6"><div class="grid-content">
					<span class="span1">今年共</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span class="span1">去年共</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span class="span1">同期差异</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span class="span1">同期比</span>
				</div></el-col>
			</el-row>
			<el-row :gutter="20" style="text-align: center;">
				<el-col :span="6"><div class="grid-content">
					<span class="span2" style="color: red;">{{value['今年非生产性']}}</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span class="span2" style="color: black;">{{value['去年非生产性']}}</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span :class="parseInt(value['同期差异'])<0?'spanDown':(value['同期差异'] == '0'?'span2':'spanUp')">{{value['同期差异']}}</span>
				</div></el-col>
				<el-col :span="6"><div class="grid-content">
					<span :class="RegExp(/↓/).exec(value['同期比'])?'spanDown':(value['同期比'] == '0%'?'span2':'spanUp')">{{value['同期比']}}</span>
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
			height: 150px;
		}

		.span1 {
			font-size: 7px;
		}

		.span2 {
			font-size: 15px;
			color: #013a81;
		}

		.spanDown {
			font-size: 15px;
			color: green;
		}

		.spanUp {
			font-size: 15px;
			color: red;
		}
	}

	@media only screen and (min-width: 751px){
		.height {
			height: 400px;
		}

		.span1 {
			font-size: 20px;
		}

		.span2 {
			font-size: 40px;
			color: #013a81;
		}

		.spanDown {
			font-size: 40px;
			color: green;
		}

		.spanUp {
			font-size: 40px;
			color: red;
		}
	}

	.frame {
		width: 90%; 
		background-color: white; 
		margin: 0 auto;
		margin-bottom: 2px;
		box-shadow: 0 14px 18px 0 black;
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

	.jump {
		margin: 0;
		width: 40px;
		height: 40px;
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
			attrs: ['行政', '业务', '技术', '生产管理', '后勤', '仓储', '检测', '维修', '物流'],
			img: require('@/assets/img/sx.png')
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
			for (var i = 0; i < this.obj.length; i++) {
				var count = 0
				var datas = []
				for (var j = 0; j < this.attrs.length; j++) {
					for (let k in this.obj[i]) {
						if (k == this.attrs[j]) {
							datas[count] = parseInt(this.obj[i][k])
							count++
						}
					}
				}
	            var myChart = this.$echarts.init(document.getElementById('myChart' + i), 'macarons')
	            
	            // 绘制条形图
	            myChart.setOption({
	                title: {
				        text: this.obj[i]['类别'],
				        x: 8,
				        textStyle: {
				        	fontSize: window.screen.width>750?24:16,
				        	color: 'black'
				        }
				    },
				    calculable: true,
				    xAxis: [
				        {
				            type: 'category',
				            data: this.attrs,
				            axisLabel: {
				            	interval: 0,
				            	textStyle: {
				            		fontSize: window.screen.width>750?16:8
				            	}
				            },
				            axisLine: {
					        	show: false
					        },
					        axisTick: {
					        	show: false
					        },
					        splitLine: {
					        	show: false
					        }
				        }
				    ],
				    yAxis: [
				        {
				            type: 'value',
				            show: false
						}
				    ],
				    grid: {  
					    left: '10%',  
					    right: '10%',
					    top: '15%',
					    bottom: '10%'
					},
				    series: [
				        {
				            name: '人数',
				            type: 'bar',
				            data: datas,
				            barWidth: window.screen.width>750?40:20,
				            itemStyle: {
								normal: {
									color: function (value) {
										if (i%2 == 0) {
					            			return value.dataIndex%2==0?'#013a81':'#29b8f0'; 
										} else {
											return value.dataIndex%2==0?'#54fec0':'#013a81'; 
										}
					            	},
									barBorderRadius: [10, 10, 10, 10],
									label: {
										show: true, //开启显示
										position: 'top', //在上方显示
										textStyle: { //数值样式
											color: 'black',
											fontSize: window.screen.width>750?24:12
										}
									}
								}
							}
				        }
				    ]
	            })
			}
		}
	}

}
</script>