<template>
	<el-dialog
	title="快速添加任务"
	:show-close="false"
	:close-on-click-modal="false"
	:close-on-press-escape="false"
	top="20vh"
	:visible.sync="visibleDialog">
		<el-input v-model="taskcontent"></el-input>
		<div style="margin-top: 0.3125rem;">
			<el-button size="medium" type="danger" @click="taskOp(1)">保存</el-button>
			<el-button size="medium" @click="taskOp(0)">取消</el-button>
			<el-popover
				trigger="hover"
				style="float: right;margin: auto;">
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
		</div>
	</el-dialog>
</template>

<script>
export default {
	name: 'TaskAddDialog',
	components: {
	},
	props: {
		flags:{
			type: Array,
			default: function () { return [] }
		},
		showAddDialog :{
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			taskcontent: '',
			visibleDialog: false
		}
	},
	methods: {
		taskOp: function(op){
			this.$emit('taskOp',op,this.taskcontent);
		}
	},
	watch:{
		showAddDialog:function (newValue) {
			this.visibleDialog = newValue;
		}
	}
}
</script>

<style>
</style>
