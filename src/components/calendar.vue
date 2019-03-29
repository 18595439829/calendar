<template>
  <div class="calendar">
    <div class="nav">
      <div class="date">{{date}}</div>
      <div class="today" @click="todayClick">今天</div>
    </div>
    <div class="content">
      <div class="weeks">
        <span class="week">日</span>
        <span class="week">一</span>
        <span class="week">二</span>
        <span class="week">三</span>
        <span class="week">四</span>
        <span class="week">五</span>
        <span class="week">六</span>
      </div>
      <div class="days" 
                      @touchstart='touchStart' 
                      @touchmove='touchMove' 
                      @touchend='touchEnd'>
        <div v-for="item in calendar" class="day" @click="dayClick(item.date)">
          <span :class="{today: item.date === today}" :style="{opacity: item.day === '无' ? '0' : '1'}">
            {{item.day}}
          </span>
        </div>
      </div>
      <div class="plan" :style="{top: planTop}">
        <div class="arrows">
          <span v-if="arrows" class="arrowsBot" @click="arrows = false">》</span>
          <span v-if="!arrows" class="arrowsTop" @click="arrows = true">》</span>
        </div>
        <div class="warning">
          <div v-for="(item,index) in warningTitle" @click="warningClick(index)" :class="{clicked: index === current}">{{item.title}}</div>
        </div>
        <div v-if="current === 0" class="warningContents">
          <div class="warningContent" v-for="item in warningContent">
            <div class="warningTitle">
              <div class="left">
                {{item.title}}
              </div>
              <div class="right">
                {{item.time}}
              </div>
            </div>
            <div class="warningDesc">
              {{item.description}}
            </div>
            <div class="warningRole">
              <div class="left" :style="{color: item.roleColor}">
                {{item.role}}
              </div>
              <div class="right">
                ...
              </div>
            </div>
          </div>
        </div>
        <div v-if="current === 1" class="warningOther">
          创建时间
        </div>
        <div v-if="current === 2" class="warningOther">
          最后发言时间
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'calendar',
  data() {
    return {
      date: '2018-12-12', // nav展示日期
      // 今天的日期
      today: '',
      arrows: true,  // 展开收起箭头
      calendar: [],  //当前展示日历数组
      warningTitle: [
        {
          title: '已设置提醒'
        },
        {
          title: '创建时间'
        },
        {
          title: '最后发言时间'
        }
      ],
      current: 0,
      // 提醒内容
      warningContent: [
        {
          title:'227市场',
          time: '16:24',
          description: '定价依据和客户判定',
          role: '我的角色: 经办人',
          roleColor: 'rgb(90,161,211)'
        },
        {
          title:'227市场',
          time: '16:24',
          description: '定价依据和客户判定',
          role: '我的角色: 经办人',
          roleColor: 'rgb(255,157,9)'
        },
        {
          title:'227市场',
          time: '16:24',
          description: '定价依据和客户判定',
          role: '我的角色: 经办人',
          roleColor: 'rgb(255,157,9)'
        }
      ],
      planTop: '5.1rem'
    }
  },
  watch: {
    arrows(v){
      if (v) {
        // 收起状态
        this.planTop = '5.1rem'
      } else {
        this.planTop = `${this.calendar.length / 7 * 2 + 3.1 }rem`
      }
    }
  },
  methods: {
    // 日期转化函数
    setDate(date) {
        let year = date.getFullYear();
				let month = date.getMonth()+1;		
				let day = date.getDate();			
				if(month < 10){
			 			month = "0" + month;
        }
        return `${year}-${month}-${day}`
    },
    // 根据年月确定当前月第一天是周几,一共有多少天
    setWeek(date) {
      date = new Date(date)
      let year = date.getFullYear();
			let month = date.getMonth()+1;
      let weeknum = new Date(`${year}-${month}`).getDay();
      let weekAll = new Date(year, month, 0).getDate()  
      console.log(weeknum, weekAll)
      let arr = []
      for (let i = 0; i < weeknum; i ++) {
        arr.push({
          day: '无',
          date: ''
        })
      }
      for (let j = 1; j < weekAll + 1 ; j ++) {
        arr.push({
          day: j + '',
          date: `${year}-${month}-${j}`
        })
      }
      console.log(arr)
      this.calendar = arr
    },
    // 点击今天
    todayClick() {
      this.date = this.setDate(new Date())
      console.log(this.date)
      this.today = this.date
      this.setWeek(this.date)
    },
    warningClick(index) {
      this.current = index
    },
    dayClick(date) {
      if (date != '') {
        this.date = date
      }
    },
    touchStart(e) {
      console.log(1,e)
    },
    touchMove(e) {
      // console.log(2,e)
    },
    touchEnd(e) {
      console.log(3,e)
    }
  },
  created() {
    // 获取当天时间
    this.todayClick()
  }
}
</script>
<style lang="stylus">
.calendar {
  width 18rem
  position relative
  .nav {
    width 100%
    height 2rem
    background rgb(51,51,51)
    color white
    font-size 0.9rem
    overflow hidden
    line-height 2rem
    .date{
      width 15rem
      float left
      text-align center
    }
    .today{
      float right
      width 2rem
      margin-right 1rem
    }
  }
  .content {
    width 100%
    .weeks {  
      height 1.5rem
      display flex
      color rgb(198,198,198)
      font-size 0.6rem
      font-weight bold
      margin-top 0.1rem 
      .week{
        flex 1
        text-align center
      }
    }
    .days{
      width 100%
      .day{
        width 2.57 rem
        height 1.5rem
        font-size 0.8rem
        display inline-block
        text-align center
        line-height 1.5rem
        .today {
          display inline-block
          width 1.5rem
          height 1.5rem
          border 0px 
          border-radius 0.75 rem
          background rgb(66,107,166)
        }
      }
    }
    .plan{
        width 100%
        position absolute
        background  white
        top 5.1rem
        z-index 10
        .arrows{
          width 100%
          height 2rem
          border-bottom sloid 
          line-height 2rem
          color rgb(66,107,166)
          text-align center
          span {
            font-size 0.8rem
            font-weight bold
            display inline-block
          }
          .arrowsBot {
            transform rotate(90deg)
          }
          .arrowsTop {
            transform rotate(270deg)
          }
        }
        .warning{
          width  17rem
          height 2rem
          line-height 2rem
          margin 0 0.5rem
          display flex
          font-size 0.8rem
          color rgb(108,108,108)
          border-top 1px solid #c6c6c6
          box-shadow: 0px 3px  #c6c6c6;
          div{
            flex 1
            text-align center
          }
          .clicked {
            font-weight bold
            color rgb(42,87,154)
          }
        }
        .warningContents{
          margin 0 1rem
          .warningContent{
            margin 0.2rem 0.5rem
            padding-bottom 0.2rem
            border-bottom 1px solid #c6c6c6
            .warningTitle{
              overflow hidden
              .left{
                font-weight bold
                font-size 1 rem
                float left
              }
              .right{
                font-size 0.6 rem
                color rgb(198,198,198)
                float right 
                
              }
            }
            .warningDesc{
              font-size 0.7 rem
              color rgb(198,198,198)
            }
            .warningRole {
              font-size 0.7 rem
              overflow hidden
              .left{
                float left 
              }
              .right {
                font-size 1 rem
                font-weight bold
                color rgb(198,198,198)
                float right
              }
            }
          }
        }
        .warningOther{
          height 10rem
          margin 0 1rem
          font-size 1 rem
        }
      }
  }
}
</style>
