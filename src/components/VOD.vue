<template>
  <div id="VOD" v-if="context.hasOwnProperty('conf')">
    <div class="container backdrop" :style="{ 'background-image': 'url(' + content.banner_placeholder + ')' }">
      <div class="video-container" :style="{ 'background-image': displayVideo ? null : renderBackgroundImage(content.items[0].poster, 'image') }">
        <div class="overlay">
          <div class="row">
            <div class="content" v-if="!displayVideo">
              <h2 class="title" v-if="content.items[0].programme">{{ content.items[0].programme }}</h2>
              <h3 class="subheading">{{ content.items[0].title }}</h3>
              <p>{{ content.items[0].description | truncateOnWord(200) }}</p>
              <div v-for="button in content.buttons">
                <ProButton
                  :target="button.target"
                  :background="button.background"
                  :color="button.color"
                  :text="button.text"
                  :context="context"
                  />
              </div>
              <p class="duration">
                <img src="../assets/icons/clock.png" />
                {{ content.items[0].duration | duration }}
              </p>
            </div>
            <div class="play-btn" v-on:click="displayVideo = true" v-if="!displayVideo"></div>
            <div class="main-video" v-if="displayVideo">
              <MediaPlayerVideo :item="content.items[0]" :itype="content.itype" :autoplay="true" :context="context" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="epg" :style="{ 'padding-top': displayVideo ? 0 : 'inherit' }">
      <div class="">
        <ProCarousel :context="context" :items="content.items" :grid="content.grid"/>
      </div>
    </div>
    <div class="container">
      <Banner type="horizontal" padding="50"/>
    </div>
  </div>
</template>

<script>
import { duration, truncateOnWord } from '@/filters'
import { renderBackgroundImage } from '@/utils'
import ProCarousel from '@/components/ProCarousel'
import ProButton from '@/components/ProButton'
import MediaPlayerVideo from '@/components/MediaPlayerVideo'
import Banner from './Banner'
export default {
  name: 'VOD',
  components: {
    ProCarousel,
    ProButton,
    MediaPlayerVideo,
    Banner
  },
  props: [
    'content',
    'context'
  ],
  data () {
    return {
      displayVideo: false
    }
  },
  filters: {
    duration,
    truncateOnWord
  },
  methods: {
    renderBackgroundImage
  }
}
</script>

<style scoped>
.backdrop {
  background-repeat: no-repeat;
  background-size: contain;
  width: 90%;
  background-position: top;
}

.video-container {
  background-repeat: no-repeat;
  background-size: 100%;
  background-position: top;
  width: 80%;
  margin: 50px auto;
  height: auto;
}

.video-container .overlay {
  position: relative;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  height: 100%;
  background: linear-gradient(to right, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.7));
  margin-bottom: 40px;
}

.video-container .content {
  max-width: 250px;
  float: right;
  margin-right: 50px;
}

.video-container .title {
  font-size: 20px;
  border-bottom: 1px solid white;
  padding: 0 10px 10px 0;
  margin-right: 20px;
  text-transform: uppercase;
  font-weight: 200;
  padding-top:20px;
  word-wrap: break-word;
}

.video-container .subheading {
  font-size: 25px;
  font-weight: 400;
}

.video-container p {
  font-size: 15px;
  margin: 0 20px 28px 0;
  text-align: justify;
  font-weight: 400;
  line-height: 17px;
}

.video-container .more-videos-btn {
  opacity: 0.6;
  padding: 7px;
  background: #1f55ff;
  border: 1px solid white;
  max-width: 120px;font-size: 12px;
  color: white;
  text-align: center;
  margin-top: 10px;
  display: inline-block;
}

.video-container .more-videos-btn:hover {
  text-decoration: none;
  opacity: 1;
}

.video-container .play-btn {
  width: 100px;
  height: 100px;
  background: url('../assets/temp/main-video-play.png');
  background-size: 100%;
  opacity: 0.8;
  position: absolute;
  bottom: 30px;
  left: 30px;
}

@media screen and (max-width: 568px) {
  
}

.video-container .play-btn:hover {
  cursor: pointer;
  opacity: 1;
}

.epg {
  padding-top: 80px;
}

.main-video {
  position: relative;
  top: 0;
  overflow: hidden;
}

.video-player {
  border: 0;
  width: 992px;
  height: 1100px;
}

.duration {
  display: block;
  margin-top: 30px !important;
  margin-bottom: 200px!important;
}
.video-player-dimensions {
  width: 100%; 
  height: auto;
}

</style>

<style lang="scss" scoped>
.row {
  @include media('<=tablet') {
    margin: 0;
  }
}
#VOD .container.backdrop {
  @include media('<=tablet') {
    background-image: none !important;
    width: 100%;
    padding: 0;
    margin-bottom: 20px;
  }
}
.video-container {
  @include media('<=tablet') {
    margin: 0px auto;
    .overlay {
      min-height: 1px;
    }
    .play-btn {
      width: 50px;
      height: 50px;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
    .content {
      float: none;
      margin: 0 auto;
      p {
        font-size: 10px;
        line-height: 11px;
        margin-bottom: 10px;
      }
      .duration {
        display: none;
      }
      #button {
        text-align: center;
      }
    }
  }
}
</style>
