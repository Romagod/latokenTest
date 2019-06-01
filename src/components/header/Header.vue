<template>
<header class="main-header">
  <div class="main-header__content">

    <div class="main-header__logo-wrapper">

      <img class="main-header__logo" src="../../assets/logo.svg" alt="logo">
      <h1 class="main-header__caption">
        {{ options.caption }}
      </h1>

    </div>

    <UserMenu/>
  </div>
</header>
</template>

<script>
import UserMenu from './UserMenu'

export default {
  name: 'Header',
  components: {UserMenu},
  props: [
    'options'
  ],
  mounted () {
    console.log(this.options.logoUrl)
  }
}
</script>

<style scoped>
.main-header {
  display: flex;

  height: 42px;

  background-color: var(--main-color);
}

.main-header__content {
  margin: 0 0 0 10%;
}

.main-header__logo-wrapper {
  display: flex;
  align-items: center;

  width: 100%;
}

.main-header__logo {
  margin-left: 7%;
}

.main-header__caption {
  width: 30%;
  font-size: 14px;
  color: var(--inactive-color-015)
}
</style>
