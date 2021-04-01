<template>
	<div id="date-picker" class="date-picker">
		<date-range-picker
		ref="picker"
		:opens="'center'"
		:locale-data="{ firstDay: 1, format: 'DD-MM-YYYY HH:mm:ss' }"
		:minDate="minDate" :maxDate="maxDate"
		:singleDatePicker="'range'"
		:timePicker="true"
		:timePicker24Hour="true"
		:showWeekNumbers="false"
		:showDropdowns="false"
		:autoApply="false"
		v-model="dateRange"
		@update="updateValues"
		@select="select"
		:linkedCalendars="true"
		:ranges="false"
		
		>
		<!-- " -->
		<template v-slot:input="picker" style="min-width: 350px;">
			<div class="calendar-btn btn d-flex align-items-center justify-content-center">
				<div class="pic"><img src="./../assets/images/calendar-2103513-1772173.png" alt=""></div>
				<span v-if="picker.startDate" class="me-1">from</span>
				<b> {{picker.startDate | pickerDateFilter}} </b>
				<span v-if="picker.startDate" class="mx-1">to</span> 
				<b> {{picker.endDate | pickerDateFilter}}&nbsp;</b>
				<span v-if="!picker.startDate">choose date</span>
			</div>
		</template>
	</date-range-picker>
</div>
</template>
<script>
	import DateRangePicker from 'vue2-daterange-picker'
	import 'vue2-daterange-picker/dist/vue2-daterange-picker.css'
	export default {
		components: { DateRangePicker },
		data(){
			return {
				// opens: 'center',
				minDate: null,
				maxDate: null,

				dateRange: {
					startDate: null,
					endDate: null
				},
				// singleDatePicker: 'range',

			}
		},
		methods: {
			updateValues(){
				// console.log('@update: ',upd.startDate+'');
			},
			select(sel){
				// console.log('startDate: ',Date.parse(sel.startDate))
				// console.log('endDate: ',Date.parse(sel.endDate))

				this.$emit('rangeChosen', {
					startDate:Date.parse(sel.startDate),
					endDate: Date.parse(sel.endDate)
				});
			}
		},
		filters: {
			pickerDateFilter(value){
				if (!value) return '';
				value = value+'';
				return value.split(" ").map((item, index)=>index<4?item:'').join(" ");
				// return value;
			}
		}
	}
</script>
<style lang="less">
	// .calendar-btn{
	// 	width: 100%;
	// }
	.date-picker{
		.pic{
			width: 25px;
			margin-right: .5rem;
		}
	}
	.vue-daterange-picker{
		width: 100%;
		margin-top: 1rem;
		margin-bottom: 1rem;
		.form-control{
			border-radius: 15px;
			// width: 100%;
			&:hover{
				box-shadow: 0px 2px 1px rgba(0, 0, 0,0.25), 0px 1px 5px rgba(0, 0, 0, 0.15) inset;
			}
		}
	}
	@media screen and (min-width: 768px){
		.vue-daterange-picker{
			width: 50%;
		}
	}
</style>