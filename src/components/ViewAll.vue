<template>
  <div>

  </div>
</template>

<script>
export default {
  name: 'ViewAll',
  props: {
    title: String,
    dataObj: Array,
    currentPage: Number,
    pageLength: Number,
    imageWidth: String,
    imageHeight: String
  },
  data() {
    return {

    }
  },
  computed: {
    computedImageSize() {
      return {
        width: this.imageWidth + 'px',
        height: (this.imageHeight ? this.imageHeight : this.imageWidth) + 'px'
      }
    },
  },
  methods: {
    clickPage(i) {
      this.$emit('clickPage', i)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
