<template>
  <button class="button" v-bind="$attrs">
    <span class="button__label">
      <slot />
    </span>
  </button>
</template>

<script>
export default {
  name: "Button"
}
</script>

<style lang="scss">
  .button {
    @include transition(background);

    font-size: 16px;
    font-weight: 400;
    padding: 24px 0;
    width: 254px;
    text-align: center;
    background: var(--color-white);
    text-transform: uppercase;
    display: inline-block;

    &:disabled {
      background: var(--color-disabled);

      .button__label {
        opacity: 0.25;
      }
    }

    &__label {
      @include transition(opacity);
    }
  }
</style>