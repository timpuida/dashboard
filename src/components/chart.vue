	<script>

		// import { Bar } from 'vue-chartjs'
		import { Line } from 'vue-chartjs'

		export default {
			extends: Line,
			props:{
				incomes:{
					type: Array,
					default: ()=>[] 
				},
				minDate:{
					type: Number,
					default: 0
				},
				maxDate: {
					type: Number,
					default: Infinity
				}
			},
			// props: ['chartdata', 'options'],
			data(){
				return {
					// type: 'line',
					options: {
						responsive: true,
						maintainAspectRatio: false,
						aspectRatio: 1,
						scales: {
							yAxes: [{
								display:false
							}]
						}
					},
					chartdata: {
						labels: [],
						datasets: [
						// {// 	// fill:false, // 	// label: '# of Votes', // 	// borderColor: "#123", // 	// data: [12, 19, 3], // 	// backgroundColor: [// 	// 'rgba(255, 159, 64, 0.2)'// 	// ], // },{// 	// fill:false, // 	// label: 'subscriptions', // 	// data: [0, 3, 8], // 	// backgroundColor: [// 	// 'rgba(255, 159, 64, 0.2)'// 	// ], // }
						],
						}


					}
				},
				methods:{
					buildChart(minTime,maxTime){
						let length = this.incomes.length;
						let start = 0;
						for (let index=start; index<length; index++){
							// if (this.incomes[index])
							let obj = this.chartdata.datasets;
							obj[index]={};
							obj[index].fill = false;

							obj[index].backgroundColor = this.incomes[index].color;
							obj[index].label = this.incomes[index].name;
							obj[index].borderColor = this.incomes[index].color;
							obj[index].data = this.incomes[index].data.map(item=>{

							if (item.time>=minTime && item.time<=maxTime){
							return item.count
							}else return
							});
						}

						this.chartdata.labels = this.incomes[0].data.map((i)=>{
							let date = new Date(i.time)+"";
							return date.split(" ").map((item, index)=>index<4?item:'').join(" ");
						});

						this.renderChart(this.chartdata, this.options);
					}
				},
				mounted () {
					this.buildChart(0,Infinity)
					if( window.innerWidth<768){
						document.querySelector('#line-chart').style.maxHeight = '150px';
					}
					// this.chartdata.datasets.length = this.incomes.length;
					// this.buildChart()
					
					
					// this.chartdata

					
				}
			}
		</script>