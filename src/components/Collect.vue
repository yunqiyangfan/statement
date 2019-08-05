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
		    			<td v-for="attr in attrs">
		    				<span v-if="(attr == '日同比' || attr == '月同比' || attr == '年同比') && RegExp(/-/).exec(value[attr])" class="blDown">{{value[attr]}}</span>
		    				<span v-else-if="(attr == '日同比' || attr == '月同比' || attr == '年同比') && value[attr] != '0%'" class="blUp">{{value[attr]}}</span>
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
			attrs: ['类别', '当日', '日同期', '日同比', '当月', '月同期', '月同比', '当年', '年同期', '年同比']
		}
	},

  	props: ['obj']

}
</script>