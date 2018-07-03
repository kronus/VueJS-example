<template>
  <div class="container" id="app">
    <span>VueJS-Example</span>
    <ul class="list-group list-inline mHeaders">
      <li class="list-group-item">Title</li>
      <li class="list-group-item">Band</li>
      <li class="list-group-item">Date Posted</li>
      <li class="list-group-item">Downloads</li>
      <li class="list-group-item">YouTube</li>
      <li class="list-group-item">MP3</li>
    </ul>
    <ul :id="'sp_' + index" :key="item.id" class="list-group list-inline" v-if="!isHidden" v-for="(item, index) in items">
      <li class="list-group-item">
        {{item.title}}
      </li>
      <li class="list-group-item">
        {{item.original_band}}
      </li>
      <li class="list-group-item">
        {{item.date_posted}}
      </li>
      <li class="list-group-item mZip">
        <a v-bind:href="''+item.download_midi_tabs+''" target="_blank"></a>
      </li>
      <li class="list-group-item mYt">
        <a v-bind:href="''+item.youtube_link+''" target="_blank"></a>
      </li>
      <li class="list-group-item mAudio">
        <a v-bind:href="''+item.download_guitar_m4v+''" target="_blank"></a>
      </li>
    </ul>
    <pagination :records="mFunc" :per-page="30" @paginate="setPage"></pagination>
    <span style="display: none;"><input type="hidden" class="numRows" :value="this.mVar" /></span>
  </div>
</template>

<script>
import axios from 'axios'
import {Pagination} from 'vue-pagination-2'

export default {
  name: 'App',
  data: function () {
    return {
      items: [{
        title: '',
        original_band: '',
        date_posted: '',
        download_midi_tabs: '',
        youtube_link: '',
        download_guitar_m4v: ''
      }],
      mVar: 0,
      isHidden: false
    }
  },
  created: function () {
    this.getPostsViaREST()
  },
  methods: {
    getPostsViaREST: function () {
      axios.get('http://kronusproductions.com/songs_angular/getSongs.php')
        .then(response => { this.items = response.data })
    },
    setPage: function (page) {
      this.page = page
      // console.log(page)
      for (var y = 0; y <= this.mVar - 1; y++) {
        if (typeof (document.getElementById('sp_' + y)) === 'undefined') {

        } else {
          document.getElementById('sp_' + y).style.display = 'none'
          // console.log("y: " + y)
        }
      }
      for (var x = (30 * (this.page - 1)); x <= 30 * (this.page); x++) {
        if (typeof (document.getElementById('sp_' + x)) === 'undefined' || (document.getElementById('sp_' + x)) == null) {
          break
        } else {
          document.getElementById('sp_' + x).style.display = 'block'
        }
      }
    }
  },
  components: {
    Pagination
  },
  computed: {
    mFunc: function () {
      // console.log('Initial this.items.size: ' + Object.keys(this.items).length)
      this.mVar = Object.keys(this.items).length
      // console.log('this.mVar: ' + this.mVar)
      return Object.keys(this.items).length
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  a {
    color: #999;
  }
  .current {
    color: red;
  }
  ul {
    padding: 0;
    list-style-type: none;
  }
  li {
    display: inline;
    margin: 5px 5px;
  }
  ul.list-group:after {
    clear: both;
    display: block;
    content: "";
  }

  .list-group-item {
    float: left;
  }
  .list-group li{
    max-width: 30%;
    min-width: 50px;
    min-height: 48px;
    max-height: 48px;
  }
  .list-group li:first-child{
    width: 200px;
    cursor: pointer;
  }
  .list-group li:nth-child(2){
    width: 200px;
  }
  .list-group li:nth-child(3){
    width: 110px;
  }
  .list-group li:nth-child(4){
    width: 48px;
  }
  .list-group li:nth-child(5){
    width: 48px;
  }
  .list-group li:last-child{
    width: 48px;
  }
  .mZip{
    background: url("http://www.kronusproductions.com/songs_angular/assets/images/mZip.png");
    display: block !important;
    max-width: 48px;
    height: 48px;
    cursor: pointer;
  }
  .mYt{
    background: url("http://www.kronusproductions.com/songs_angular/assets/images/youtube-icon_48x48.png");
    display: block !important;
    width: 48px;
    height: 48px;
    cursor: pointer;
  }
  .mAudio{
    background: url("http://www.kronusproductions.com/songs_angular/assets/images/volume.png");
    display: block !important;
    width: 48px;
    height: 48px;
    cursor: pointer;
  }
  .mZip a{
    display: block !important;
    width: 48px;
    height: 48px;
  }
  .mYt a{
    display: block !important;
    width: 48px;
    height: 48px;
  }
  .mAudio a{
    display: block !important;
    width: 48px;
    height: 48px;
  }
  .mHeaders li{
    background-color: cornflowerblue;
    font-size: 0.85rem;
    color: white;
  }
</style>
