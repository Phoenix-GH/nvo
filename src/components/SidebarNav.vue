<template>
<div id="sidebar-nav" v-if="context.hasOwnProperty('header')">
  <div id="sidebar" :class="{ active: active }">
    <ul>
      <li>
        <a href='/'><h3 class="title">HOME</h3></a>
      </li>
      <li v-for="section in context.header.side_menu">
        <h3 class="title" @click="(event) => {toggle(event, section.title)}">{{ section.title }}</h3>
        <ul v-if="options[section.title]===true">
          <li v-for="link in section.links">
            <a v-if="isExternalLink(link.link)" :href="link.link">
              <img :src="link.icon" v-if="link.icon"/>
              <p v-else>{{ link.title }}</p>
            </a>
            <router-link :to="link.link" v-else v-on:click.native="disableSidebar">
              <img :src="link.icon" v-if="link.icon"/>
              <p v-else>{{ link.title }}</p>
            </router-link>
          </li>
        </ul>
      </li>
      
      <li>
        <h3 class="title" @click="(event) => {toggle(event, 'EMISIUNI')}">EMISIUNI</h3>
        <ul v-if="options['EMISIUNI']===true">
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Apropo TV
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              La Maruta
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Vorbeste Lumea
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Ce se intampia, Doctore?
            </router-link>
          </li>
        </ul>
      </li>
      <li>
        <h3 class="title" @click="(event) => {toggle(event, 'STIRI')}">STIRI</h3>
        <ul v-if="options['STIRI']===true">
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Stirileprotv
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Romania Te lubesc
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Dupa 20 de ani
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
             iLikeIT
            </router-link>
          </li>
        </ul>
      </li>

      <li>
        <h3 class="title" @click="(event) => {toggle(event, 'SERIALE')}">SERIALE</h3>
        <ul v-if="options['SERIALE']===true">
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Las Fierbinti
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Atletico Textila
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              Lectii de viata
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
             Ai nostri
            </router-link>
          </li>
        </ul>
      </li>

      <li>
        <h3 class="title" @click="(event) => {toggle(event, 'FILME')}">FILME</h3>
        <ul v-if="options['FILME']===true">
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              TRANSFORMERS
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              TRANSFORMERS 2
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
              TRANSFORMERS 3
            </router-link>
          </li>
          <li>
            <router-link to="#"  v-on:click.native="disableSidebar">
             TRANSFORMERS 4
            </router-link>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <slot id="push-content"></slot>
  <div id="pusher" :class="{ active: active }" v-on:click="disableSidebar">
  </div>
</div>
</template>
<script>
export default {
  name: 'SidebarNav',
  props: [
    'active',
    'context'
  ],
  data () {
    return {
      options: [],
      optionValues: []
    }
  },
  methods: {
    disableSidebar () {
      if (this.active) {
        this.$emit('toggleSidebar')
      }
    },
    isExternalLink (url) {
      if (/^(f|ht)tps?:\/\//i.test(url)) {
        return true
      } else {
        return false
      }
    },
    toggle (e, title) {
      if (!this.options[title]) {
        this.$set(this.options, title, true)
      } else {
        this.$set(this.options, title, !this.options[title])
      }
    }
  }
}
</script>
<style scoped>
#sidebar-nav {
  height: 100%;
  min-height: 100%;
  /* Disable because scrollto is not working for now
  overflow-x: hidden;
  */
  position: relative;
  background: #131313;
  width: 100%;
  display: table;
}

#sidebar {
  width: 300px;
  background: #1c4de6;
  position: fixed;
  top: 85px;
  left: 0;
  bottom: 0;
  overflow-y: auto;
  z-index: 999;
  transform: translate3d(-300px, 0, 0);
  transform-origin: 100% 50%;
  transition: transform 0.5s;
}

@media screen and (max-width: 959px) {
  #sidebar {
    top: 106px;
  }
}
@media screen and (max-width: 537px) {
  #sidebar {
    top: 114px;
  }
}
@media screen and (max-width: 535px) {
  #sidebar {
    top: 142px;
  }
}
@media screen and (max-width: 483px) {
  #sidebar {
    top: 116px;
  }
}

#sidebar.active {
  transform: translate3d(0, 0, 0);
  box-shadow: 4px 4px 4px rgba(0,0,0,0.3);
}

#pusher {
  background: black;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  height: 100%;
  width: 100%;
  display: table;
  table-layout: fixed;
}

#pusher.active {
  transform: translate3d(300px, 0, 0);
  opacity: 0.5;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  margin: 0 auto;
}

#pusher.active #header,
#pusher.active #content,
#pusher.active #footer {
  pointer-events: none;
}

ul {
  margin: 0;
  padding: 0;
}

li {
  list-style-type: none;
  cursor: hand;
}
.title {
  font-size: 20px;
  text-transform: uppercase;
  padding-left: 50px;
}

ul li li a {
  padding: 10px 0 10px 100px;
}
a {
  color: white;
  display: block;
}

a:hover {
  text-decoration: none;
  background: #20a4f3;
}

</style>
