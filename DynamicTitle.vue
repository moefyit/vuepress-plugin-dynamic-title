<template>
</template>

<script>
  export default {
    name: "DynamicTitle",
    data() {
      return {
        originTitle: "",
        recoverTimeout: null,
        config: {
          showIcon: SHOW_ICON,
          showText: SHOW_TEXT,
          hideIcon: HIDE_ICON,
          hideText: HIDE_TEXT,
          recoverTime: RECOVER_TIME
        }
      }
    },
    mounted() {
      document.addEventListener('visibilitychange', () => {
        if (document.hidden) {
          this.hidden()
        }
        else {
          this.visible()
        }
      })
    },
    methods: {
      hidden() {
        this.originTitle = document.title
        if (this.config.hideIcon !== '') {
          let elm = document.querySelector("link[rel=icon]")
          if (elm === null) {
            elm = document.createElement("link")
            elm.setAttribute('rel', 'icon')
            document.head.appendChild(elm)
          }
          elm.setAttribute('href', this.config.hideIcon)
        }
        document.title = this.config.hideText
        clearTimeout(this.recoverTimeout)
      },
      visible() {
        if (this.config.showIcon !== '') {
          document.querySelector("link[rel=icon]").setAttribute('href', this.config.showIcon)
        }
        document.title = this.config.showText + this.originTitle
        this.recoverTimeout = setTimeout(() => {
          document.title = this.originTitle
        }, this.config.recoverTime)
      }
    },
    watch: {
      '$route' (to, from) {
        if (to.path !== from.path) {
          clearTimeout(this.recoverTimeout)
        }
      }
    }
  }
</script>
