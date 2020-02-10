<template>
	<div>
		<el-card shadow="hover">
			<el-input v-model="taskContent">
				<el-button id="date-btn" slot="append" @click="settingDate($event)"><span id="date-span">{{ dateTimeShowText }}</span></el-button>
			</el-input>
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
		<transition name="fade">
			<div v-if="dateSelectVisiable" class="dateSelect" :style="{'left':dateSelectPosition.x+'px','top':dateSelectPosition.y+'px'}">
				<el-button type="text" icon="el-icon-circle-close" style="margin-left: 4px;" @click="cancelSettingDateTime">取消设置</el-button>
				<calendar :value="selectedDate" @select="select"></calendar>
				<el-button type="text" @click="timeSetting($event)">{{timeSettingText}}</el-button>
				<TimePicker v-if="timeSelectVisiable" @selectedTime="getSelectedTime"></TimePicker>
			</div>
		</transition>
	</div>
</template>

<script>
import calendar from './calendar.vue'
import TimePicker from './TimePicker.vue'
export default {
	name: 'TaskEdit',
	components: {
		calendar,
		TimePicker
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
		taskOperate: function (op) {//任务操作
			this.$emit('taskOperate',op,this.taskContent);
		},
		settingDate:function(e){//设置日期
			var rectPosition = e.target.getBoundingClientRect();
			window.console.log(rectPosition);
			if(e.target.id === 'date-btn'){
				this.dateSelectPosition.x=rectPosition.left-100;
				this.dateSelectPosition.y=rectPosition.top+50+document.documentElement.scrollTop;
			}else{
				this.dateSelectPosition.x=rectPosition.left-121;
				this.dateSelectPosition.y=rectPosition.top+39+document.documentElement.scrollTop;
			}
			window.console.log(this.dateSelectPosition);
			this.dateSelectVisiable=true;
			e.flag = this.componentFlag;
		},
		timeSetting:function(e){//时间设置
			if(this.timeSelectVisiable){
				this.timeSettingText='添加时间';
			}else{
				this.timeSettingText='取消时间';
			}
			this.timeSelectVisiable = !this.timeSelectVisiable;
			e.stopPropagation();
		},
		getSelectedTime:function(arg){//获取选中的时间
			this.selectedTime = arg;
		},
		cancelSettingDateTime:function(){//取消设置日期时间
			this.selectedDate = [];
		}
	},
	data() {
		return {
			taskContent: this.content,
			dateSelectVisiable: false,
			selectedDate:[],
			selectedTime:'18:00:00',
			dateSelectPosition:{
				x:0,
				y:0
			},
			componentFlag:0,
			timeSelectVisiable:false,
			timeSettingText:'添加时间',
			select:(value)=>{
				this.selectedDate=value;
			}
		}
	},
	computed:{
		dateTimeShowText:function(){
			return this.selectedDate.length>0 ? this.selectedDate.join("/") 
			+ (this.timeSelectVisiable ? "\n"+this.selectedTime : "") :"日程选择";
		}
	},
	created() {
		this.componentFlag = Math.random();
		var _that = this;
		document.addEventListener('click',function(e){
			if(e.target.id === 'date-btn' || e.target.id === 'date-span'){
				if(e.flag != _that.componentFlag){
					_that.dateSelectVisiable = false;
				}
				e.stopPropagation();
			}else{
				_that.dateSelectVisiable =false;
			}
		},false);
	}
}
</script>

<style>
.el-card__body {
	padding: 10px;
}
.dateSelect{
	width: 200px;
	position:absolute;
	background: #fff;
	z-index: 9999;
	border-radius: 4px;
	box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
</style>
