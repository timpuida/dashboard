<template>
  <div class="dashboard" id="dashboard">
    <div class="container">
      <chart
      :incomes="incomes"
      :minDate="startDate"
      :maxDate="endDate"
      ref="chart"
      />
      <date-picker
      @rangeChosen="filterByDate"
      />
      <div class="incomes">
        <div 
        v-for="(income,index) in incomes"
        :key="income.id"
        class="income-wrap"
        >
          <div class="income d-flex justify-content-between align-items-center fw-bold">
            <div class="d-flex align-items-center">
              <div class="li" :style="{backgroundColor:income.color}"></div>
              <p class="mb-0 ps-1">{{income.name | upperCase}}</p>
            </div>
            <div class="sum"> $ {{incomeCount(index)}} </div>
          </div>
        </div>
        <hr class="hr-blue">
        <p class="fw-bold"> TOTAL: <span style="font-size: 1.2rem;">{{totalCount | round}}</span> </p>
      </div>
      <!-- <div class="months"></div> -->
    </div>
  </div>
</template>

<script>
  import chart from './chart.vue'
  import datePicker from './datepicker.vue'

export default {
  name: 'dashboard',
  components: {
    chart,
    datePicker  
  },
  data(){
    return {
      startDate:null,
      endDate:null,
      incomes:[
      {
        name:"subscriptions",
          data: [
          {
            time:1616922000000,
            count:5416.23
          },
          {
            time:1617008400000,
            count:8416.23
          },
          {
            time:1617094800000,
            count:10416.23
          },
          ],
          color: '#0d6efd',
          id:0

        },
        {
          name:"tips",
          data: [
          {
            time:1616922000000,
            count:1565
          },
          {
            time:1617008400000,
            count:1645.23
          },
          {
            time:1617094800000,
            count:1783.97
          },
          ],
          // count: 1783.97,
          color: '#6610f2',
          id:1

        },
        {
          name:"posts",
          data: [
          {
            time:1616922000000,
            count:0
          },
          {
            time:1617008400000,
            count:0
          },
          {
            time:1617094800000,
            count:0
          },
          ],
          color: '#6f42c1',
          id:2

        },
        {
          name:"messages",
          data: [
          {
            time:1616922000000,
            count:6141.61
          },
          {
            time:1617008400000,
            count:12141.61
          },
          {
            time:1617094800000,
            count: 26141.61
          },
          ],
          color: '#d63384',
          id:3

        },
        {
          name:"referrals",
          data: [
          {
            time:1616922000000,
            count:11
          },
          {
            time:1617008400000,
            count:22
          },
          {
            time:1617094800000,
            count: 38.34
          },
          ],
          color: '#dc3545',
          id:4

        },
        {
          name:"streams",
          data: [
          {
            time:1616922000000,
            count:0
          },
          {
            time:1617008400000,
            count:1
          },
          {
            time:1617094800000,
            count: 2
          },
          ],
          color: '#fd7e14',
          id:5

        }
        ]
      }
    },
  created(){
    console.clear()
  },
  methods: {
    filterByDate(data){
      console.log('data:',data);
      this.startDate= data.startDate;
      this.endDate = data.endDate;
      
      this.$refs.chart.buildChart(this.startDate,  this.endDate);


    },
    incomeCount(i){
      // let order = this.incomes[i].data.length-1;
      // this.endData
      let time = this.incomes[i].data.map(d=>{
        if(!this.endDate){
          return d.time
        }else if (d.time && d.time<=this.endDate) {
        return d.time
      } else return 0
      });
      // console.log('timeArr:',time);
      time=Math.max(...time);
      let count = this.incomes[i].data.find(el=>el.time===time)?.count
      // console.log('time: ',time);  
      return count?count:0
      // return this.incomes[i].data[order].count;
    },
  },
  filters: {
    upperCase(val){
      return val.toUpperCase();
    },
    round(val){
      return Math.round(val*100)/100
    }
  },
  computed: {
    totalCount(){
      // let buff = this.incomes.filter(obj=>obj.currency);

      // buff=buff.map(item=>item.data[item.data.length-1].count);
      // return buff.reduce((prev,sum)=>prev+sum);
      let arr=[];
      for (let i =0; i<this.incomes.length; i++){
        arr.push(this.incomeCount(i))
      }
      // console.log('arr:',arr);
      return arr.reduce((prev,sum)=>prev+sum)
    }
  }
 
}
</script>

<style lang="less" scoped>
    .li{
      border-radius: 50%;
      flex-shrink: 0;
      width: 8px;
      height: 8px;
    }
    hr{
      width: 100%;
    }
    .income{
      &-wrap{
        &:not(:last-of-type){
          border-bottom: 1px solid #ccc;
        }
      }
      padding: 1.5rem;
    }
    .hr-blue{
      margin-top: 0;
      height:2px;
      background: #FF3E9D;
    }
</style>
