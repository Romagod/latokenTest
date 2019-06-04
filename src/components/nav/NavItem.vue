<template>
  <li class="nav-item"
       v-if="isVisible"
       :class="{
        'nav-item--primary': isPrimary,
       }">

    <a class="nav-item__link"
       :class="{'nav-item--active': isActive}"
       :href="href ? href : '#'"
       @click.prevent="setActiveTab">
      {{ options.title }}
    </a>

    <ul class="nav-item__list"
      v-if="isActive && options.children">
      <NavItem
        v-for="(item, index) in options.children"
        :key="index"
        :id="index"
        :options="item"
        @active="activeHandler">
      </NavItem>
    </ul>

  </li>

</template>

<script>
export default {
  name: 'NavItem',
  props: [
    'options',
    'href',
    'id'
  ],
  data () {
    return {
      isPrimary: false,
      isActive: false,
      isEndPoint: false
    }
  },
  methods: {
    // toggle neighbour active property
    setActiveTab: function () {
      this.isActive = true
      this.$parent.$emit('active', this.id)

      if (this.isEndPoint) {
        // do redirect
      }
    },

    activeHandler: function () {
      this.isVisible = true
    },
    isVisible: function () {
      return this.$parent.isActive || this.isPrimary
    }
  },
  created () {
    this.isPrimary = this.$parent.$options._componentTag === 'Nav'

    this.isEndPoint = !!this.options.children

    this.isActive = this.options.isActive

    this.$parent.$on('active', (id) => {
      if (this.id !== id) {
        this.isActive = false
      }
    })
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
