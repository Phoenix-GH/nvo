<template>
  <div id="app">
    <SidebarNav :active="sidebarNavActive"
      :context="context"
      v-on:toggleSidebar="toggleActive"
    >
      <AppHeader :context="context"
        v-on:toggleSidebar="toggleActive"
      />
      <div id="content" v-on:click="hideMenu">
        <div class="inner">
          <router-view :context="context" />
        </div>
      </div>
      <AppFooter :context="context" v-on:click="hideMenu"/>
    </SidebarNav>
  </div>
</template>

<script>
/* globals axios */
import AppHeader from '@/components/Header'
import AppFooter from '@/components/Footer'
import SidebarNav from '@/components/SidebarNav'
import { bus } from './main'
export default {
  name: 'app',
  components: {
    AppHeader,
    AppFooter,
    SidebarNav
  },
  data () {
    return {
      context: {},
      sidebarNavActive: false
    }
  },
  mounted () {
    this.getContext()
  },
  methods: {
    getContext () {
      const url = '/context/home/'
      axios.get(url).then((response) => {
        this.context = response.data.context
      })
    },
    toggleActive () {
      this.sidebarNavActive = !this.sidebarNavActive
    },
    hideMenu () {
      bus.$emit('hideMenu', 1)
    }
  }
}
</script>

<style>
#app {
  /*font-family: 'Avenir', Helvetica, Arial, sans-serif;*/
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  background: black;
  /* Disable because scrollto is not working for now
  overflow: auto;
  */
  height: 100%;
}
#content {
  height: 100%;
  display: table-row;
  background: black;
}

.inner {
  padding-top: 150px;
  height: 100%;
}
</style>
