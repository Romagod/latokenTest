<template>
  <aside class="sidebar">
    <Nav>
      <NavItem caption="Project info">
        <NavItem caption="General"/>
        <NavItem caption="Test">
          <NavItem caption="test-1" href="/test1"/>
          <NavItem caption="test-2" href="/test2"/>
          <NavItem caption="test-3" href="/test3"/>
        </NavItem>
      </NavItem>
      <NavItem caption="Team"/>
      <NavItem caption="Product"/>
      <NavItem caption="Financials"/>
    </Nav>
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
  flex: 0.2 0 auto;

  margin: 0 0 0 10%;
}
</style>
