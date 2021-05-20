<template>
  <div class="slide">
    <div class="slide_title">
      <h3>{{ title }}</h3>
<!--      <span class="slide_morebtn">더보기
          <button><svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M6 13L11 8L6 3" stroke="#C0C0C0" stroke-width="2" stroke-linecap="round"/>
          </svg>
          </button>
      </span>-->
    </div>
    <div class="slide_container">
      <div class="slide_element" v-for="data in dataObj" :key="data.id">
        <div class="slide_img" :style="[computedImageSize, {backgroundImage: 'url('+data.image+')'}]"></div>
        <p>{{ data.name.length > 11 ? data.name.slice(0,11) + '...' : data.name }}</p>
        <span>{{ data.address.length > 36 ? data.address.slice(0,36) + '...' : data.address }}</span>
        <div v-if="data.phone" class="slide_price">{{ data.phone }}</div>

        <button @click="() => data.detail = true">MAP</button>
        <Detail v-if="data.detail"
                @close="() => data.detail = false"
                :keyword='data.name'/>
      </div>

    </div>


    <ul class="page">
      <li v-for="i in pagenation"
          :key="i"
          :class="{'selected' : i === currentPage}"
          @click="clickPage(i)"
      >{{i}}</li>
    </ul>
  </div>
</template>

<script>
import Detail from "@/components/Detail";

export default {
  name: 'Slide',
  components: {
    Detail,
  },
  props: {
    title: String,
    dataObj: Array,
    currentPage: Number,
    pageLength: Number,
    imageWidth: String,
    imageHeight: String,
  },
  data() {
    return {
      isDetailOpen: false
    }
  },
  computed: {
    computedImageSize() {
      return {
        width: this.imageWidth + 'px',
        height: (this.imageHeight ? this.imageHeight : this.imageWidth) + 'px'
      }
    },
    pagenation() {
      return Math.ceil(this.pageLength/5)
    }
  },
  methods: {
    clickPage(i) {
      this.$emit('clickPage', i)
    },
    /*open() {
      this.isDetailOpen = true
    },
    close() {
      this.isDetailOpen = false
    },*/
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
