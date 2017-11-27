<template>
  <div id="header" v-if="context.header" v-on:mouseleave="clearMenu" v-on:mouseenter="cancelHideMenu"> 
    <div class="container">
      <div class="top">
        <i class="glyphicon glyphicon-menu-hamburger pull-left"
          v-on:click="enableSidebar"
          ></i>
        <router-link to="/">
          <img class="logo pull-left" src="../assets/logo.png">
        </router-link>
        
        <div class="social pull-right">
          <img src="../assets/icons/facebook.png">
          <img src="../assets/icons/twitter.png">
          <img src="../assets/icons/profile.png">
        </div>
        <div class="drop-down-menus pull-right">
          <ul v-for="(menus, index) in context.header.selectors">
            <li v-on:click="setActiveMenu(index)">{{ menus.title }} <i class="glyphicon glyphicon-menu-down"></i></li>
          </ul>
        </div>

        <div class="search">
          <input type="text" placeholder="Search" v-model="searchValue" v-on:keyup.enter="submitSearch">
        </div>
      </div>
    </div>
    <transition name="slide-fade" mode="out-in">
      <div class="bottom" :class="{active: displayMenu }" v-if="displayMenu">
        <div class="list-container" :class="{ 'container-wide': activeMenu === 0 }">
            <ul class="shows" v-if="activeMenu === 1">
              <li v-for="link in context.header.selectors[activeMenu].links">
                <router-link :to="link.page">{{ link.title }}</router-link>
              </li>
            </ul>
            <ul v-else>
              <li v-for="link in context.header.selectors[activeMenu].links" :style="link.title === channelHover.title ? channelHoverUpdate : null" v-on:mouseenter="handleChannelHover(link)">
                <router-link :to="link.page">
                  <img :src="link.icon" />
                </router-link>
              </li>
            </ul>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>

import { bus } from '@/main'
export default {
  name: 'Header',
  props: [
    'context'
  ],
  data () {
    return {
      activeMenu: 0,
      displayMenu: false,
      hideMenuPending: false,
      hideMenuTimeout: null,
      hideMenuDelayDuration: 3000,
      channelHover: {},
      searchValue: null
    }
  },
  mounted () {
    bus.$on('hideMenu', this.clearMenu)
  },
  methods: {
    clearMenu () {
      this.displayMenu = false
    },
    setActiveMenu (id) {
      // console.log('setting', id, 'active')
      this.activeMenu = id
      this.displayMenu = true

      // Disable hiding menu delay for now
      // let _this = this
      // setTimeout(function () {
      //   _this.displayMenu = false
      // }, 15000)
    },
    hideMenu () {
      this.channelHover = {}
      let self = this
      this.hideMenuPending = true
      this.hideMenuTimeout = setTimeout(function () {
        self.displayMenu = false
        self.hideMenuPending = false
      }, self.hideMenuDelayDuration)
    },
    cancelHideMenu () {
      if (this.hideMenuPending) {
        clearTimeout(this.hideMenuTimeout)
      }
    },
    handleChannelHover (link) {
      this.channelHover = link
    },
    enableSidebar () {
      this.$emit('toggleSidebar')
    },
    submitSearch () {
      this.$router.push({
        path: '/search',
        query: {
          q: this.searchValue
        }
      })
      this.searchValue = null
    }
  },
  computed: {
    channelHoverUpdate () {
      return {
        backgroundColor: this.channelHover.hover_clr
      }
    }
  }
}
</script>
<style scoped>
.container {
  width: 95%;
  margin: 25px auto;
  max-width: 1024px;
}
#header {
  background: url('../assets/bg2.jpg') no-repeat;
  background-size: cover;
  position: fixed;
  width: 100%;
  z-index: 999;
  display: table-row;
  box-shadow: 0px 2px 10px rgba(0,0,0,0.55);
}

.logo {
  margin-right: 58px;
}

.glyphicon-menu-hamburger {
  font-size: 20px;
  position: relative;
  top: 3px;
  margin-right: 20px;
  margin-left: -5px;
}

.top {
  
}

.bottom {
  clear: both;
  background: rgba(0,0,0,0.5);
  display: none;
}

.bottom.active {
  display: block;
}

.bottom .container-wide {
  width: 100%;
  position: relative;

}

.bottom p {
  margin: 0;
  padding: 0;
}

.bottom ul {
  margin: 0;
  padding: 0;
}

.bottom li {
  list-style-type: none;
  display: inline-block;
  margin: 0;
  height: 66px;
  width: 25%;
  text-align: center;
}

.bottom li a {
  display: inline-block;
  padding: 20px 25px;
  margin: 0;
  font-size: 18px;
}
@media screen and (max-width:568px) {
  .bottom li {
    width: 50%;
  }
  .bottom li a {
    padding:10px 10px;
  }
}
/*

.bottom li:hover {
  background: #f22061;
}

li.channel-pro-tv:hover {
  background: #2556fd;
}

li.channel-pro-2:hover {
  background: #f12160;
}

li.channel-pro-x:hover {
  background: #000000;
}

li.channel-pro-gold:hover {
  background: #cba635;
}

li.channel-pro-cinema:hover {
  background: #7d287d;
}
*/

.bottom .shows {

}

.bottom .shows li {
  height: 40px;
  padding: 2px 0;
}

.bottom .shows li a {
  display: inline-block;
  width: 25%;
  min-width: 150px;
  padding: 5px 15px;
  font-weight: 300;
}

.bottom .shows li:hover {
  background: none;
}

.bottom li a:hover {
  text-decoration: underline;
  font-weight: bold;
  /* border-bottom: 2px solid white; */
}

.search {
  float: left;
  width: 350px;
}

.search input {
  width: 100%;
  height: 28px;
  padding-left: 40px;
  background-image: url('../assets/icons/search.png');
  background-repeat: no-repeat;
  background-position: 15px center;
  color: black;
  border: 0;
}
@media screen and (max-width: 960px) {
  .search {
    margin: 0 auto;
    width: 100%;
  }
  .logo {
    margin-right: 10px;
  }
}

@media screen and (max-width: 483px) {
  .drop-down-menus {
    display: none;
  }
  .search {
    margin-top: 10px;

  }

}
@media screen and (max-width:307px) {
  .search {
    padding: 10px 0 10px;
  }
  .glyphicon-menu-hamburger {
    margin-right: 20px;
  }
  .bottom li {
    width: 100%;
  }

}
.search input::placeholder {
  font-style: italic;
}

.main-menu {
  background: #092061;
  text-transform: uppercase;
  font-weight: 800;
  font-size: 20px;
}
.main-menu li {
  display: inline-block;
}

.list-container {
  max-width: 1024px;
  margin: 0 auto;
}
.drop-down-menus {
  position: relative;
  
}

.drop-down-menus ul {
  display: inline-block;
  margin: 0;
  padding: 0;
}

.drop-down-menus ul > li {
  list-style-type: none;
  margin: 0;
  padding: 10px 10px 10px;
}

.drop-down-menus ul > li i {
  font-size: 8px;
}

.drop-down-menus ul ul {
  display: none;
}

.drop-down-menus ul > li {
  font-size: 15px;
}

.drop-down-menus ul > li:hover {
  background: rgba(0,0,0,0.5);
  cursor: pointer;
}

.drop-down-menus ul:hover ul {
  display: inline-block;
}

a {
  color: white;
}

.social img {
  margin: 0px 7.5px;
}

.top i:hover { cursor: pointer; }

/* A N I M A T I O N S */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .4s cubic-bezier(0.23, 1, 0.32, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateY(10px);
  opacity: 0;
}

</style>
