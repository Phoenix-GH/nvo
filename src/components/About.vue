<template>
<div id="about" v-if="context.hasOwnProperty('conf')">
  <div class="b top" v-if="bannerPlacement === 'top'">
    <img :src="banner">
  </div>
  <div class="container">
    <div v-html="item.content"></div>
  </div>
</div>
</template>
<script>
export default {
  name: 'About',
  props: [
    'context',
    'item',
    'bannerPlacement',
    'banner'
  ]
}
</script>
<style>
#about .container {
  width: 100%;
  margin-top: 50px;
  max-width: 970px;
}
.b {
  text-align: center;
}

.b.top img {
  width: 100%;
  height: 120px;
}

blockquote {
  margin: 0 auto!important;
}
blockquote div div{
  width: 100%!important;
  height: 90vw!important;
  background-repeat: no-repeat!important;
}

blockquote div div div,  blockquote div div div div{
  width: 44px!important;
  height: auto;
}
#areas div img{
  width: 100%!important;
  height: auto!important;
}
#areas div img .rating {
  width: auto!important;
}
</style>
