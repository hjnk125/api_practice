<template>
  <div id="app">
    <Slide title="강원도 강릉시_동물병원 현황"
           :dataObj="this.gangwon"
           :currentPage="gangwonPage"
           :pageLength="gangwonLength"
           @clickPage="(i) => gangwonPage = i"
           imageWidth="177" imageHeight="80"/>

    <Slide title="경기도 화성시_동물병원 현황"
           :dataObj="this.gyeonggi"
           :currentPage="gyeonggiPage"
           :pageLength="gyeonggiLength"
           @clickPage="(i) => gyeonggiPage = i"
           imageWidth="177" imageHeight="80"/>

    <Slide title="부산광역시 영도구_동물병원 현황"
           :dataObj="this.busan"
           :currentPage="busanPage"
           :pageLength="busanLength"
           @clickPage="(i) => busanPage = i"
           imageWidth="177" imageHeight="80"/>
  </div>
</template>

<script>
import Vue from 'vue'
import Slide from './components/Slide.vue'

export default {
  name: 'App',
  components: {
    Slide,
  },
  data() {
    return {
      gangwon: [],
      gangwonPage: 1,
      gangwonLength: 0,
      gyeonggi: [],
      gyeonggiPage: 1,
      gyeonggiLength: 0,
      busan: [],
      busanPage: 1,
      busanLength: 0,
    }
  },
  methods: {
    fetchGangwon() {
      fetch(`https://api.odcloud.kr/api/15077032/v1/uddi:d92bf384-6f47-4559-b401-cacfb7e13aa8?page=${this.gangwonPage}&perPage=5&serviceKey=JrNJKukcbJJdKqysf43GM4ZN2OfNSRmz8%2F6CMz8g1sLK3L1kikw3DZCc8YsDOSqdUrpaFgq754kGI9JZDuFUGw%3D%3D`)
          .then((response) => {
            if(response.ok){
              return response.json();
            }
            throw new Error('Network response was not ok');
          })
          .then((json) => {
            this.gangwon = []
            json.data.forEach((data) => {
              this.gangwon.push({
                name: data['병원명'],
                address: data['주소'],
                phone: data['전화번호'],
                detail: false
              })
            })
            this.gangwonLength = json.matchCount
            return this.gangwon
          })
          .then(()=>{
            // HIGHLIGHT: 이미지 받아오기!
            this.fetchImage(this.gangwon)
          })
          .catch((error) => {
            console.log(error);
          })
    },
    fetchGyeonggi() {
      fetch(`https://api.odcloud.kr/api/3044526/v1/uddi:e92f49c3-ec70-4341-853d-ea9f694b385d?page=${this.gyeonggiPage}&perPage=5&serviceKey=JrNJKukcbJJdKqysf43GM4ZN2OfNSRmz8%2F6CMz8g1sLK3L1kikw3DZCc8YsDOSqdUrpaFgq754kGI9JZDuFUGw%3D%3D`)
          .then((response) => {
            if(response.ok){
              return response.json();
            }
            throw new Error('Network response was not ok');
          })
          .then((json) => {
            this.gyeonggi = []
            json.data.forEach((data) => {
              this.gyeonggi.push({
                name: data['동물병원명'],
                address: data['소재지'],
                phone: data['전화번호']
              })
            })
            this.gyeonggiLength = json.matchCount
          })
          .catch((error) => {
            console.log(error);
          })
    },
    fetchBusan() {
      fetch(`https://api.odcloud.kr/api/15015023/v1/uddi:6e71bb54-707f-48ce-a0b9-994d9a23e990_202002181333
    ?page=${this.busanPage}&perPage=5&serviceKey=JrNJKukcbJJdKqysf43GM4ZN2OfNSRmz8%2F6CMz8g1sLK3L1kikw3DZCc8YsDOSqdUrpaFgq754kGI9JZDuFUGw%3D%3D`)
          .then((response) => {
            if(response.ok){
              return response.json();
            }
            throw new Error('Network response was not ok');
          })
          .then((json) => {
            this.busan = []
            json.data.forEach((data) => {
              this.busan.push({
                name: data['업소명'],
                address: data['사업장소재지(도로명)'],
                phone: data['전화번호']
              })
            })
            this.busanLength = json.matchCount
          })
          .catch((error) => {
            console.log(error);
          })
    },

    // TODO: 랜덤이미지 받아오고싶은데 바로 랜더가 안됨~~~~~~~!
    fetchImage(arr) {
      arr.forEach((obj) => {
        fetch('https://dog.ceo/api/breeds/image/random')
            .then(res => {
              return res.json();
            })
            .then(json => {
              // FIXME: vue는 객체 속성 추가 감지못함! set으로 넣어줘야됨
              // obj.image = json.message
              Vue.set(obj, 'image', json.message)
            })
      })
      return arr
    }
  },
  created() {
    this.fetchGangwon()
    this.fetchGyeonggi()
    this.fetchBusan()
  },
  watch: {
    gangwonPage() {
      this.fetchGangwon()
    },
    gyeonggiPage() {
      this.fetchGyeonggi()
    }
  }
}
</script>

<style>
</style>
