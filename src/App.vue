<template>
	<div id="app" :style="table == '表格'?'background-color: #ececec;':'background-color: #002b60;'">
		<div id="header"><br>人事报表</div>
		<!-- 切换表格和图表的组件 -->
		<el-radio-group v-model="table" size="mini" style="position: absolute; top: 18px; left: 10px; z-index: 999; width: 50%;">
    		<el-radio-button label="表格"></el-radio-button>
    		<el-radio-button label="图表"></el-radio-button>
    	</el-radio-group>
    	<!-- 标题，由sheet变量控制显示，同时这里也显示处理过的时间参数 -->
    	<el-row>
			<el-col><div class="grid-content">
				<h1 v-if="sheet == 1">非生产性岗位人事报表<br><span style="color: red; font-size: 12px;">{{dateFormat}}</span></h1>
				<h1 v-else-if="sheet == 2">每日人事报表<br><span style="color: red; font-size: 12px;">{{dateFormat}}</span></h1>
				<h1 v-else-if="sheet == 3">人事报表汇总<br><span style="color: red; font-size: 12px;">{{dateFormat}}</span></h1>
			</div></el-col>
		</el-row>
		<!-- 以下为主要的六个组件，三个报表，每个报表两个组件，一个显示表格，一个显示图表，通过obj变量把数据传给子组件 -->
		<NonIndustrial v-if="sheet == 1 && table == '表格'" :obj="obj.list1"></NonIndustrial>
		<NonIndustrialGraph v-else-if="sheet == 1 && table == '图表'" :obj="obj.list1"></NonIndustrialGraph>
		<Today v-else-if="sheet == 2 && table == '表格'" :obj="obj.list2"></Today>
		<TodayGraph v-else-if="sheet == 2 && table == '图表'" :obj="obj.list2"></TodayGraph>
		<Collect v-else-if="sheet == 3 && table == '表格'" :obj="obj.list3"></Collect>
		<CollectGraph v-else-if="sheet == 3 && table == '图表'" :obj="obj.list3"></CollectGraph>
		<!-- 底部，用于在三张报表中切换，用户点击则更改sheet变量的值 -->
		<div class="bottom">
			<el-row>
				<el-col :span="8"><div class="grid-content" @click="sheet=3">
					<img :src="'./static/img/' + src3">
					<br><span :class="sheet==3?'bottomTextFocused':'bottomText'">总况</span>
				</div></el-col>
				<el-col :span="8"><div class="grid-content" @click="sheet=2">
					<img :src="'./static/img/' + src2">
					<br><span :class="sheet==2?'bottomTextFocused':'bottomText'">每日人事</span>
				</div></el-col>
				<el-col :span="8"><div class="grid-content" @click="sheet=1">
					<img :src="'./static/img/' + src1">
					<br><span :class="sheet==1?'bottomTextFocused':'bottomText'">非生产性岗位</span>
				</div></el-col>
			</el-row>
		</div>
  	</div>
</template>

<style>
	* {
		font-family: '微软雅黑';
	}

	@media screen and (min-width: 320px) and (max-width: 750px){
		/* 在这里写小屏幕设备的样式 */
		#app {
			padding-bottom: 40px;
		}

		.bottom {
			width: 100%;
			position: fixed;
			bottom: 0;
			margin: 0;
			background-color: #013a81;
			text-align: center;
			line-height: 9px;
		}

		.bottomText {
			font-size: 9px;
			color: #6c9ed1;
		}

		.bottomTextFocused {
			font-size: 9px;
			color: white;
		}

		img {
			height: 25px;
			margin-top: 5px;
		}

		table {
    		box-sizing: border-box;
    		text-align: center;
    		table-layout: fixed;
    		min-width: 100%;
    		border-spacing: 0;
		    border-bottom: solid 1px #fff;
		    border-right: solid 1px #fff;
    	}
    	
		table tr td {
		    border-top: solid 1px #fff;
		    border-left: solid 1px #fff;
		}
    	
    	th {
   		    border-left: 1px #fff solid;
		    background-color: #5b9bd5;
		    color: #ececec;
		    font-size: 7px;
		    text-align: center;
		    width: 24px;
    	}

    	td {
    		font-size: 6px;
    		border-top: solid 1px #fff;
		    border-left: solid 1px #fff;
		    height: 24px;
    	}

    	div {
    		width: 100%;
    		overflow:auto;
    	}
    	
    	h1 {
    		text-align: center;
    		margin-bottom: 8px;
    		font-size: 18px;
    		color: #0093fa;
    		margin-top: 20px;
    		margin-bottom: 16px;
    	}
	}
	
	@media only screen and (min-width: 751px){
		/* 这里写pc客户端的样式 */
		.bottom {
			width: 100%;
			margin-top: 20px;
			width: 100%;
			background-color: #013a81;
			text-align: center;
		}

		.bottomText {
			font-size: 18px;
			color: #6c9ed1;
		}

		.bottomTextFocused {
			font-size: 18px;
			color: white;
		}

		img {
			width: 30px;
			margin-top: 5px;
			cursor: pointer;
		}

		table {
			width: 100%;
		}

		table tr td th {
		    border-top: solid 1px #fff;
		    border-left: solid 1px #fff;
		}
    	
    	th {
		    background-color: #5b9bd5;
		    color: #ececec;
		    font-size: 20px;
		    text-align: center;
    	}

    	td {
    		font-size: 16px;
    		line-height: 32px;
    		border-top: solid 1px #fff;
		    border-left: solid 1px #fff;
		    text-align: center;
    	}
    	
    	h1 {
    		text-align: center;
    		margin-top: 40px;
    		margin-bottom: 32px;
    		font-size: 24px;
    		color: #0093fa;
    	}
    	
    	div {
    		width: 100%;
    		overflow:auto;
    	}
	}

	html, body {
	    margin:0;
	    padding:0;
	    background-color: #ececec;
	}

	#header {
		width: 100%;
		height: 60px;
		background-color: #0093fa;
		color: #fefefe;
		text-align: center;
		font-size: 20px;
		line-height: 20px;
	}
	
	.tr1 {
		background-color: #d2deef;
	}

	.tr2 {
		background-color: #eaeff7;
	}

	.td1 {
		background-color: #1bbcbb;
	}

	.td2 {
		background-color: #dcffff;
	}

	.blUp {
		color:#ff0000;
	}

	.blDown {
		color:#008927;
	}
</style>

<script>
import $ from 'jquery';
import NonIndustrial from './components/NonIndustrial.vue'
import Today from './components/Today.vue'
import Collect from './components/Collect.vue'
import NonIndustrialGraph from './components/NonIndustrialGraph.vue'
import TodayGraph from './components/TodayGraph.vue'
import CollectGraph from './components/CollectGraph.vue'
// 这个函数用来获取URL参数
function getQueryVariable(variable)
{
       var query = window.location.search.substring(1);
       var vars = query.split("&");
       for (var i=0;i<vars.length;i++) {
               var pair = vars[i].split("=");
               if(pair[0] == variable){return pair[1];}
       }
       return(false);
}
// 这个函数用来处理用户没有输入日期参数的默认情况，默认为今日日期
function getNowFormatDate() {
    var date = new Date()
    date.setTime(date.getTime()-24*60*60*1000)
    var year = date.getFullYear()
    var month = date.getMonth() + 1
    var strDate = date.getDate()
    if (month >= 1 && month <= 9) {
        month = '0' + month
    }
    if (strDate >= 0 && strDate <= 9) {
        strDate = '0' + strDate
    }
    var currentdate = '' + year + month + strDate
    return currentdate
}

export default {

  	name: 'app',

  	components: { // 自定义的单文件组件
    	NonIndustrial,
    	Today,
    	Collect,
    	NonIndustrialGraph,
    	TodayGraph,
    	CollectGraph
  	},

  	data () {
  		return {
  			sheet: 3,
  			table: '表格'
  		}
  	},

  	computed: {
  		date () {
  			return getQueryVariable('RQ')===false?getNowFormatDate():getQueryVariable('RQ')
  		},
  		
  		dateFormat () {
  			let year = this.date.substr(0, 4)
  			let month = parseInt(this.date.substr(4, 2))>9?this.date.substr(4, 2):this.date.substr(5, 1)
  			let day = parseInt(this.date.substr(6, 2))>9?this.date.substr(6, 2):this.date.substr(7, 1)
  			return year + '年' + month +'月' + day +'日'
  		},

  		obj () {
  			let obj = {}
  			$.ajax({
		        url: './static/data/data.json',
		        // url: '/phone/taskToPerson/textOne',
		        data: {
		        	ReportType: '2',
		        	RQ: this.date 
		        },
		        async: false,
		        type: 'get',
		        // type: 'post',
		        dataType: 'json',
		        success: function (data) {
		            obj = data
		        }
		    });
		    return obj
  		},

  		src1 () {
  			if (this.sheet == 1) {
  				return 'fsx-fill.png'
  			} else {
  				return 'fsx.png'
  			}
  		},

  		src2 () {
  			if (this.sheet == 2) {
  				return 'jr-fill.png'
  			} else {
  				return 'jr.png'
  			}
  		},

  		src3 () {
  			if (this.sheet == 3) {
  				return 'hz-fill.png'
  			} else {
  				return 'hz.png'
  			}
  		}
  	}

}
</script>