<template>
  <div
      class="timer text-red-shadow"
      :class="{ 'timer--blinking': blink }"
  >
    <span>{{ hours }}</span>
    <span class="timer__separate">:</span>
    <span>{{ minutes }}</span>
    <span class="timer__separate">:</span>
    <span>{{ seconds }}</span>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    targetDate: {
      type: Date,
      required: true
    },
    isActive: {
      type: Boolean,
      default: true
    },
  },
  data() {
    return {
      currentTime: new Date(),
      intervalId: null,
      blink: false,
      isTimerFinished: false
    };
  },
  computed: {
    timeRemaining() {
      return Math.max(0, this.targetDate - this.currentTime);
    },
    hours() {
      return Math.floor(this.timeRemaining / 3600000).toString().padStart(2, '0');
    },
    minutes() {
      return Math.floor((this.timeRemaining % 3600000) / 60000).toString().padStart(2, '0');
    },
    seconds() {
      return Math.floor((this.timeRemaining % 60000) / 1000).toString().padStart(2, '0');
    }
  },
  watch: {
    timeRemaining(newVal) {
      if (newVal === 0 && !this.isTimerFinished) {
        this.finishTimer();
      }
    },
    isActive(newVal) {
      if(newVal) return;
      clearInterval(this.intervalId);
    }
  },
  methods: {
    finishTimer() {
      this.isTimerFinished = true;
      clearInterval(this.intervalId);
      this.$emit('finished');
    }
  },
  mounted() {
    this.intervalId = setInterval(() => {
      this.currentTime = new Date();
      this.blink = !this.blink; // Мигание двоеточия
    }, 1000);
  },
  beforeUnmount() {
    if (this.intervalId) {
      clearInterval(this.intervalId); // Очистка интервала при уничтожении компонента
    }
  }
}
</script>

<style lang="scss">
  .timer {
    --separate-opacity: 1;

    font-size: 56px;
    width: max-content;

    &--blinking {
      --separate-opacity: 0.5;
    }

    &__separate {
      @include transition(opacity);

      display: inline-block;
      opacity: var(--separate-opacity);
      transform: translateY(-4px);
    }

    @media screen and (max-width: 768px) {
      font-size: 48px;
    }
  }
</style>