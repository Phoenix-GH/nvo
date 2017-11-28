<template>
<div class="container"
  :class="{
  'b-inline': bannerPlacement === 'inline',
  ['page-' + type]: true
  }">
  <div v-if="bannerPlacement === 'inline'" class="backdrop-single" :class="{ inline: bannerPlacement === 'inline'}">
    <img :src="banner"/>
  </div>
  <div class="header"
    :style="{ 'background-image': media[0].itype === 'item_mov_vod' ? 'url(' + media[0].poster + ')' :'url(' + media[0] + ')' }"
    >
    <div class="header-video" v-if="headerVideo">
      <video muted autoplay loop>
        <source :src="headerVideo" type="video/mp4">
      </video>
    </div>
    
    <div class="menu row">
      <ul>
        <li v-for="item in menu">
          <router-link :to="item.link" :class="{active: item.link === $route.path}">{{ item.title }}</router-link>
        </li>
      </ul>
    </div>

    <div class="thumb">
      <img v-if="type === 'show'" :src="icon" width="100" height="100">
      <img v-else :src="icon" width="100" height="100">
    </div>
  </div>
</div>
</template>
<script>
/* global axios */
import Slick from 'vue-slick'
export default {
  name: 'SiteHeader',
  components: {
    Slick
  },
  props: [
    'context',
    'buttons',
    'icon',
    'media',
    'menu',
    'banner',
    'banner-placement',
    'type'
  ],
  data () {
    return {
      headerVideo: null,
      slickOptions: {
        autoplay: false,
        slidesToShow: 8,
        slidesToScroll: 1,
        variableWidth: true,
        arrows: false,
        infinite: true
      }
    }
  },
  mounted () {
    this.getVideoUrl()
  },
  methods: {
    getVideoUrl () {
      if (this.media[0].hasOwnProperty('media')) {
        const url = this.media[0].media + '?sid=test_udid'
        axios.get(url).then(response => {
          if (response.data.media[0].link) {
            this.headerVideo = response.data.media[0].link
          } else {
            this.headerVideo = null
          }
        })
      }
    }
  },
  watch: {
    '$route' (to, from) {
      // This will instantly remove the header video
      // this.headerVideo = null
    },
    media (newValue) {
      // This will give a delay untill the parent API request has finished
      this.headerVideo = null
      if (newValue[0].hasOwnProperty('link')) {
        this.getVideoUrl()
      }
    }
  }
}
</script>
<style scoped>
.container {
  width: 100%;
  max-width: 1235px;
  padding: 0;
  position: relative;
  margin: 0 auto;
}

.b-inline .backdrop-single {
  width: 100%;
  position: absolute;
  height: 740px;
}

.backdrop-single img {
  width: 100%;
}

.header {
  position: relative;
  width: 100%;
  max-width: 975px;
  min-height: 170px;
  height: 100%;
  left: 0;
  right: 0;
  margin: 0 auto;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center top;
  margin-bottom: 50px;
}
.slick-list {
  width: 100vw;
  overflow: hidden;
}
.header-video {
  width: 100%;
  max-width: 975px;
  height: auto;

  position: relative;
}

.header-video video {
  outline: 1px solid red;
  width: 100%;
  min-height:170px;
  left: 0;
}

.b-inline .header {
  margin-bottom: 260px;
  top: 150px;
  height: 385px;
}

.row {
  background: white;
  position: absolute;
  bottom: -40px;
  width: 100%;
  margin: 0;
  padding-left: 160px;
  overflow: hidden;
}

.thumb {
  position: absolute;
  border: 3px solid white;
  left: 74px;
  bottom: -70px;
  margin-right: 10px;
}

.page-channel .thumb {
  background: white;
}

ul {
  padding-top: 5px;
}

li {
  display: inline!important;
  font-size: 16px;
  margin-right: 25px;
  font-weight: 300;
  padding-top: 3px;
}

li a {
  color: #434343;
}

li a.active {
  color: black;
  font-weight: 500;
  text-transform: uppercase;
}

li a:focus {
  text-decoration: none;
}

li.highlight {
  background: #1f55ff;
  color: white;
  padding: 3px 10px;
  text-transform: uppercase;
  font-weight: 400;
  margin-right: 0;
}

@media screen and (max-width: 924px) {
  .thumb {
    bottom: -60px;
  }
}

@media screen and (max-width: 620px) {
  .thumb {
    bottom: -50px;
  }
}

@media screen and (max-width: 415px) {
  .thumb {
    bottom: -35px;
    width: 56px;
    heigth: 56px;
    left: 10px;
  }
  .thumb img {
    width: 50px;
    height: 50px;
  }
  .row {
    padding-left: 50px;
  }
}

.slick-slide {
  width: 100px !important;
}

</style>
