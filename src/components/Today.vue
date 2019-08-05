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
		    				<span v-if="(attr == '在职同期比' || attr == '在岗同期比' || attr == '留存率') && RegExp(/↓/).exec(value[attr])" class="blDown">{{value[attr]}}</span>
		    				<span v-else-if="(attr == '在职同期比' || attr == '在岗同期比' || attr == '留存率') && value[attr] != '0%'" class="blUp">{{value[attr]}}</span>
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
			attrs: ['类别', '上年在职', '今年在职', '在职同期差异', '在职同期比', '上年在岗', '今年在岗', '在岗同期差异', '在岗同期比', '今年分配人数', '留存人数', '留存率', '新员工累计', '派遣工累计']
		}
	},

  	props: ['obj']

}
</script>