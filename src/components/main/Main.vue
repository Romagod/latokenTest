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
  flex: 0.4 0 auto;
}
</style>
