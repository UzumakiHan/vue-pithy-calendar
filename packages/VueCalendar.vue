<template>
  <div>
    <div class="calendar">
      <div class="header">
        <span class="picker-btn" @click="prevYear" >
           <i class="iconfont iconshuangjiantou2 "></i>
        </span>
        <span class="picker-btn " @click="prevMonth"><i class="iconfont iconshangyiyedanjiantou "></i></span>
        <span class="picker-date"> {{ year }}年{{ month }}月 </span>
        <span class="picker-btn " @click="nextMonth"><i class="iconfont iconxiayiyedanjiantou "></i></span>
        <span class="picker-btn " @click="nextYear"><i class="iconfont iconshuangjiantou1 "></i></span>
        <span class="picker-btn" @click="nowDate" style="margin-left:10px"><i class="iconfont iconjintian "></i></span>
      </div>

      <div class="content">
        <div class="picker-weeks">
          <div>日</div>
          <div>一</div>
          <div>二</div>
          <div>三</div>
          <div>四</div>
          <div>五</div>
          <div>六</div>
        </div>
        <div class="picker-days">
          <div
            v-for="(item, index) in days"
            :key="index"
            :class="[
              isCur(item).day ? 'is-today' : '',
              !isCur(item).month ? 'other-month' : '',
              isCur(item).select ? 'is-select' : '',
            ]"
            @click="clickDay(item)"
          >
            {{ item.getDate() }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'vue-pithy-calendar',
  data() {
    return {
      year: "",
      month: "",
      days: [],
      day: "",
      activeDay: "",
      showDate: null,
      date: new Date(),
      chooseDay: "",
    };
  },
  mounted() {
    this.nowDate()
  },

  methods: {
    /**
     * 根据传入的日期对象，获得包含年、月、日索引的普通对象
     * @param {Data} date - 日期对象
     * @return {Object} - 返回包含年、月、日索引的普通对象
     */
    getYearMonthDay(date) {
      // console.log(date);
      var year = date.getFullYear();
      var month = date.getMonth();
      var day = date.getDate();
      return {
        year: year,
        month: month,
        day: day,
      };
    },
    /**
     * 获取渲染日历面板显示日期的数组
     * @return {Array} - [new Date(...), new Date(...)],日历面板根据该数组进行渲染
     */
    getShowDays() {
      var arr = [];
      var firstDay = new Date(this.showDate.year, this.showDate.month, 1); //获取本月第一天的日期对象：
      var firstDayWeek = firstDay.getDay(); // 根据第一天的日期对象，获取本月的第一天是周几
      var startDay = +firstDay - firstDayWeek * 24 * 60 * 60 * 1000; // 获取本月日历起始日期
      for (var i = 0; i < 42; i++) {
        var day = new Date(startDay + i * 24 * 60 * 60 * 1000);
        arr[i] = day;
      }
      return arr;
    },
    /**
     * 判断传入的日期对象是展示日期的月份、是否为当天、是否为被选择的那一天
     * @param {Data} date - 日期对象
     * @return {Object} - 包含判断结果的值
     */
    isCur(date) {
      // 获取传入日期对象的年、月、日
      var year = this.getYearMonthDay(date).year;
      var month = this.getYearMonthDay(date).month;
      var day = this.getYearMonthDay(date).day;

      // 获取当前展示日期的年、月
      var showDate = this.showDate;
      var showYear = showDate.year;
      var showMonth = showDate.month;

      // 获取今天的年、月、日
      var curDate = this.getYearMonthDay(new Date());
      var curYear = curDate.year;
      var curmonth = curDate.month;
      var curDay = curDate.day;

      // 获取选择日期的年、月、日
      var chooseDate = this.getYearMonthDay(new Date(this.chooseDay));
      var chooseYear = chooseDate.year;
      var chooseMonth = chooseDate.month;
      var chooseDay = chooseDate.day;

      return {
        month: year == showYear && month == showMonth, // 判断当前日期是否为本月，需要年月都相同
        day: year === curYear && month === curmonth && day === curDay, // 判断当前日期是否为今天，需要年月日都相同
        select:
          year === chooseYear && month === chooseMonth && day === chooseDay, // 判断当前日期是否为选择的那天，需要年月日都相同
      };
    },
    /**
     * 获取当前选择的日期
     * @param {Date|Object} date
     * @return {String} 如 2020-4-26
     */
    getChooseDay: function (date) {
      if (date instanceof Date) {
        return (
          date.getFullYear() +
          "-" +
          (date.getMonth() + 1) +
          "-" +
          date.getDate()
        );
      } else {
        return date.year + "-" + (date.month + 1) + "-" + date.day;
      }
    },
    //点击日期
    clickDay(day) {
      this.showDate = this.getYearMonthDay(day);
      this.year = this.showDate.year;
      this.month = this.showDate.month + 1;
      this.days = this.getShowDays();
      this.chooseDay = this.getChooseDay(this.showDate); // 获取选择的日期
      this.$emit('chooseDay',this.chooseDay)
      // console.log(this.chooseDay);
    },
    //上一年
    prevYear() {
      this.year -= 1; // 更改显示日期的year
      let chooseDate = this.year + "-" + this.month + "-" + this.day;
      this.SameCode(chooseDate);
    },
    //上一个月
    prevMonth() {
      this.month -= 1; // 更改显示日期的year
      if (this.month === 1) {
        // this.month = 1;
        this.month = 12;
        this.prevYear();
      } else {
        let chooseDate = this.year + "-" + this.month + "-" + this.day;
        this.SameCode(chooseDate);
      }
    },
    //下一个月
    nextMonth() {
      this.month += 1; // 更改显示日期的year
      if (this.month > 12) {
        this.month = 1;
        this.nextYear();
      } else {
        let chooseDate = this.year + "-" + this.month + "-" + this.day;
        this.SameCode(chooseDate);
      }
    },
    //下一年
    nextYear() {
      this.year += 1; // 更改显示日期的year
      let chooseDate = this.year + "-" + this.month + "-" + this.day;
      console.log(chooseDate);
      this.SameCode(chooseDate);
    },
    SameCode(chooseDate) {
      this.showDate = this.getYearMonthDay(new Date(chooseDate));
      this.days = this.getShowDays();
    },
    //今天
    nowDate() {
      this.showDate = this.getYearMonthDay(this.date);
      this.year = this.showDate.year;
      this.month = this.showDate.month + 1;
      this.day = this.showDate.day;

      this.days = this.getShowDays();
      this.chooseDay = this.getChooseDay(this.showDate);
    },
  },
};
</script>

<style lang="scss" scoped>
.calendar {
  width: 322px;
  height: 329px;
  margin-top: 5px;
  border: 1px solid #e4e4e4;
  border-radius: 4px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  background-color: #fff;
  user-select: none;
  .header {
    padding-top: 15px;
    padding-bottom: 15px;
    text-align: center;
    span {
      vertical-align: middle;
      &.picker-btn {
        display: inline-block;
        width: 12px;
        height: 12px;
        margin: 0 3px;
        cursor: pointer;
       
      }
      &.picker-date {
        margin: 0 50px;
        font-size: 14px;
      }
    }
  }
  .content {
    padding: 0 10px 10px 10px;
    color: #606266;
    .picker-weeks {
      height: 40px;
      line-height: 40px;
      border-bottom: 1px solid #ebeef5;
      div {
        float: left;
        width: 30px;
        height: 30px;
        margin: 4px 6px;
        line-height: 30px;
        text-align: center;
        font-size: 14px;
      }
    }
    .picker-days {
      div {
        float: left;
        width: 30px;
        height: 30px;
        margin: 4px 6px;
        line-height: 30px;
        text-align: center;
        font-size: 12px;
        cursor: pointer;
        &:hover {
          color: #409eff;
        }
        &.is-today {
          color: #409eff;
          font-weight: 700;
        }
        &.is-select {
          border-radius: 50%;
          background-color: #409eff;
          color: #fff;
        }
        &.other-month {
          color: #c0c4cc;
        }
      }
    }
  }
}
</style>