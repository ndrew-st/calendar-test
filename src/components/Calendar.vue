<template>
  <div class="wrapper">
    <div class="flex">
      <div class="arrow left" @click="changeMonth(-1)"></div>
      {{curMonth}}
      <div class="arrow right" @click="changeMonth(1)"></div>
    </div>

    <table>
      <tr>
        <th v-for="(day, ind) in week" :key="ind" class="sell">{{day}}</th>
      </tr>

      <tr v-for="(week, weekInd) in monthArray" :key="`week${weekInd}`">
        <td
          v-for="(date, dateInd) in week"
          :key="`weekdate${weekInd}${dateInd}`"
          class="sell"
          :class="{selected: +selDate === +date.date}"
          @click="selectDate(date.date)"
        >{{date.title}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    lang: {
      type: String,
      default: "en",
      required: false,
    },
    startDate: {
      type: String,
      default: "",
      required: false,
    },
  },
  data() {
    return {
      enWeek: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      ruWeek: ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"],
      enMonth: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      ruMonth: [
        "Январь",
        "Февраль",
        "Март",
        "Апрель",
        "Май",
        "Июнь",
        "Июль",
        "Август",
        "Сентябрь",
        "Октябрь",
        "Ноябрь",
        "Декабрь",
      ],
      firstDateCurMonth: new Date(),
      selDate: new Date(),
    };
  },
  computed: {
    week() {
      return this.lang === "en" ? this.enWeek : this.ruWeek;
    },
    monthArray() {
      const ma = [[]];

      const fd = this.firstDateCurMonth.getDay();
      let addCount = 0;

      if (this.lang === "en") {
        addCount = fd;
      } else {
        addCount = fd === 0 ? 6 : fd - 1;
      }

      for (let i = 0; i < addCount; i++) {
        ma[0].push({ title: "", date: "" });
      }

      let curDate = new Date(this.firstDateCurMonth);
      let curWeek = 0;
      while (curDate.getMonth() === this.firstDateCurMonth.getMonth()) {
        ma[curWeek].push({ title: curDate.getDate(), date: new Date(curDate) });

        addCount++;
        if (addCount > 6) {
          curWeek++;
          ma.push([]);
          addCount = 0;
        }
        curDate.setDate(curDate.getDate() + 1);
      }

      console.log("monthArray -> ma", ma);
      return ma;
    },
    curMonth() {
      const curArray = this.lang === "en" ? this.enMonth : this.ruMonth;
      return (
        curArray[this.firstDateCurMonth.getMonth()] +
        " " +
        this.firstDateCurMonth.getFullYear()
      );
    },
  },
  created() {
    if (this.startDate) {
      this.firstDateCurMonth = new Date(this.startDate);
      this.selDate = new Date(this.startDate);
    }
    this.firstDateCurMonth.setDate(1);
    this.firstDateCurMonth = new Date(this.firstDateCurMonth);
    console.log("created -> this.firstDateCurMonth", this.firstDateCurMonth);
  },
  methods: {
    changeMonth(par) {
      this.firstDateCurMonth.setMonth(this.firstDateCurMonth.getMonth() + par);
      console.log(
        "changeMonth -> this.firstDateCurMonth",
        this.firstDateCurMonth
      );
      this.firstDateCurMonth = new Date(this.firstDateCurMonth);
    },
    selectDate(par) {
      this.selDate = new Date(par);
      this.$emit("select", par);
    },
  },
};
</script>

<style scoped>
.wrapper {
  border: 1px solid #cccccc;
  display: inline-block;
  padding: 20px;
}

.flex {
  display: flex;
  justify-content: space-between;
}

.arrow {
  width: 20px;
  height: 20px;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  cursor: pointer;
}

.left {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' version='1.1' id='Layer_1' x='0px' y='0px' viewBox='0 0 492 492' style='enable-background:new 0 0 492 492;' xml:space='preserve'%3E%3Cg%3E%3Cg%3E%3Cpath d='M198.608,246.104L382.664,62.04c5.068-5.056,7.856-11.816,7.856-19.024c0-7.212-2.788-13.968-7.856-19.032l-16.128-16.12 C361.476,2.792,354.712,0,347.504,0s-13.964,2.792-19.028,7.864L109.328,227.008c-5.084,5.08-7.868,11.868-7.848,19.084 c-0.02,7.248,2.76,14.028,7.848,19.112l218.944,218.932c5.064,5.072,11.82,7.864,19.032,7.864c7.208,0,13.964-2.792,19.032-7.864 l16.124-16.12c10.492-10.492,10.492-27.572,0-38.06L198.608,246.104z'/%3E%3C/g%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3C/svg%3E");
}

.right {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' version='1.1' id='Capa_1' x='0px' y='0px' width='451.846px' height='451.847px' viewBox='0 0 451.846 451.847' style='enable-background:new 0 0 451.846 451.847;' xml:space='preserve'%3E%3Cg%3E%3Cpath d='M345.441,248.292L151.154,442.573c-12.359,12.365-32.397,12.365-44.75,0c-12.354-12.354-12.354-32.391,0-44.744 L278.318,225.92L106.409,54.017c-12.354-12.359-12.354-32.394,0-44.748c12.354-12.359,32.391-12.359,44.75,0l194.287,194.284 c6.177,6.18,9.262,14.271,9.262,22.366C354.708,234.018,351.617,242.115,345.441,248.292z'/%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3Cg%3E%3C/g%3E%3C/svg%3E");
}

.sell {
  width: 40px;
  height: 40px;
  cursor: pointer;
  border: 1px solid white;
}

.selected {
  border: 1px solid black;
}
</style>