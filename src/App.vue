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
				<el-menu default-active="2"
						@select="selectMenu">
					<el-menu-item index="1">
						<i></i>
						<span>常态任务</span>
					</el-menu-item>
					<el-menu-item index="2">
						<i></i>
						<span>今天</span>
					</el-menu-item>
					<el-menu-item index="3">
						<i></i>
						<span>标签</span>
					</el-menu-item>
					<el-menu-item index="4">
						<i></i>
						<span>项目</span>
					</el-menu-item>
					<el-menu-item index="5">
						<i></i>
						<span>已完成任务</span>
					</el-menu-item>
				</el-menu>
			</el-aside>
			<el-main style="background-color: #e7faff;">
				<el-col :span="16">
					<div v-if="showMenuIndex === '1'">
						<el-row>
							<span style="font-size: 20px;font-weight: bold;">待办事项</span>
						</el-row>
						<el-divider></el-divider>
						<TaskShow v-for="(taskItem,index) in taskList"
								:content="taskItem.taskconent"
								:key="index"
								:flags="flags" 
								@taskOperate="taskShowOp(arguments,index)">
						</TaskShow>
					</div>
					<div class="contentcontainer" v-if="showMenuIndex === '2'">
						<el-row>
							<el-col :span="12">
								<div>
									<span style="font-size: 20px;font-weight: bold;">今日待办</span>
								</div>
								<div>
									<span style="font-size: 15px;">{{today.getFullYear()+"年"+today.getMonth()+1+"月"+today.getDate()+"日"}}</span>
								</div>
							</el-col>
							<el-col :span="4" :offset="8">
								<el-progress type="circle" :percentage="30" :width="60"></el-progress>
							</el-col>
						</el-row>
						<el-divider></el-divider>
						<TaskShow v-for="(taskItem,index) in taskList"
								:content="taskItem.taskconent"
								:key="index"
								:flags="flags" 
								@taskOperate="taskShowOp(arguments,index)">
						</TaskShow>
					</div>
					<div v-if="showMenuIndex === '3'">
						<el-row>
							<span style="font-size: 20px;font-weight: bold;">标签管理</span>
						</el-row>
						<el-divider></el-divider>
						<el-row>
							<el-tag
								:key="flag"
								v-for="flag in flags"
								closable
								:disable-transitions="false"
								@close="handleClose(flag)">
								{{flag}}
							</el-tag>
							<el-input
								class="input-new-tag"
								v-if="addTagInputVisible"
								v-model="addTagValue"
								ref="saveTagInput"
								size="small"
								@keyup.enter.native="handleInputConfirm"
								@blur="handleInputConfirm"
							>
							</el-input>
							<el-button v-else class="button-new-tag" size="small" @click="showInput">+ New Tag</el-button>

						</el-row>
					</div>
					<div v-if="showMenuIndex === '4'">
						<el-row>
							<span style="font-size: 20px;font-weight: bold;">项目</span>
						</el-row>
						<el-divider></el-divider>
						<TaskProject v-for="(project,index) in projectList" :key=index 
							:name="project.name"
							:taskNum="project.taskNum"
							:percentage="project.percentage"
							:endDate="project.endDate">
							<TaskShow v-for="(task,ind) in project.taskList" :key=ind
										:content="task.taskconent">
							</TaskShow>
						</TaskProject>
					</div>
					<div v-if="showMenuIndex === '5'">
						<el-row>
							<el-col :span="12">
								<span style="font-size: 20px;font-weight: bold;">已完成任务</span>
							</el-col>
							<el-col :span="8" :offset="3">
								<el-date-picker
									v-model="week"
									type="week"
									size="small"
									format="yyyy 第 WW 周"
									placeholder="选择周">
								</el-date-picker>
							</el-col>
						</el-row>
						<el-divider></el-divider>
						<el-timeline :reverse="reverse">
							<el-timeline-item
							v-for="(dayItem, index) in weekCompletedTasks"
							:key="index"
							:timestamp="dayItem.completedDate"
							placement="top"
							type="primary"
							value-format="yyyy-MM-dd">
							<el-card shadow="hover">
								<el-row v-for="(taskItem,taskIndex) in dayItem.dayCompletedTasks" :key="taskIndex">
									<el-col :span="21">
										<span>🔸 {{taskItem.taskName}}</span>
									</el-col>
									<el-col :span="3">
										{{taskItem.completedTime}}
									</el-col>
								</el-row>
							</el-card>
							</el-timeline-item>
						</el-timeline>
					</div>
				</el-col>
				<el-col :span="8">
					<div class="rightcontainer">
						<div style="position: absolute;">
							<calendar
								:events="calendar.events"
								:value="calendar.value">
						</calendar>
						</div>
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
import TaskProject from './components/TaskProject.vue'
import calendar from './components/calendar.vue'
export default {
  name: 'app',
  components: {
	TaskShow,
	TaskAddDialog,
	TaskProject,
	calendar
  },
  data() {
    return {
		today: new Date(),
		showMenuIndex: '2',
		flags : ['工作','生活','学习','其它'],
		addTagInputVisible: false,
		addTagValue: '',
		showAddDialog : false,
		content: '测试内容',
		taskList: [
			{
				taskconent:"测试内容1"
			},
			{
				taskconent:"测试内容2"
			}
		],
		projectList:[
			{
				name:'工程一',
				endDate:'2019-01-01',
				taskNum:2,
				percentage:20,
				taskList:[
					{
						taskconent:"测试内容1"
					},
					{
						taskconent:"测试内容2"
					}
				]
			}
		],
		reverse: true,
		week:'2020-01-20',
		weekCompletedTasks:[
			{
				dayCompletedTasks:[
					{
						taskName:'任务1',
						completedTime:'15:32'
					},
					{
						taskName:'任务2',
						completedTime:'18:33'
					}
				],
				completedDate:'2019-01-01',
			},
			{
				dayCompletedTasks:[
					{
						taskName:'任务1',
						completedTime:'15:32'
					},
					{
						taskName:'任务2',
						completedTime:'18:33'
					}
				],
				completedDate:'2019-01-02',
			},
			{
				dayCompletedTasks:[
					{
						taskName:'任务1',
						completedTime:'15:32'
					},
					{
						taskName:'任务2',
						completedTime:'18:33'
					}
				],
				completedDate:'2019-01-03',
			}
		],
		calendar:{
			value:[2020,1,20], //默认日期
			// lunar:true, //显示农历
			events:{
				'2017-7-7':'$408',
				'2017-7-20':'$408',
				'2017-7-21':'$460',
				'2017-7-22':'$500',
			},
			// select(value){
			//     // console.log(value.toString());
			// },
			// selectMonth(month,year){
			//     // console.log(year,month)
			// },
			// selectYear(year){
			//     // console.log(year)
			// },
			timestamp:Date.now()
		}
    }
  },
  methods: {
	selectMenu:function(key){
		this.showMenuIndex = key;
	},
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
	},
	handleClose:function(tag) {
		this.flags.splice(this.flags.indexOf(tag), 1);
	},
	showInput:function() {
		this.addTagInputVisible = true;
		this.$nextTick( ()=> {
		this.$refs.saveTagInput.$refs.input.focus();
		});
	},
	handleInputConfirm:function() {
		let inputValue = this.addTagValue;
		if (inputValue) {
			this.flags.push(inputValue);
		}
		this.addTagInputVisible = false;
		this.addTagValue = '';
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
.el-tag + .el-tag {
    margin-left: 10px;
  }
.button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
}
.input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
}
</style>
