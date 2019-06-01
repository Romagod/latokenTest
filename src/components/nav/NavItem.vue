<template>
  <li class="nav-item"
       :class="{
        'nav-item--primary': isPrimary,
       }">

    <a class="nav-item__link"
       :class="{'nav-item--active': isActive}"
       :href="href ? href : '#'"
       @click.prevent="toggle">
      {{ caption }}
    </a>

    <ul class="nav-item__list"
      v-if="isActive && hasChildren">
      <slot></slot>
    </ul>

  </li>

</template>

<script>
export default {
  name: 'NavItem',
  props: [
    'caption',
    'href'
  ],
  data () {
    return {
      isActive: false
    }
  },
  computed: {
    isPrimary: function () {
      return this.$parent.$options.name !== this.$options.name // !!!check name
    },
    hasChildren: function () {
      return !!this.$slots.default
    }
  },
  methods: {
    toggle: function () {
      this.$parent.$children.forEach(item => {
        item.isActive = false
      })

      this.isActive = true
    }
  }
}

</script>

<style scoped>
.nav-item {
  min-height: 32px;
  font-size: 12px;
}

.nav-item__list {
  margin: 1em 0 0 1em;
  padding: 0;

  list-style: none;
}

.nav-item__link {
  display: flex;
  align-items: center;

  color: var(--inactive-color-04);
  text-decoration: none;
}

.nav-item--active {
  color: var(--main-color);
}

.nav-item--primary {
  font-size: 14px;
}

</style>
