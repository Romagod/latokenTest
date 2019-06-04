<template>
  <aside class="sidebar">
    <slot></slot>
  </aside>
</template>

<script>
import NavItem from './NavItem'
import Nav from './Nav'

export default {
  name: 'Sidebar',
  components: {
    Nav, NavItem
  }
}
</script>

<style scoped>
.sidebar {
  max-width: 280px;
  width: 25%;
}
</style>
