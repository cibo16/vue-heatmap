<template>
  <div >
    <table>
      <tr>
        <th></th>
        <th v-for="row in rowMonths" :key="row" :colspan="row.count" style="text-align:left;font-size:9px"> {{row.count > 1 ? row.month.slice(0, -2) : ""}}</th>
      </tr>
      <tr>
        <td></td>
        <td :title="sunday+' : '+(dates[sunday] || 0)" v-for="sunday in days.sundays" :key="sunday" style="margin-left:1px;">
          <svg   width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[sunday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px">Mon</td>
        <td :title="monday+' : '+(dates[monday] || 0)" v-for="monday in days.mondays" :key="monday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[monday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px"></td>
        <td :title="tuesday+' : '+(dates[tuesday] || 0)" v-for="tuesday in days.tuesdays" :key="tuesday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[tuesday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px">Wed</td>
        <td :title="wednesday+' : '+(dates[wednesday] || 0)" v-for="wednesday in days.wednesdays" :key="wednesday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[wednesday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px"></td>
        <td :title="thursday+' : '+(dates[thursday] || 0)" v-for="thursday in days.thursdays" :key="thursday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[thursday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px">Fri</td>
        <td :title="friday+' : '+(dates[friday] || 0)" v-for="friday in days.fridays" :key="friday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[friday])" />
          </svg> 
        </td>
      </tr>
      <tr>
        <td style="text-align:left;font-size:9px"></td>
        <td :title="saturday+' : '+(dates[saturday] || 0)" v-for="saturday in days.saturdays" :key="saturday" style="margin-left:1px;">
          <svg width="11" height="11">
            <rect width="100%" height="100%" :fill="color(dates[saturday])" />
          </svg> 
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import moment from "moment"

export default {
  props: {
    dates: Object
  },
  data() {
    return {
      days: {},
      rowMonths: []
    }
  },
  mounted() {
    var end = moment();
    var start = moment().subtract(365 , 'd');

    function getDaysBetweenDates(start, end, dayName) {
      var days = {sunday:0,monday:1,tuesday:2,wednesday:3,thursday:4,friday:5,saturday:6};
      var day = days[dayName];
      var first = start.clone().day(day);
      var result = [first.format('YYYY-MM-DD')];

      while( first.isBefore(end) ){
        first.add(7, 'days');
        result.push(first.format('YYYY-MM-DD'));
      }
      return result;  
    }
    this.days.mondays = getDaysBetweenDates(start, end, "monday");
    this.days.tuesdays = getDaysBetweenDates(start, end, "tuesday");
    this.days.wednesdays = getDaysBetweenDates(start, end, "wednesday");
    this.days.thursdays = getDaysBetweenDates(start, end, "thursday");
    this.days.fridays = getDaysBetweenDates(start, end, "friday");
    this.days.saturdays = getDaysBetweenDates(start, end, "saturday");
    this.days.sundays = getDaysBetweenDates(start, end, "sunday");

    const obj = {};
    this.days.sundays.map(element => {
      obj[moment(element).format('MMMYY')] = (obj[moment(element).format('MMMYY')]+1) || 1 ;
    })
    Object.keys(obj).forEach(key => {
      let obj2 = {month: key, count:obj[key]};
      this.rowMonths.push(obj2);

    });
  },
  methods: {
    color (contribution) {
      if( typeof contribution === 'undefined' || contribution === null ){
          contribution = 0;
      }
      var color = '';
      if (contribution == 0) {
        color = "#ededed";
      } else if(contribution<=9){
        color = "#acd6f2";
      } else if(contribution<=19){
        color = "#7fa8c9";
      } else if(contribution<=29){
        color = "#527ba0";
      } else if(contribution>=30){
        color = "#264e77";
      }
      return color;
    }

  }
}
</script>

<style scoped>
</style>
