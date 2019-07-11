<template>
  <div
    class="day"
    :class="[
      isToday ? 'is-today' : '',
      obj.thisMonth ? '' : 'not-thisMonth',
      isActive ? 'is-active' : ''
    ]"
    :style="{
      background: color ? color : '#fff'
    }"
    @click="clickTime"
  >
    <!-- <a class="date" @click.prevent.stop="update"
      >{{ obj.month }}/{{ obj.date }}</a
    > -->
    {{ obj.background_color }}
    <a class="date">{{ obj.month }}/{{ obj.date }}</a>
    <div
      class="sensor"
      @drop.prevent="dropDone"
      @dragenter.prevent="dropEnter"
      @dragover.prevent
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    obj: {
      require: true
    },
    today: {
      require: true
    },
    activeNode: {
      require: true
    },
    dateColor: {
      require: true
    }
  },
  computed: {
    isToday: function() {
      return (
        this.obj.year === this.today.year &&
        this.obj.month === this.today.month &&
        this.obj.date === this.today.date
      );
    },
    isActive: function() {
      let $obj = this.obj;

      if (this.activeNode[$obj.year + "-" + $obj.month + "-" + $obj.date]) {
        return true;
      } else {
        return false;
      }
    },
    color: function() {
      for (let data of this.dateColor) {
        if (
          data.year == this.obj.year &&
          data.month == this.obj.month &&
          data.date == this.obj.date
        ) {
          return data.color;
        }
      }
      return void 0;
    }
  },
  methods: {
    // update: function() {
    //   //推送日期的更新
    //   this.$emit("sendEvent", "updateCal", "main", {
    //     year: this.obj.year,
    //     month: this.obj.month,
    //     date: this.obj.date
    //   });

    //   this.$emit("sendEvent", "updateMode", "date");
    // },
    clickTime: function(evt) {
      this.$emit(
        "sendEvent",
        "clickTime",
        {
          year: this.obj.year,
          month: this.obj.month,
          date: this.obj.date,
          hour: 0,
          minutes: 0
        },
        "date"
      );
    },
    dropDone: function() {
      this.$emit("sendEvent", "dropDone", this.obj);
    },
    dropEnter: function() {
      this.$emit("sendEvent", "dropEnter", this.obj);
    }
  }
};
</script>

<style scoped>
</style>
