<template>
  <div class="tooltip"
    v-if="isVisible ? isVisible : false">
    <div class="tooltip__content">
      {{ message }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tooltip',
  props: [
    'message',
    'isVisible'
  ]
}
</script>

<style scoped>
.tooltip {
  box-sizing: border-box;

  position: absolute;
  right: calc(-20px - 230px);
  top: 15px;

  display: flex;
  align-items: center;

  width: 230px;
  height: 48px;

  padding: 0 21px;

  background-color: var(--warning-color);

  color: var(--light-color);
  font-size: 12px;
  word-wrap: break-word;
  line-height: 144%;

  clip-path: polygon(0% 50%, 10px 0%, 100% 0%, 100% 100%, 10px 100%);
}

.tooltip__content {

}
</style>
