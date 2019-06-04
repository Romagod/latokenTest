<template>
  <div class="user-menu">
    <div class="user-menu__user-info">Bruce Waine</div>
    <div class="user-menu__user-auth"
    @click="logout"></div>
  </div>
</template>

<script>
export default {
  name: 'UserMenu',
  methods: {
    logout: function () {
      fetch('/logout', {
        method: 'post'
      }).then(data => {
        if (data.status !== '200') {
          throw new Error('Not 200 response')
        } else {
          // do something
        }
      }).catch(() => {
        // error handler
      })
    }
  }
}
</script>

<style scoped>
.user-menu {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.user-menu__user-info {
  display: flex;
  justify-content: flex-end;

  width: 200px;

  margin-right: 8px;

  color: var(--light-color)
}

.user-menu__user-auth {
  width: 32px;
  height: 42px;

  background: url("../../assets/logout.svg") no-repeat 50% 50%;

  cursor: pointer;
}
</style>
