<template>
  <div :class="getClass">
    <AppBar
      :fixed="keyboardOpen"
      @showEpisode="showEpisode"
      @keyboard="onKeyboard"
    ></AppBar>
    <Player :fixed="keyboardOpen"></Player>
    <MainNav :fixed="keyboardOpen"></MainNav>
    <RouterView
      class="content"
      @showEpisode="showEpisode"
      @keyboard="onKeyboard"
    ></RouterView>
    <Episode v-if="episode" :id="episode" @closed="closeEpisode"></Episode>
  </div>
</template>

<script>
import Component from '~/scripts/component'
import AppBar from '~/components/molecular/AppBar'
import Player from '~/components/molecular/Player'
import MainNav from '~/components/molecular/MainNav'
import Episode from '~/pages/Episode'
import { mapActions, mapState } from 'vuex'

export default new Component({
  name: 'App',
  components: {
    AppBar,
    Player,
    MainNav,
    Episode,
  },
  data() {
    return {
      scrollDir: 0,
      episode: false,
      lastScrollPos: 0,
      lastScrollDelta: 0,
      keyboardOpen: false,
    }
  },
  computed: mapState('app', ['scrollBlock']),
  watch: {
    scrollBlock(v) {
      document.body.classList[v ? 'add' : 'remove']('noscroll')
    },
  },
  methods: {
    ...mapActions('user', ['initGoogleAuth']),
    onScrollDirChange(dir) {
      this.scrollDir = dir
    },
    showEpisode(id) {
      this.episode = id
      document.body.classList.add('noscroll')
    },
    closeEpisode() {
      this.episode = false
      document.body.classList.remove('noscroll')
    },
    onKeyboard(v) {
      if (v) this.keyboardOpen = v
      else setTimeout(() => (this.keyboardOpen = v), 200)
    },
  },
  created() {
    if (window.gapi) {
      this.initGoogleAuth()
    } else {
      window.initGoogleAuth = function() {
        this.initGoogleAuth()
        delete window.initGoogleAuth
      }.bind(this)
    }
  },
})
</script>

<style lang="scss" scoped>
.app {
  min-height: 100vh;
  overflow: auto;

  .content {
    box-sizing: border-box;
    width: 100%;

    @media (orientation: portrait) {
      margin-bottom: 4rem;
      margin-top: 3.5rem;
    }

    @media (orientation: landscape) {
      margin-left: 5rem;
      width: calc(100vw - 5rem);
    }
  }

  .player:not(.hidden) ~ .content {
    margin-bottom: 8rem;
    @media (orientation: landscape) {
      margin-bottom: 4rem;
    }
  }

  .main-nav.fixed ~ .content {
    @media (orientation: landscape) {
      margin-left: 0;
      width: 100%;
    }
  }
}
</style>
