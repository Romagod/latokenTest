<template>
  <div id="app">
    <Header
      :options="{
         logoUrl: '../../assets/logo.svg',
         caption: 'IEO Launchpad'
       }"/>
    <div class="content-wrapper">
      <Sidebar></Sidebar>
      <Main></Main>
    </div>

  </div>
</template>

<script>

import Header from './components/header/Header'
import Sidebar from './components/nav/Sidebar'
import Main from './components/main/Main'

export default {
  name: 'App',
  components: {
    Main,
    Sidebar,
    Header
  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: column;

  @import url('https://fonts.googleapis.com/css?family=Fira+Sans:400,700&display=swap');

  font-family: 'Fira Sans', sans-serif;
  font-size: 14px;

  --main-color: rgb(64, 73, 96); /* !!! change*/
  --inactive-color-08: #666d80;
  --inactive-color-06: #8c92a0;
  --inactive-color-04: #b3b6bf;
  --inactive-color-03: #c6c8cf;
  --inactive-color-02: #d9dbdf;
  --inactive-color-015: #e2e4e7;
  --inactive-color-01: #e2e4e7;
  --light-color: #ffffff;
  --link-color: #5495aa;
  --warning-color: #d56161;
}

.content-wrapper {
  display: flex;

  margin: 40px 0 0 0;
}

html, body {
  margin: 0;
}
</style>
