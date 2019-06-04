<template>
  <main class="main">
    <PageHeader/>
    <Form action="/" id="form"/>
  </main>
</template>

<script>
import Form from './Form'
import PageHeader from './PageHeader'

export default {
  name: 'Main',
  components: {
    PageHeader,
    Form
  }
}
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;

  align-items: center;

  max-width: 580%;
  width: 50%;
}
</style>
