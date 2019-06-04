<template>
  <div class="button-component">
    <input class="button" type="button"
           :class="{[`button--${buttonStyle ? buttonStyle : 'base'}`]: true}"
           :value="value"
           @click.prevent="click">
  </div>
</template>

<script>
export default {
  name: 'Button',
  props: [
    'value',
    'buttonStyle'
  ],
  methods: {
    click: function (event) {
      this.$emit('click')
    }
  }
}
</script>

<style scoped>
.button {
  width: 100%;
  height: 48px;

  margin: 1em 0; /* todo delete */

  background-color: var(--light-color);

  border: 1px solid var(--main-color);
  border-radius: 4px;

  cursor: pointer;
}

.button--base {
  background-color: var(--light-color);
}

.button--main {
  background-color: var(--main-color);

  font-family: var(--font);
  color: var(--light-color)
}

.button:focus {
  outline: none;
}
</style>
