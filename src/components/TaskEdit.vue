<template>
	<el-card shadow="hover">
		<el-input v-model="taskContent"></el-input>
		<div style="margin-top: 0.3125rem;">
			<el-button size="medium" type="danger" @click="taskOperate(1)">保存</el-button>
			<el-button size="medium" @click="taskOperate(0)">取消</el-button>
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
	</el-card>
</template>

<script>
export default {
	name: 'TaskEdit',
	components: {
	},
	props: {
		content: {
			type: String,
			default: ""
		},
		flags: {
			type: Array,
			default: function () { return [] }
		}
	},
	methods: {
		taskOperate: function (op) {
			this.$emit('taskOperate',op,this.taskContent);
		}
	},
	data() {
		return {
			taskContent: this.content,
		}
	}
}
</script>

<style>
.el-card__body {
	padding: 10px;
}
</style>
