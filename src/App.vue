<template>
  <div class="main">
    <Timer
        class="main__timer"
        :target-date="new Date('2025-03-07T20:00:00')"
        :is-active="isTimerActive"
        @finished="onTimerFinish"
    />
    <CodeDialog
      code="4719265830471583692046752"
      v-model:is-visible="isModalVisible"
      v-model:status="currentStatus"
      @update:status="stopTimer"
    />
    <Button
        class="main__button"
        @click="showModal"
        v-if="hasCodeButton"
    >
      Ввести код
    </Button>
  </div>
</template>

<script>
import Button from "./components/Button.vue";
import Timer from "./components/Timer.vue";
import CodeDialog from "./components/CodeDialog.vue";

export default {
  name: 'App',
  components: {CodeDialog, Timer, Button},
  data() {
    return {
      currentStatus: 'process',
      isModalVisible: false,
      isTimerActive: true,
    }
  },
  computed: {
    hasCodeButton() {
      return window.location.hash === '#code'
    }
  },
  methods: {
    showModal() {
      this.isModalVisible = true;
    },
    stopTimer() {
      this.isTimerActive = false;
    },
    onTimerFinish() {
      this.currentStatus = 'failed';
      this.isModalVisible = true;
    },
  }
}
</script>

<style lang="scss">
  .main {
    position: relative;
    width: 100%;
    height: 100vh;
    background-size: cover;
    background-position: center center;
    background-repeat: no-repeat;
    background-image: url("./assets/img/bomb.jpg");

    &__button {
      position: absolute;
      bottom: 48px;
      left: 50%;
      transform: translateX(-50%);
    }

    &__timer {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  }
</style>
