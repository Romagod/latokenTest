<template>
  <nav>
    <ul>
      <NavItem v-for="(item, index) in menu"
        :options="item"
        :key="index"
        :id="index">
      </NavItem>
    </ul>
  </nav>
</template>

<script>
/* eslint-disable */
import NavItem from './NavItem'
export default {
  name: 'Nav',
  components: {NavItem},
  data () {
    return {
      menu: [{
        title: 'Project info', href: '/', isActive: true, children: [
          {title: 'General', href: '/', isActive: true},
          {title: 'Fundraising', href: '/', isActive: false},
          {title: 'Previous rounds', href: '/', isActive: false},
          {title: 'Roadmap', href: '/', isActive: false},
          {title: 'Partnerships', href: '/', isActive: false}]
        },
        {title: 'Team', href: '/', isActive: false},
        {title: 'Product', href: '/', isActive: false},
        {title: 'Financial', href: '/', isActive: false},
        {title: 'Risks', href: '/', isActive: false},
        {title: 'Data', href: '/', isActive: false},
        {title: 'Reviews', href: '/', isActive: false},
        {title: 'FAQ', href: '/', isActive: false},
        {title: 'Mentions', href: '/', isActive: false}]
    }
  }
}
</script>

<style scoped>
ul {
  padding: 0;
  margin: 0;

  list-style: none;
}
</style>
