<template>
	<div>
	  	<div>
	  		<table cellpadding="0px" cellspacing="0px">
		    	<thead>
		    		<tr>
		    			<th v-for="attr in attrs">{{attr}}</th>
		    		</tr>
		    	</thead>
		    	<tbody v-if="obj.length > 0">
		    		<tr v-for="(value, index) in obj" :class="'tr' + (index%2 + 1)">
		    			<td v-for="(attr, col) in attrs" :class="col>attrs.length-5?'td'+(index%2+1):''">
		    				<span v-if="(attr == '同期比' && RegExp(/↓/).exec(value[attr])) || (attr == '同期差异' && parseInt(value[attr]) < 0)" class="blDown">{{value[attr]}}</span>
		    				<span v-else-if="(attr == '同期比' && value[attr] != '0%') || (attr == '同期差异' && value[attr] != '0')" class="blUp">{{value[attr]}}</span>
		    				<span v-else>{{value[attr]}}</span>
		    			</td>
		    		</tr>
		    	</tbody>
		    	<tbody v-else> 
		    		<tr style="text-align: center; color: red;"><td :colspan="attrs.length">未查询到数据...</td></tr>
		    	</tbody>
		    </table>
	  	</div>
	</div>
</template>

<script>
import $ from 'jquery';

export default {

	data () {
		return {
			attrs: ['类别', '行政', '业务', '技术', '生产管理', '后勤', '仓储', '检测', '维修', '物流', '去年非生产性', '今年非生产性', '同期差异', '同期比']
		}
	},

  	props: ['obj']

}
</script>