<template>
	<div class="project">
		<div class="summary">
			<el-row>
				<el-col :span="12">
					<div style="margin-left: 10px;">
						<el-row><span style="font-weight: bold;">{{name}}</span></el-row>
						<el-row style="margin-top: 8px;">
							<span style="font-size:10px;">到期时间:{{endDate}}</span>
						</el-row>
						<el-row style="margin-top: 8px;">
							<el-col :span="8">
								<span style="font-size: 10px;">任务数:{{taskNum}}</span>
							</el-col>
							<el-col :span="2">
								<el-link :underline="false" @click="changeTaskListStatus">
									<span style="font-size: 10px;">{{statusText}}</span>
								</el-link>
							</el-col>
						</el-row>
					</div>
				</el-col>
				<el-col :span="3" :offset="9">
					<el-progress :percentage="percentage" type="circle" :width="60"></el-progress>
					<div>
						<span style="font-size: 10px; margin-left: 12px;">完成度</span>
					</div>
				</el-col>
			</el-row>
		</div>
		<div class="detail" v-if="taskListVisiable">
			<el-divider></el-divider>
			<slot></slot>
		</div>
	</div>
</template>

<script>
export default {
	name:'TaskProject',
	props:{
		name:{
			type:String,
			default:'项目名称'
		},
		taskNum:{
			type: Number,
			default:0
		},
		percentage:{
			type: Number,
			default: 0
		},
		endDate:{
			type:String,
			default:''
		},
	},
	methods:{
		changeTaskListStatus:function(){
			this.taskListVisiable = !this.taskListVisiable;
			if(this.taskListVisiable){
				this.statusText = "收起";
			}else{
				this.statusText = "详情";
			}
		}
	},
	data() {
		return {
			taskListVisiable:false,
			statusText:"详情"
		}
	}
}
</script>

<style>
.project{
	background-color: white;
	border: #D1DBE5;
	border-width: 1px;
	border-radius: 4px;
	box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
	padding: 10px;
}
</style>
