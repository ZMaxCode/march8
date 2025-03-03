<template>
  <div
      class="dialog"
      :class="{
        'dialog--visible': isVisible
      }"
  >
    <div class="dialog__back"/>
    <div
        class="dialog__main"
        :class="{
          'dialog__main--success': isCodeCorrect
        }"
    >
      <template v-if="status === 'process'">
        <button class="dialog__close" @click="$emit('update:isVisible', false)">
          <CrossIcon class="dialog__close-icon"/>
        </button>
        <h3 class="dialog__title">
          Введите код
        </h3>
        <input
            ref="codeInput"
            class="dialog__code-input"
            v-maska:unmaskedValue.unmasked="'##### ##### ##### ##### #####'"
            v-model="maskedValue"
        />
        <Button
          v-if="!isCodeCorrect"
          :disabled="unmaskedValue.length !== 25"
          class="dialog__code-button"
          @click="handleEnterCode"
        >
          Ввести код
        </Button>
        <span
            v-else
            class="dialog__success-message"
        >
          Устройство обезврежено!
        </span>
      </template>
      <template v-if="status === 'completed'">
        <h3 class="dialog__title">
          Mission completed!
        </h3>
      </template>
      <template v-if="status === 'failed'">
        <h3 class="dialog__title">
          Mission failed!
        </h3>
      </template>
    </div>
  </div>
</template>

<script>
import CrossIcon from '/src/assets/icons/cross.svg';
import Button from "./Button.vue";
import { vMaska } from "maska/vue"

export default {
  name: "CodeDialog",
  emits: ['update:isVisible', 'update:status'],
  components: {CrossIcon, Button},
  directives: {
    maska: vMaska
  },
  props: {
    isVisible: Boolean,
    code: String,
    status: {
      type: String,
      validator: (val) => {
        return ['process', 'completed', 'failed'].includes(val)
      }
    }
  },
  data() {
    return {
      unmaskedValue: "",
      maskedValue: "",
      isCodeCorrect: false,
    }
  },
  methods: {
    closeModal() {
      this.isModalVisible = false;
    },
    handleEnterCode() {
      if(this.unmaskedValue !== this.code) this.handleCodeError()
      else this.handleCodeSuccess();
    },
    handleCodeSuccess() {
      this.isCodeCorrect = true;
      setTimeout(() => {
        this.$emit('update:status', 'completed');
      }, 3000);
    },
    handleCodeError() {
      console.log('error')
    }
  },
  mounted() {
    this.$refs.codeInput.focus();
  }
}
</script>

<style lang="scss">
  .dialog {
    @include transition(visibility, opacity);

    position: fixed;
    visibility: hidden;
    opacity: 0;
    width: 100%;
    height: 100vh;
    z-index: 2;

    &--visible {
      opacity: 1;
      visibility: visible;
    }

    &__back {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: var(--color-modal);
      opacity: 0.6;
    }

    &__main {
      --text-color: var(--color-bright-red);
      --text-shadow: var(--shadow-text-red);

      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: var(--color-modal);
      padding: 64px;
      z-index: 2;
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 80%;
      gap: 48px;

      &--success {
        --text-color: var(--color-bright-green);
        --text-shadow: var(--shadow-text-green);
      }
    }

    &__title {
      @include transition(color);

      font-size: 96px;
      text-align: center;
      color: var(--text-color);
      text-shadow: var(--shadow-text-red);
    }

    &__close {
      position: absolute;
      padding: 30px;
      top: 0;
      right: 0;

      &-icon {
        width: 24px;
        height: 24px;
        fill: white;
      }
    }

    &__code-input {
      @include transition(color, text-shadow);

      font-size: 56px;
      text-align: center;
      width: 100%;
      color: var(--text-color);
      text-shadow: var(--text-shadow);
    }

    &__success-message {
      font-size: 48px;
      color: var(--text-color);
      text-shadow: var(--shadow-text-red);
    }
  }
</style>