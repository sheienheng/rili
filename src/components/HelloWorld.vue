<template>
    <div id="dateContainer">
      今天是 <span v-text="ynow2"></span>年<span v-text="mnow2+1"></span>月<span v-text="dnow2"  @click="addM(1)"></span>日<br>
      <button class="preMon" @click="addM(-1)">上个月</button>
        <div class="thisMon">
            <span v-text="ynow"></span>/
          <span v-text="mnow+1"></span>
        </div>
        <button class="nextMon" @click="addM(1)">下个月</button>
      <table border="1">
        <tr>
          <td>星期一</td>
          <td>星期二</td>
          <td>星期三</td>
          <td>星期四</td>
          <td>星期五</td>
          <td class="isRed">星期六</td>
          <td class="isRed">星期日</td>
        </tr>
        <tr v-for="(item,index) in 5" :key="index">
          <td :class="(houxu>6&&index===4)||(index===0&&xingqi>1)?'isDD':''">{{arrList[index*7+0]}}</td>
          <td :class="(houxu>5&&index===4)||(index===0&&xingqi>2)?'isDD':''">{{arrList[index*7+1]}}</td>
          <td :class="(houxu>4&&index===4)||(index===0&&xingqi>3)?'isDD':''">{{arrList[index*7+2]}}</td>
          <td :class="(houxu>3&&index===4)||(index===0&&xingqi>4)?'isDD':''">{{arrList[index*7+3]}}</td>
          <td :class="(houxu>2&&index===4)||(index===0&&xingqi>5)?'isDD':''">{{arrList[index*7+4]}}</td>
          <td class="isRed" :class="(houxu>2&&index===4)||(index===0&&xingqi>6)?'isDD':''">{{arrList[index*7+5]}}</td>
          <td class="isRed" :class="(houxu>1&&index===4)||(index===0&&xingqi>7)?'isDD':''">{{arrList[index*7+6]}}</td>
        </tr>
</table>

    </div>
</template>
<script>
  export default {
    data() {
      return {
        newDate: '',//当前的日期的信息
        ynow: '',//当前的年数
        mnow: '',//当前的月份
        dnow: '',//当前的天数
        ynow2: '',//当前的年数
        mnow2: '',//当前的月份
        dnow2: '',//当前的天数
        firstDay: '',//第一天
        firstnow: '',//当前的星期
        m_days: [],
        tr_str: '',
        arrList: [],
        xingqi: 0,
        houxu: 0
      }
    },
    methods: {
      addM (num) {
        var _this = this;
        if (num === 1 && this.mnow === 11) {
          this.mnow = 0
          this.ynow = this.ynow + 1
        } else if (num === -1 && this.mnow < 1) {
          this.mnow = 11
          this.ynow = this.ynow - 1
        } else {
          this.mnow = this.mnow + num
        }
        this.setData()
      },
      setData(){
        this.m_days = [31,28+this.is_leap(this.ynow),31,30,31,30,31,31,30,31,30,31];
        this.xingqi = this.oneDay(this.ynow,this.mnow+1,1)
        let m = this.mnow-1;
        let mA = this.mnow+1;
        if (this.mnow===0) {
          m = 11
        }
        if (this.mnow===11) {
          mA = 0
        }
        let arr = []
        if (this.xingqi>1) {
          for (let i = this.m_days[m];i > this.m_days[m]-this.xingqi+1;i--) {
            arr.push(i)
          }
        }
        arr = arr.reverse()
        this.houxu = 35 - this.xingqi + 1 - this.m_days[this.mnow]
        let arr2 = []
        for (let i = 0;i < this.m_days[this.mnow];i++) {
            arr2.push(i+1)
          }
          let arr3 = []
        for (let i = 0;i < this.m_days[mA];i++) {
          arr3.push(i+1)
        }
        this.arrList = [...arr,...arr2,...arr3]
      },
      is_leap(year) {
        return (year%100==0?(year%400==0?1:0):(year%4==0?1:0));
      },
      oneDay(y,m,d){
          var myDate=new Date();
          myDate.setFullYear(y,m-1,d);
          var week = myDate.getDay()
          if (week === 0) {
            week = 7
          }
          return week
      }

    },
    mounted() {
      this.newDate = new Date();
      this.ynow = this.newDate.getFullYear();
      this.mnow=this.newDate.getMonth()
      this.dnow=this.newDate.getDate()
      this.ynow2 = this.newDate.getFullYear();
      this.mnow2=this.newDate.getMonth()
      this.dnow2=this.newDate.getDate()
      this.setData()
    }
  }
</script>
<style>
  td{
    width: 200px;
    height: 120px;
  }
  .isRed{
    color: red
  }
  .isDD{
    color: #ddd;
  }
</style>
