<template>
	<div>
		<TaskEdit v-if='edit' :content='content' 
							:flags='flags' 
							@taskOperate="editOp(arguments)">
		</TaskEdit>
		<TaskLabel v-else   :content='content' 
							:flags='flags' 
							@taskOperate="labelOp">
		</TaskLabel>
	</div>	
</template>

<script>
import TaskEdit from './TaskEdit.vue'
import TaskLabel from './TaskLabel.vue'
export default {
	name: 'TaskShow',
	components:{
		TaskEdit,
		TaskLabel
	},
	props: {
		content: {
			type: String,
			default: ""
		},
		flags: {
			type: Array,
			default: function () { return ['工作','生活','学习','其它'] }
		}
	},
	methods: {
		labelOp:function(arg){
			if(1 === arg){//打开编辑界面
				this.edit = !this.edit;
			}else if(2 === arg){//完成任务
				var op = {type:2,taskconent:''};
				this.taskOperate(op);
			}else if(3 === arg){//删除任务
				var op1 = {type:3,taskconent:''};
				this.taskOperate(op1);
			}
		},
		editOp:function(args){
			if(0 === args[0]){
				this.edit = false;//取消编辑内容
			}else if(1 === args[0]){
				this.edit = false;//保存编辑内容
				var op = {type:1,taskconent:args[1]};
				this.taskOperate(op);
			}
		},
		taskOperate: function(op) {//数组中第一个元素为操作方式，第二元素为内容
			//1保存编辑，2完成任务，3删除任务
			this.$emit('taskOperate',op);
		},
		deleteTask:function(){
			var args = [3,this.content];
			this.taskOperator(args);
		}
	},
	data() {
		return {
			edit: false
		}
	}
}
</script>

<style>
</style>
