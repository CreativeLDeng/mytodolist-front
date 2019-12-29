<template>
	<el-card shadow="hover">
		<el-checkbox v-model="checked" @change="completeTask">
		</el-checkbox>
		<span style="margin-left: 10px;" @click="editTask"> {{ content }}</span>
		<el-popover
			trigger="hover"
			style="float: right;margin: auto;">
			<el-row>
				<el-col :span="24">
					<el-button  type="text" 
								icon="el-icon-delete-solid" 
								style="width: 130px;"
								@click="deleteTask">删除</el-button>
				</el-col>
			</el-row>
			<el-row>
				<el-col :span="6"><el-button type="text" icon="el-icon-date"></el-button> </el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-refresh-left"></el-button></el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-refresh-right"></el-button></el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-d-arrow-right"></el-button></el-col>
			</el-row>
			<el-row>
				<el-col :span="6"><el-button type="text" icon="el-icon-s-flag"></el-button></el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-s-flag"></el-button></el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-s-flag"></el-button></el-col>
				<el-col :span="6"><el-button type="text" icon="el-icon-s-flag"></el-button></el-col>
			</el-row>
			<el-button slot="reference" icon="el-icon-s-operation" type="text"></el-button>
		</el-popover>
		<el-popover
			trigger="hover"
			style="float: right;margin: auto;"
			width="100px">
			<el-row v-for="(item,index) in flags" v-bind:key="index">
				<el-button type="text" size="mini">{{ item }} ✔</el-button>
			</el-row>
			<el-button slot="reference" icon="el-icon-collection-tag" type="text"></el-button>
		</el-popover>
	</el-card>
</template>

<script>
export default {
	name: 'TaskLabel',
	props: {
		content: {
			type: String,
			default: ''
		},
		flags: {
			type: Array,
			default: function () { return [] }
		}
	},
	methods: {
		editTask:function(){
			this.taskOperate(1);//编辑任务操作
		},
		completeTask:function(){
			this.taskOperate(2);//完成任务
		},
		deleteTask:function(){
			this.taskOperate(3);//删除任务
		},
		taskOperate: function(args){
			this.$emit("taskOperate",args);
		}
	},
	data() {
		return {
			checked: false
		}
	}
}
</script>

<style>
</style>
