<script>
export default {
  data() {
    return {
      hours: '00',
      minute: '00',
      name: '',
      days: [
        {
          label: 'Sun',
          isEnabled: true,
        },
        {
          label: 'Mon',
          isEnabled: true,
        },
        {
          label: 'Tue',
          isEnabled: true,
        },
        {
          label: 'Wed',
          isEnabled: true,
        },
        {
          label: 'Thu',
          isEnabled: true,
        },
        {
          label: 'Fri',
          isEnabled: true,
        },
        {
          label: 'Sat',
          isEnabled: true,
        },
      ],
    };
  },
  computed: {
    alarmDays() {
      let resultString = 'Every ';

      let totalDays = 0;

      this.days.forEach((day, i) => {
        if (day.isEnabled) {
          resultString += day.label;
          if (i !== this.days.length - 1) {
            resultString += ', ';
          }

          totalDays++;
        }
      });

      if (totalDays === 7) {
        resultString = 'Everyday';
      }

      return resultString;
    },
  },
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    alarm: {
      type: Object,
    },
    isEditing: Boolean,
  },
  watch: {
    isOpen: {
      handler() {
        if (this.isEditing) {
          this.hours = this.alarm.hour;
          this.minute = this.alarm.minute;
          this.name = this.alarm.name;
          this.days = this.alarm.days;
        } else {
          this.hours = 0;
          this.minute = 0;
          this.name = '';
        }
      },
    },
    hours: {
      handler() {
        if (isNaN(this.hours || this.hours == '')) this.hours = '00';

        if (this.hours > 23) this.hours = 0;
        else if (this.hours < 0) this.hours = 23;

        if (this.hours.toString().length === 1) {
          this.hours = '0' + this.hours;
        }
      },
    },
    minute: {
      handler() {
        if (this.minute > 59) {
          this.minute = 0;
        } else if (this.minute < 0) {
          this.minute = 59;
        }

        if (this.minute.toString().length === 1) {
          this.minute = '0' + this.minute;
        }
      },
    },
  },
  methods: {
    close: function () {
      this.$emit('close');
    },
    save: function () {
      if (this.isEditing) {
        this.$emit(
          'edit',
          this.alarm.index,
          this.hours,
          this.minute,
          this.name,
          this.days
        );
      } else {
        this.$emit('add', this.hours, this.minute, this.name, this.days);
      }
      this.close();
    },
    deleteAlarm: function () {
      this.$emit('delete', this.alarm.index);
      this.close();
    },
  },
};
</script>

<template>
  <div class="modal" :class="{ open: isOpen }" @click.self="close">
    <div class="container">
      <div class="time-picker">
        <input type="number" v-model="hours" dir="rtl" />
        <span>:</span>
        <input type="number" v-model="minute" />
      </div>
      <div class="day-picker">
        <p class="desc">
          {{ alarmDays }}
        </p>
        <div class="days">
          <div
            v-for="(day, index) in days"
            :key="index"
            class="day"
            :class="{ 'day-active': day.isEnabled }"
            @click="day.isEnabled = !day.isEnabled"
          >
            {{ day.label[0] }}
          </div>
        </div>
      </div>
      <input type="text" v-model="name" name="alarm name" placeholder="Name" />
      <div class="buttons">
        <button
          :class="{ hidden: !isEditing }"
          class="delete"
          @click="deleteAlarm"
        >
          Delete
        </button>
        <button @click="save">Save</button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.open {
  display: flex !important;
}

.hidden {
  visibility: hidden;
}

.modal {
  display: none;
  background: rgba(0, 0, 0, 0.3);
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0;
  z-index: 9;
  color: rgb(57, 92, 105);

  .container {
    background: rgb(248, 250, 251);
    justify-self: center;
    align-self: center;
    margin: auto;
    padding: 2rem 2.5rem;
    min-width: 600px;
    min-height: 400px;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    z-index: 11;
    max-width: 12rem;

    .time-picker {
      display: flex;
      justify-content: center;

      input[type='number'] {
        background: none;
        border: none;
        outline: none;
        font-size: 7rem;
        padding: 0;
        margin: 0;
        width: 30%;
        -moz-appearance: textfield;
        color: rgb(57, 92, 105);
        caret-color: transparent;

        &:nth-child(1) {
          text-align: right;
        }
      }

      span {
        font-size: 5rem;
        margin: 1rem 1.5rem;
      }
    }

    .day-picker {
      width: 65%;
      margin: 1rem auto;
      margin-bottom: 0;

      .desc {
        font-size: 0.8rem;
        margin: 0;
        padding: 0;
      }

      .days {
        display: flex;
        justify-content: space-between;
        align-self: center;
        margin: 0.5rem 0;

        .day {
          padding: 0.5rem;
          border-radius: 50%;
          width: 2.25rem;
          height: 2.25rem;
          display: flex;
          justify-content: center;
          align-items: center;
          font-weight: 600;
          cursor: pointer;
          user-select: none;

          &-active {
            color: rgb(101, 99, 253);
            border: 1.5px solid rgb(101, 99, 253);
          }
        }
      }
    }

    input[type='text'] {
      width: 65%;
      font-family: 'Roboto';
      outline: none;
      border: none;
      background: none;
      border-bottom: 2px solid gray;
      margin: 0.5rem auto;
      font-size: 1.2rem;
      color: rgb(57, 92, 105);
      padding: 0.3rem 0;

      &:not(:placeholder-shown) {
        border-bottom: 2px solid rgb(101, 99, 253);
      }
    }

    .buttons {
      margin-top: 1.25rem;
      width: 65%;
      align-self: center;
      display: flex;
      justify-content: space-between;

      button {
        cursor: pointer;
        outline: none;
        border: none;
        font-size: 1rem;
        padding: 0.5rem 1rem;
        background: rgb(101, 99, 253);
        color: white;
        border-radius: 12px;
        box-shadow: -4px -2px 4px 0px rgba(231, 231, 231, 0.1),
          4px 2px 6px 0px rgba(228, 228, 247, 0.1);

        &:hover {
          filter: brightness(85%);
        }
      }

      .delete {
        background: none;
        border: 1px solid rgb(214, 66, 7);
        color: rgb(214, 66, 7);
      }
    }
  }
}
</style>
