<template>
  <div>
    <div class="album-info">
      <img v-lazy="`https://y.gtimg.cn/music/photo_new/T002R300x300M000${albumMid}.jpg`" alt="">
      <div class="singer-info">
        <h3>{{albumName}}</h3>
         <button class="btn btn-sm" @click="favoriteAblum">
            {{favorite ? '取消收藏' : '收藏'}}
        </button>
        <router-link :to="`/singer/${singer.singerMid}`">
          <p style="margin-top:15px">{{singer.singerName}}</p>
        </router-link>
      </div>
    </div>
    <div class="divider text-center" data-content="音乐列表"></div>
    <f-music-list :list="{list}" :hideMoreBtn="true" :hideAlbum="true"></f-music-list>
  </div>
</template>
<script>
import { mapState } from 'vuex'
import fMusicList from './common/MusicList'

export default {
  components: {
    fMusicList
  },
  computed: {
    ...mapState({
      list: state => state.album.list,
      albumMid: state => state.album.albumMid,
      albumName: state => state.album.albumName,
      singer: state => state.album.singer
    }),
    favorite () {
      return this.albumMid in this.$store.state.Favorite.album
    }
  },
  methods: {
    favoriteAblum () {
      let album = {
        albumMid: this.albumMid,
        albumName: this.albumName
      }
      this.favorite ? this.$store.commit('delFavoriteAlbum', album) : this.$store.commit('addFavoriteAlbum', album)
    }
  },
  created () {
    this.$store.commit('albumInit', this.$route.params.id)
    this.$store.dispatch('getAlbum')
  }
}
</script>
<style lang="stylus" scoped>
.album-info
  display flex
  align-items center
  img
    width 180px
    height 180px
    border-radius 10px
  div.singer-info
    margin-left 50px

.divider
  height 20px
  margin-top 20px
</style>
