<script>
export default {
  data() {
    return {
      hours: 0,
      minute: 0,
      name: '',
    };
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
        } else {
          this.hours = 0;
          this.minute = 0;
          this.name = '';
        }
      },
    },
  },
  methods: {
    close: function () {
      this.$emit('close');
    },
    save: function () {
      if (this.alarm.index !== undefined) {
        this.$emit(
          'edit',
          this.alarm.index,
          this.hours,
          this.minute,
          this.name
        );
      } else {
        this.$emit('add', this.hours, this.minute, this.name);
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
        <input type="number" v-model="hours" dir="rtl" min="0" max="23" />
        <span>:</span>
        <input type="number" v-model="minute" min="0" max="59" />
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

    .time-picker {
      display: flex;
      justify-content: center;

      input[type='number'] {
        background: none;
        border: none;
        outline: none;
        font-size: 5rem;
        padding: 0;
        margin: 0;
        width: 6.5rem;
        -moz-appearance: textfield;
        color: rgb(57, 92, 105);
        caret-color: transparent;

        &:nth-child(1) {
          text-align: right;
        }
      }

      span {
        font-size: 5rem;
        margin: 0 0.5rem;
      }
    }

    input[type='text'] {
      width: 40%;
      font-family: 'Roboto';
      outline: none;
      border: none;
      background: none;
      border-bottom: 2px solid gray;
      margin: 1rem auto;
      font-size: 1.2rem;
      color: rgb(57, 92, 105);
      padding: 0.3rem 0;

      &:not(:placeholder-shown) {
        border-bottom: 2px solid rgb(101, 99, 253);
      }
    }

    .buttons {
      margin-top: 1.25rem;
      width: 40%;
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
