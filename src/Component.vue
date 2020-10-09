<template>
  <div class="vue-make-lazy">
    <transition name="fade">
      <slot v-if="intersected" />
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      intersected: false,
      observer: null,
    }
  },
  methods: {
    onIntersect(entries) {
      const el = entries[0]
      if (el.isIntersecting) {
        this.intersected = true
        this.destroy()
      }
    },
    destroy() {
      if (!this.observer) return
      this.observer.disconnect()
    },
  },
  mounted() {
    this.observer = new IntersectionObserver(this.onIntersect.bind(this), {
      root: null,
      threshold: 0,
    })
    this.observer.observe(this.$el)
  },
  beforeDestroy() {
    this.destroy()
  },
}
</script>

<style lang="scss">
.vue-make-lazy {
  .fade-enter-active,
  .fade-leave-active {
    transition-duration: 150ms;
    transition-timing-function: ease-in;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
