<template>
  <div :class="{ ...getClassObj, fixed: fixed && wasPortrait }" :style="fixPos">
    <Icon name="library" @click="$router.push('/')"></Icon>
    <Icon name="feed" @click="$router.push('/')"></Icon>
    <Icon name="search" @click="$router.push('/discover')"></Icon>
    <Icon name="account" @click="$router.push('/profile')"></Icon>
  </div>
</template>

<script>
import Component from '~/scripts/component'

export default new Component({
  name: 'MainNav',
  props: {
    fixed: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      wasPortrait: null,
    }
  },
  watch: {
    fixed(v) {
      if (v) this.wasPortrait = window.innerHeight >= window.innerWidth
    },
  },
  computed: {
    fixPos() {
      return !this.fixed || !this.$el ? {} : { top: `${this.$el.offsetTop}px` }
    },
  },
})
</script>

<style lang="scss" scoped>
@import '~/styles/shadows';

.main-nav {
  background-color: white;
  height: 4rem;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  box-shadow: shadow(7);

  position: fixed;
  bottom: 0;

  z-index: 1002;

  .icon {
    cursor: pointer;
  }

  &:not(.fixed) {
    @media (orientation: landscape) {
      width: 5rem;
      height: 100vh;
      flex-direction: column;
      top: 0;

      .player:not(.hidden) ~ & {
        height: calc(100vh - 4rem);
        z-index: 1000;
      }
    }
  }
}
</style>
