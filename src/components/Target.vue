<template>
  <div class="target">
    <span class="bustype">{{ this.buslist !== "hello" ? type : "" }}</span>
    <span>
      <span class="time_num">{{ this.buslist !== "hello" ? (hours > 0 ? hours+":": "") : "" }}</span>
      <span class="time_num">{{ this.buslist !== "hello" ? minutes + ":" : "" }}</span>
      <span class="time_num">{{ this.buslist !== "hello" ? (seconds < 10 ? "0" + seconds : seconds) : "" }}</span>
    </span>
    <span class="estimate_test"> 후 도착예정</span>
  </div>
</template>

<script>
export default {
  name: "Target",
  created() {
    setInterval(() => {
      this.getCurrentTime(this);
    }, 1000);
  },

  methods: {
    getCurrentTime: x => {
      x.now = Math.floor(new Date().getTime() / 1000);
    }
  },

  props: ["buslist","a"],

  data() {
    return {
      now: Math.floor(new Date().getTime() / 1000),
      i: 0+this.a
    };
  },

  computed: {
    type() {
      let j = this.buslist.length;
      if (this.buslist[this.i].type == "F") {
        return "운행종료"
      }
      else {
        if (Math.floor(this.buslist[this.i].time - this.now) <= 0) {
          if (this.i < j - 1) {
            this.i++;
          }
        } else {
          switch (this.buslist[this.i].type) {
            case "DH":
              return "한대앞행";
            case "DY":
              return "예술인행";
            case "C":
              return "순환노선"
            case "R":
              return "기숙사행"
            case "NA":
              return "운행안함"
            default:
              return "셔틀콕행";
          }
        }
      }
    },
    hours() {
      let j = this.buslist.length;
      if (this.buslist[this.i].type == "F") {
        return 0;
      }
      else {
        if (Math.floor(this.buslist[this.i].time - this.now) <= 0) {
          if (this.i < j - 1) {
            this.i++;
            return 0;
          } else {
            return 0;
          }
        } else {
          return Math.floor((this.buslist[this.i].time - this.now) / 60 / 60) % 24;
        }
      }
    },
    minutes() {
      let j = this.buslist.length;
      if (this.buslist[this.i].type == "F") {
        return 0;
      }
      else {
        if (Math.floor(this.buslist[this.i].time - this.now) <= 0) {
          if (this.i < j - 1) {
            this.i++;
            return 0;
          } else {
            return 0;
          }
        } else {
          return Math.floor((this.buslist[this.i].time - this.now) / 60) % 60;
        }
      }
    },
    seconds() {
      let j = this.buslist.length;
      if (this.buslist[this.i].type == "F") {
        return 0;
      }
      else { 
        if (Math.floor(this.buslist[this.i].time - this.now) <= 0) {
          if (this.i < j - 1) {
            this.i++;
            return 0;
          } else {
            return 0;
          }
        } else {
          return (this.buslist[this.i].time - this.now) % 60;
        }
      }
    }
  }
};
</script>

<style scoped>
  /*1rem == 14px*/
.target {
  display: table;
}
.bustype {
  color: #007afa;
  font-size: 0.785rem;
  display: table-cell;
  padding: 5px;
}

.time {

  font-size: 1rem;
}

  .time_num {
    font-size: 1.42rem;
    font-weight: bold;
    vertical-align: text-bottom;
  }

  .estimate_test {
    font-size: 1rem;
  }
</style>
