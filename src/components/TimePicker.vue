<template>
	<div>
		<div @click="selectTime($event)">
			{{h}}:{{m}}:{{s}}
		</div>
		<div v-if="timeListVisiable" class="timeBox">
			<el-col :span="8">
				<div class="time">
					<span v-for="hour in hours" :key="hour" @click="selectHour($event,hour)">{{hour}}</span>
				</div>
			</el-col>
			<el-col :span="8">
				<div class="time">
					<span v-for="m in ms" :key="m" @click="selectMinute($event,m)">{{m}}</span>
				</div>
			</el-col>
			<el-col :span="8">
				<div class="time">
					<span v-for="s in ms" :key="s" @click="selectSecond($event,s)">{{s}}</span>
				</div>
			</el-col>
		</div>
	</div>
</template>

<script>
export default{
	data(){
		return {
			hours:['01','02','03','05','06','07','08','09','10','11','12',
			'13','14','15','16','17','18','19','20','21','22','23','24'],
			ms:['00','01','02','03','05','06','07','08','09','10',
			'11','12','13','14','15','16','17','18','19','20',
			'21','22','23','24','25','26','27','28','29','30',
			'31','32','33','34','35','36','37','38','39','40',
			'41','42','43','44','45','46','47','48','49','50',
			'52','53','54','55','57','58','59'],
			h:'18',
			m:'00',
			s:'00',
			timeListVisiable:false
		}
	},
	methods:{
		selectHour:function(e,h){
			this.h = h;
			e.stopPropagation();
			this.$emit("selectedTime",this.h+":"+this.m+":"+this.s);
		},
		selectMinute:function(e,m){
			this.m = m;
			e.stopPropagation();
			this.$emit("selectedTime",this.h+":"+this.m+":"+this.s);
		},
		selectSecond:function(e,s){
			this.s = s;
			this.timeListVisiable = !this.timeListVisiable;
			e.stopPropagation();
			this.$emit("selectedTime",this.h+":"+this.m+":"+this.s);
		},
		selectTime:function(e){
			this.timeListVisiable = !this.timeListVisiable;
			e.stopPropagation();
		}
	}
}
</script>

<style>
.timeBox{
	height: 100px;
}
.time{
	height: 100px;
	display: block;
	align-items: center;
	justify-content:center;
	overflow:overlay
}
.time::-webkit-scrollbar{
	width: 4px;
}
.time::-webkit-scrollbar-thumb{
	border-radius: 10px;
	-webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
	background: rgba(0,0,0,0.2);
}
.time>span{
	display: block;
	padding-top: 3px;
	padding-left: 15px;
}
.time>span:hover{
	background-color: #00FFFF;
}
</style>
