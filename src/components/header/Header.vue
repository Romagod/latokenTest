<template>
<header class="main-header">
  <div class="main-header__content">

    <div class="main-header__logo-wrapper">

      <a class="main-header__link" href="/">
        <img class="main-header__logo" src="../../assets/logo.svg" alt="logo">
      </a>

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
  }
}
</script>

<style scoped>
.main-header {
  width: 100%;

  background-color: var(--main-color);
}

.main-header__content {
  display: flex;
  align-items: center;

  height: 42px;
  width: 1366px;

  margin: 0 auto;
}

.main-header__logo-wrapper {
  display: flex;
  align-items: center;

  width: 100%;
}

.main-header__link {
  font-size: 0;
}

.main-header__logo {
  margin-right: 16px;
}

.main-header__caption {
  width: 30%;
  font-size: 14px;
  color: var(--inactive-color-015)
}
</style>
