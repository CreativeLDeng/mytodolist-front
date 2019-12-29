<template>
  <div id="app">
    <el-container class="globalcontainer">
		<el-header class="header">
				<el-row>
					<el-col :span="7">
						<div class="logo">
							<el-avatar shape="square" :size="50" fit="fill" :src="require('@/assets/logo.png')"></el-avatar>
						</div>
					</el-col>
					<el-col :span="10">
						<div class="headerheight search">
							<el-input placeholder="请输入任务" size="medium">
								<el-button slot="append" icon="el-icon-search"></el-button>
							</el-input>
						</div>
					</el-col>
					<el-col :span="7">
						<div class="headerheight add">
							<el-button icon="el-icon-plus" size="mini" @click="addTask"></el-button>
						</div>
					</el-col>
				</el-row>
		</el-header>
		<el-container>
			<el-aside class="menucontainer">
				<el-menu>
					<el-menu-item>
						<i></i>
						<span>常态任务</span>
					</el-menu-item>
					<el-menu-item>
						<i></i>
						<span>今天</span>
					</el-menu-item>
					<el-menu-item>
						<i></i>
						<span>标签</span>
					</el-menu-item>
					<el-menu-item>
						<i></i>
						<span>项目</span>
					</el-menu-item>
				</el-menu>
			</el-aside>
			<el-main style="background-color: #00FF00;">
				<el-col :span="16">
					<div class="contentcontainer">
						<el-progress :text-inside="true" :stroke-width="26" :percentage="70"></el-progress>
						<TaskShow v-for="(taskItem,index) in taskList"
								:content="taskItem.taskconent"
								:key="index"
								:flags="flags" 
								@taskOperate="taskShowOp(arguments,index)">
						</TaskShow>
					</div>
				</el-col>
				<el-col :span="8">
					<div class="rightcontainer">
						<el-calendar>
							<template
							slot="dateCell"
							slot-scope="{date, data}">
								<p :class="data.isSelected ? 'is-selected' : ''">
									{{ data.day.split('-')[2] }} {{ data.isSelected ? '✔️' : ''}}
								</p>
							</template>
						</el-calendar>
					</div>
				</el-col>
			</el-main>
		</el-container>
    </el-container>
	<TaskAddDialog :flags="flags" :showAddDialog="showAddDialog" @taskOp="taskOp(arguments)"></TaskAddDialog>
  </div>
</template>

<script>
import TaskShow from './components/TaskShow.vue'
import TaskAddDialog from './components/TaskAddDialog.vue'
export default {
  name: 'app',
  components: {
	TaskShow,
	TaskAddDialog
  },
  data() {
    return {
		flags : ['工作','生活','学习','其它'],
		showAddDialog : false,
		content: '测试内容',
		taskList: [
			{
				taskconent:"测试内容1"
			},
			{
				taskconent:"测试内容2"
			}
		]
    }
  },
  methods: {
	taskShowOp: function(args,index){
		if(1 === args[0].type){//保存编辑内容
			this.taskList[index].taskconent = args[0].taskconent;
		}else if(2 === args[0].type){//完成任务
			alert("完成任务");
		}else if(3 === args[0].type){//删除任务
			this.taskList.splice(index,1);
		}
	},
	taskOp: function(args){
		if(args[0] === 0){
			this.showAddDialog = false;
		}else if(args[0] === 1){
			this.taskList.push({taskconent:args[1]});
			this.showAddDialog = false;
		}else if(args[0] === 3){
			this.taskList.slice()
		}
	},
	addTask: function(){
		this.showAddDialog = true;
	}
  }
}
</script>

<style>
.globalcontainer{
  width: -webkit-fill-available;
  height:1000px
}
.header{
	height: 3.125rem;
	background: #F56C6C;
	width: -webkit-fill-available;
}
.logo{
	text-align: right;
	margin-right: 100px;
}
.menucontainer{
	width: 25%;
	background-color: #D3DCE6;
}
.contentcontainer{
	background-color: #;
	height: 1000px;
}
.rightcontainer{
	background-color: #72767B;
	height: 1000px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  /* color: #2c3e50; */
  /* margin-top: 60px; */
}
.headerheight{
    height: 3.125rem;
}
.search{
	display: flex;
    align-items: center;
    justify-content: center;
/* margin-top: 0.5rem; */
}
.add{
	display: flex;
	align-items: center;
	text-align: left;
	margin-left: 0.625rem;
}
.is-selected {
    color: #1989FA;
}
</style>
