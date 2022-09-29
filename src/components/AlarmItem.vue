<script>
export default {
  props: {
    alarmId: Number,
    alarm: {
      type: Object,
    },
  },
  methods: {
    toggleAlarm(e) {
      this.$emit('toggle', this.alarmId, e.target.checked);
    },
  },
};
</script>

<template>
  <div class="alarm-item">
    <div class="desc">
      <span class="alarm-time"
        >{{ alarm.hour }}:{{ alarm.minute.toString().padStart(2, '0') }}</span
      >
      <span class="alarm-name">{{ alarm.name }}</span>
    </div>
    <input
      type="checkbox"
      name=""
      :id="alarmId"
      @change="toggleAlarm($event)"
      :checked="alarm.isActive"
    />
    <label :for="alarmId"></label>
  </div>
</template>

<style lang="scss" scoped>
.alarm-item {
  cursor: pointer;
  background: white;
  width: 100%;
  background-color: rgb(248, 250, 251);
  box-shadow: -4px -2px 4px 0px #ffffff,
    4px 2px 6px 0px rgba(209, 217, 230, 0.9);
  display: flex;
  align-items: center;
  margin: 1rem 0;
  padding: 1rem 3rem;
  border-radius: 20px;
  /* font-family: "Poppins"; */
  justify-content: space-between;
  font-size: 2.2rem;
  font-weight: 500;
  color: rgb(57, 92, 105);

  .desc {
    align-self: center;

    .alarm-name {
      font-weight: 300;
      font-size: 1.3rem;
      align-self: center;
      margin-left: 1rem;
    }
  }
}

input {
  height: 0;
  width: 0;
  visibility: hidden;
}

label {
  cursor: pointer;
  text-indent: -9999px;
  width: 60px;
  height: 30px;
  display: block;
  background: rgb(235, 235, 235);
  border-radius: 100px;
  position: relative;
  box-shadow: -4px -2px 4px 0px rgba(231, 231, 231, 0.1),
    4px 2px 6px 0px rgba(84, 84, 85, 0.1),
    2px 2px 2px 0px rgba(84, 84, 85, 0.1) inset,
    -2px -2px 2px 0px rgba(231, 231, 231, 0.1) inset;
}

label:after {
  content: '';
  position: absolute;
  top: 0px;
  left: 0;
  width: 30px;
  height: 30px;
  background: #fff;
  border-radius: 50%;
  transition: 0.3s;
  border-radius: 15px;
  box-shadow: -4px -2px 4px 0px rgba(231, 231, 231, 0.1),
    4px 2px 6px 0px rgba(84, 84, 85, 0.1),
    2px 2px 2px 0px rgba(84, 84, 85, 0.2) inset,
    -2px -2px 2px 0px rgba(231, 231, 231, 0.4) inset;
}

input:checked + label {
  background: rgb(101, 99, 253);
}

input:checked + label:after {
  left: calc(100%);
  transform: translateX(-100%);
}

label:active:after {
  width: 30px;
}
</style>
