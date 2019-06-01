<template>
  <nav>
    <ul>
      <slot></slot>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'Nav'
}
</script>

<style scoped>
ul {
  padding: 0;

  list-style: none;
}
</style>
