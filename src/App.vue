<template>
  <div id="app">
    <Header
      :options="{
         logoUrl: '../../assets/logo.svg',
         caption: 'IEO Launchpad'
       }"/>
    <div class="content-wrapper">
      <Sidebar>
        <Nav></Nav>
      </Sidebar>
      <Main></Main>
      <Sidebar></Sidebar>
    </div>

  </div>
</template>

<script>

import Header from './components/header/Header'
import Sidebar from './components/nav/Sidebar'
import Main from './components/main/Main'
import Nav from './components/nav/Nav'

export default {
  name: 'App',
  components: {
    Nav,
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

  font-family: var(--font);
  font-size: 14px;

  --font:'Fira Sans', sans-serif;

  --main-color: rgb(64, 73, 96);
  --inactive-color-08: #666d80;
  --inactive-color-06: #8c92a0;
  --inactive-color-04: #b3b6bf;
  --inactive-color-03: #c6c8cf;
  --inactive-color-02: #d9dbdf;
  --inactive-color-015: #e2e4e7;
  --inactive-color-01: #e2e4e7;
  --light-color: #ffffff;
  --link-color: #5495aa;
  --success-color: #5ec16f;
  --success-color-07: #8ed49a3d;
  --warning-color: #d56161;
  --warning-color-07: #e29090;
}

.content-wrapper {
  display: flex;

  max-width: 1366px;
  width: 100%;

  margin: 40px auto;
}

html, body {
  margin: 0;

}
</style>
