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
			<el-row :gutter="20" style="text-align: center;">
				<el-col :span="12"><div class="grid-content">
					<div class="height" :id="'myChart' + index"></div>
				</div></el-col>
				<el-col :span="12"><div class="grid-content">
					<br><br>
					<el-row :gutter="20" style="text-align: center;">
						<el-col :span="12"><div class="grid-content">
							<div class="span1">在职<br>同期差异</div>
							<div class="span2">{{value['在职同期差异']}}</div>
						</div></el-col>
						<el-col :span="12"><div class="grid-content">
							<div class="span1">在职<br>同期比</div>
							<div :class="RegExp(/↓/).exec(value['在职同期比'])?'spanDown':(value['在职同期比'] == '0%'?'span2':'spanUp')">{{value['在职同期比']}}</div>
						</div></el-col>
					</el-row>
					<br>
					<el-row :gutter="20" style="text-align: center;">
						<el-col :span="12"><div class="grid-content">
							<div class="span1">在岗<br>同期差异</div>
							<div class="span2">{{value['在岗同期差异']}}</div>
						</div></el-col>
						<el-col :span="12"><div class="grid-content">
							<div class="span1">在岗<br>同期比</div>
							<div :class="RegExp(/↓/).exec(value['在岗同期比'])?'spanDown':(value['在岗同期比'] == '0%'?'span2':'spanUp')">{{value['在岗同期比']}}</div>
						</div></el-col>
					</el-row>
				</div></el-col>
			</el-row>
			<el-row :gutter="20" style="text-align: center;">
				<el-col :span="5"><div class="grid-content">
					<div class="span3">
						今年<br>分配人数<br>
						<span class="span4" style="color: red;">{{value['今年分配人数']}}</span>
					</div>
				</div></el-col>
				<el-col :span="5"><div class="grid-content">
					<div class="span3">
						留存<br>人数<br>
						<span class="span4">{{value['留存人数']}}</span>
					</div>
				</div></el-col>
				<el-col :span="4"><div class="grid-content">
					<div class="span3">
						留存<br>率<br>
						<span :class="RegExp(/↓/).exec(value['留存率'])?'spanDown2':(value['留存率'] == '0%'?'span4':'spanUp2')">{{value['留存率']}}</span>
					</div>
				</div></el-col>
				<el-col :span="5"><div class="grid-content">
					<div class="span3">
						累计分配<br>新员工<br>
						<span class="span4">{{value['新员工累计']}}</span>
					</div>
				</div></el-col>
				<el-col :span="5"><div class="grid-content">
					<div class="span3">
						累计分配<br>派遣工<br>
						<span class="span4">{{value['派遣工累计']}}</span>
					</div>
				</div></el-col>
			</el-row>
		</div>
	</div>
</template>

<style scoped>
	@media screen and (min-width: 320px) and (max-width: 750px){
		* {
			-webkit-tap-highlight-color:rgba(255,0,0,0);
		}

		.height {
			height: 200px;
			width: 120px;
		}

		.span1 {
			display: inline-block;
			font-size: 12px;
		}

		.span2 {
			display: inline-block;
			font-size: 12px;
			color: #002b60;
		}

		.span3 {
			display: inline-block;
			font-size: 6px;
		}

		.span4 {
			display: inline-block;
			font-size: 10px;
			color: #002b60;
		}

		.spanDown {
			display: inline-block;
			font-size: 12px;
			color: #00ff9c;
		}

		.spanDown2 {
			display: inline-block;
			font-size: 10px;
			color: #00ff9c;
		}

		.spanUp {
			display: inline-block;
			font-size: 12px;
			color: #ff0054;
		}

		.spanUp2 {
			display: inline-block;
			font-size: 10px;
			color: #ff0054;
		}

		.frame {
			width: 90%;
			overflow: hidden;
			background-color: white; 
			margin: 0 auto;
			margin-bottom: 2px;
			box-shadow: 0 14px 18px 0 black;
		}
	}

	@media only screen and (min-width: 751px){
		.height {
			height: 400px;
		}

		.span1 {
			display: inline-block;
			font-size: 14px;
		}

		.span2 {
			display: inline-block;
			font-size: 24px;
			color: #002b60;
		}

		.span3 {
			display: inline-block;
			font-size: 12px;
		}

		.span4 {
			display: inline-block;
			font-size: 20px;
			color: #002b60;
		}

		.spanDown {
			display: inline-block;
			font-size: 24px;
			color: #00ff9c;
		}

		.spanDown2 {
			display: inline-block;
			font-size: 20px;
			color: #00ff9c;
		}

		.spanUp {
			display: inline-block;
			font-size: 24px;
			color: #ff0054;
		}

		.spanUp2 {
			display: inline-block;
			font-size: 20px;
			color: #ff0054;
		}

		.frame {
			width: 90%;
			overflow: hidden;
			background-color: white; 
			margin: 0 auto;
			margin-bottom: 4px;
			box-shadow: 0 14px 18px 0 black;
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
			attrs1: ['今年在职', '今年在岗'],
			attrs2: ['上年在职', '上年在岗'],
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
				var datas1 = []
				for (var j = 0; j < this.attrs1.length; j++) {
					for (let k in this.obj[i]) {
						if (k == this.attrs1[j]) {
							datas1[count] = parseInt(this.obj[i][k])
							count++
						}
					}
				}
				count = 0
				var datas2 = []
				for (var j = 0; j < this.attrs2.length; j++) {
					for (let k in this.obj[i]) {
						if (k == this.attrs2[j]) {
							datas2[count] = parseInt(this.obj[i][k])
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
				        y: 12,
				        textStyle: {
				        	fontSize: window.screen.width>750?24:16,
				        	color: 'black'
				        }
				    },
				    legend: {
				        data: ['今年在职', '上年在职'],
				        y: 'bottom',
				        itemWidth: window.screen.width>750?12:6,
				        itemHeight: window.screen.width>750?12:6,
				        textStyle: {
				        	fontSize: window.screen.width>750?12:6
				        }
				    },
				    calculable: true,
				    xAxis: [
				        {
				            type: 'category',
				            data: ['在职人数', '在岗人数'],
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
					    left: '5%',  
					    right: '5%',
					    top: '25%',
					    bottom: '20%'
					},
				    series: [
				        {
				            name: '今年在职',
				            type: 'bar',
				            data: datas1,
				            itemStyle: {
								normal: {
									color: function (value) {
										// if (i%2 == 0) {
					            		return '#2ab9f0';
										// } else {
										// 	return '#54fec0'; 
										// }
					            	},
									label: {
										show: true, //开启显示
										position: 'top', //在上方显示
										textStyle: { //数值样式
											color: 'black',
											fontSize: window.screen.width>750?14:7
										}
									}
								}
							}
				        },
				        {
				            name: '上年在职',
				            type: 'bar',
				            data: datas2,
				            itemStyle: {
								normal: {
									color: function (value) {
					            		return '#013a81';
					            	},
									label: {
										show: true, //开启显示
										position: 'top', //在上方显示
										textStyle: { //数值样式
											color: 'black',
											fontSize: window.screen.width>750?14:7
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