<template lang="pug">
.wrapall
  .header
    a.logo(href="#")
      img(src="https://i.pinimg.com/originals/1b/8e/62/1b8e628911c42bb354ff8c60e2cd2f97.jpg", alt="")

    nav(:class="{'fixednav' : navappear}")
        a(href="#") 首頁
        a(href="#") 旅遊網站
        a(href="#") 去哪裡玩
        a(href="#") 跟隨我們
        a(href="#") 更多發現

    .cross(@click="navappear=!navappear" :class="{'grabed' : navappear}")
      .line.line1(:class="{'grabed' : navappear}")
      .line.line2(:class="{'grabed' : navappear}")
      .line.line3(:class="{'grabed' : navappear}")
  .banner
    .banner_txt
      h1 尋找旅行的意義
      h4 那次出遊&nbsp;&nbsp;&nbsp;&nbsp;風
      h4.secon 從我的指尖溜過
      p 這次走過的路，我會把它放進屬於我的記憶盒子<br>因此，它將成為我生命中的一部份
      .form
        input(type="text" placeholder="想去哪逍遙~~" v-model="searchitem" )
        button(type='submit')
          fa.fa(:icon="['fas' , 'magnifying-glass']")
   
  .section.travelplace
    .wrap
      .waitdata(v-if="apidata.length==0 ")
        fa.fa_spin(:icon="['fas' , 'spinner']" ) 
        span 載入中...
      .wrongsearch(v-if="searched.length==0 & searchitem!='' ") 嗚嗚...沒有搜尋到 "{{searchitem}}"，再搜尋一次吧!
      .item(v-if="searched.length>0" v-for="(i,index) in searched.slice(((currentpage-1)*9),(currentpage*9))" )
        .pic
          img(v-bind:src="i.Photo")
          h2 {{i.Name}}
          .tag(v-show="index<3")
            span 最熱門
        .text
          .arrd
            fa.fa(:icon="['fas' , 'location-dot']")
            span {{i.Address}}
          .tel
            fa.fa(:icon="['fas' , 'phone']")
            span {{i.Tel}}
          .place {{i.City}}&nbsp;/&nbsp;{{i.Town}}
          p {{i.Introduction.substring(0,90)}}
        label(v-if="!i.isOpenDetail" v-on:click="i.isOpenDetail=!i.isOpenDetail" ) more
        .about
          .detailbox(v-if="i.isOpenDetail")
            h2 {{i.Name}}
            p {{i.Introduction}}    
        .overplay(v-if="i.isOpenDetail" v-on:click="i.isOpenDetail=!i.isOpenDetail")
    .pages
      ul(v-if="searched.length!=0 ")
        li(@click="currentpage=1")
          fa.fa(:icon="['fas' , 'angles-left']" ) 
        li(@click="cutpage()")
          fa.fa(:icon="['fas' , 'angle-left']")
        li(v-if="(currentpage-2)>1 & totalpages>6 ")
          fa.fa(:icon="['fas' , 'ellipsis']")
        li(v-for=" i in showpage" v-on:click="currentpage = i" :class="{active : currentpage == i}") {{i}}
        li(v-if="(currentpage+2)<totalpages & totalpages>6")
          fa.fa(:icon="['fas' , 'ellipsis']")
        li(@click="addpage()")
          fa.fa(:icon="['fas' , 'angle-right']")
        li(@click="currentpage=totalpages")
          fa.fa(:icon="['fas' , 'angles-right']")

  .clearnav(v-if="navappear" @click="navappear=false")

</template>


<style lang="sass">
*
  list-style: none
  box-sizing: border-box
  padding: 0
  margin: 0
  // border: solid 1px 
body,html
  padding: 0
  margin: 0
.wrapall
  background-color: #fff
  height: 100vh
  .header
    display: flex
    align-items: center
    background-color: #fff
    padding: 0px 10px
    width: 100%
    border: 1px solid
    .logo
      width: 130px
      height: 100px
      overflow: hidden
      img
        width: 180%
        vertical-align: top
        margin-left: -50px
        margin-top: -25px
    nav
      margin: auto
      opacity: 0
      transition: .3s
      a
        text-decoration: none
        color: #08d19c
        padding: 5px 20px
        position: relative
        transition: 0.5s
        font-size: 20px
        font-weight: 900
        top: 0px
        &:hover 
          top: -5px
          &:after
            left: 0%
            right: 0%
        &:after
          content:""
          position: absolute
          left: 50%
          right: 50%
          height: 2px
          bottom: 0px
          background-color: #057859
          transition: 0.5s
    .fixednav
      opacity: 1
    .cross
      width: 50px
      height: 50px
      border-radius: 10px
      border: 5px solid #155799
      position: relative
      cursor: grab
      .line
        width: 65%
        height: 5px
        background-color: #155799
        position: absolute
        top: 50%
        left: 50%
        transform: translate(-50%,-50%)
        transition: transform .5s
      .line1
        transform: translate(-50%,-250%)
      .line3
        transform: translate(-50%, 150%)
      .line.grabed
        width: 75%
        background-color: #fff
      .line1.grabed
        transform: translate(-50%,-50%) rotate(45deg)
      .line2.grabed
        opacity: 0
      .line3.grabed
        transform: translate(-50%,-50%) rotate(-45deg)
    .grabed
      background-color: #155799
  .clearnav
    position: absolute  
    top: 100px
    left: 0
    right: 0
    bottom: 0
    background-color: transparent
  .banner
    height: 500px
    background: linear-gradient(115deg , transparent 30% ,#159957 30%, #155799),url("https://picsum.photos/1200/800?random=20")  center center/ auto 100%
    background-size: 100% auto
    // box-shadow: 0px 0px 10px #057859 inset
    .banner_txt
      width: 50%
      height: 100%
      position: relative
      left: 40%
      display: flex
      flex-direction: column
      justify-content: center
      color: #fff
      min-width: 500px
      // border: 1px solid
      h1
        font-size: 40px
        margin-bottom: 10px
      h4
        font-size: 30px
      h4 + h4
        font-size: 30px
        margin-bottom: 10px
      p
        font-size: 25px
      .form
        display: flex
        border-radius: 10px
        overflow: hidden
        border: 1px solid #fff
        box-shadow: 0 0 8px #eee
        max-width: 800px
        position: relative
        top: 50px
        left: -10%
        input,button
          border: none
          line-height: 35px
          background-color: transparent
          color: #fff
        input
          width: 80%
          padding: 0 10px
          &:focus
            outline: none
          &::placeholder
            color: rgba(225,225,225,.8) 
            font-size: .8em
        button
          position: absolute
          right: 0
          padding: 0 20px 0 10px
          text-align: center
          cursor: pointer
  .section.travelplace
    padding: 20px 0px 80px
    background: linear-gradient(45deg,#007991,#78ffd6)
    position: relative
    .wrap
      width: 1200px
      margin: auto
      display: flex
      flex-wrap: wrap
    .waitdata
      color: #fff
      position: absolute
      top: 50%
      left: 50%
      transform: translate(-50%,-50%)
      .fa_spin
        animation: spin 2s linear infinite
        margin-right: 10px
        font-size: 30px
        vertical-align: middle
      span
        font-size: 20px
        vertical-align: middle
    @keyframes spin 
      0%
        transform: rotate(0deg)
      100%
        transform: rotate(360deg)
    .wrongsearch
      margin: auto
      color: #fff
      text-align: center
      font-size: 30px
      position: absolute
      top: 50%
      transform: translateY(-50%)
      right: 0
      left: 0
    .item
      width: 380px
      height: 470px
      margin: 0px 10px 20px
      padding: 0px 0 35px
      background-color: #eee
      display: flex
      flex-direction: column
      // align-items: center
      position: relative
      border-radius: 5px
      overflow: hidden
      .pic
        width: 100%
        height: 50%
        flex-grow: 0
        position: relative
        overflow: hidden
        h2
          color: #fff
          position: absolute
          left: 10px
          bottom: 6px
          font-weight: 600
          font-size: 1.3em
          text-shadow: 0 0 0.2em #888, 0 0 0.2em #888, 0 0 0.2em #888
        .tag
          background-color: #ff8000
          color: #eee
          font-weight: 600
          font-size: 20px
          width: 50%
          text-align: center
          line-height: 1.6em
          transform: rotate(45deg)
          position: absolute
          right: -65px
          top: 18px
        img
          width: 120%
          vertical-align: bottom
          transition: .8s

          &:hover
            transform: scale(1.1)
      .text
        padding: 8px 5% 0px
        text-align: left
        .arrd
          font-size: 15px
          margin-top: 0px
          .fa
            color: #f22727
            margin-right: 7px
        p::after
          content:"  …"
        .tel
          font-size: 15px
          font-family: 'Lobster', cursive
          .fa
            color: green
            margin-right: 5px
      .text > *
        margin-bottom: 8px
        line-height: 1.2em
      label
        border: none
        border: 1px solid #007991
        border-radius: 10px
        padding: 2px 5px
        position: absolute
        cursor: pointer
        bottom: 10px
        right: 20px
        box-shadow: 0 0 5px #eee
        font-size: 10px
        font-weight: 800
        color: #007991
        &:hover
          background-color: rgba(#007991,.8)
          color: #fff
    .about
      .detailbox  
        padding: 5px 35px 15px
        width: 700px
        background: #baffee
        position: fixed
        top: 50%
        left: 50%
        transform: translate(-50%,-50%)
        z-index: 1
        border: 2px solid rgba(#fff,.5)
        border-radius: 10px
        box-shadow: 0 0 5px #fff
        z-index: 2
        h2
          line-height: 1.6em
          color: darken(#007991,.8)
        p
          text-indent: 2em
          color: darken(#888,.3)
          font-weight: bold
          letter-spacing: .1em
          text-align: justify
          line-height: 1.6em
        button
          border: none
          border: 2px solid rgba(#007991,.5)
          padding: 2px 7px
          position: absolute
          right: 10px
          top: 10px
          border-radius: 2px
          color: #888
          cursor: pointer
          &:hover
            color: #fff
            background-color: #007991
    .overplay
      position: fixed
      left: 0
      top: 0
      right: 0
      bottom: 0
      background-color: rgba(#00596b,.3)
      z-index: 1
    .pages
      display: flex
      justify-content: center
      ul
        display: flex
        user-select: none
        margin-top: 20px
        li
          padding: 2px
          cursor: pointer
          font-family: 'Pacifico', cursive
          width: 50px
          text-align: center
          transform: translateY(0px)
          transition: transform .5s
        .active
          color: #fff
          transform: translateY(-8px)
          border-bottom: 3px solid #fff
</style>

<script>
import axios from 'axios'
export default {
  async fetch(){
    // let vuethis = this
    await axios.get("https://data.coa.gov.tw/Service/OpenData/ODwsv/ODwsvAttractions.aspx")
                .then(
                  response => {
                    for( let i=0; i<response.data.length ; i++){
                      response.data[i].isOpenDetail = false
                    }
                    this.apidata = response.data
                  }
                )
  },
  data(){
    return{
      apidata: [],
      searchitem: '',
      currentpage: 1,
      navappear: false

    }
  },
  computed:{
    searched: function() {
      return this.apidata.filter(
        item=> item.Address.match(this.searchitem) || item.Name.match(this.searchitem)
      );
    },
    totalpages: function() {
      let x = Math.floor(this.searched.length/9)
      if (this.searched.length % 9 >0) {
        x = x + 1
      }
      this.currentpage=1
      console.log(x)
      return x
    },
    showpage: function() {
      const showarray=[]
      if (this.currentpage-2 > 1 & this.currentpage+2 < this.totalpages){
        for (let i =this.currentpage-2 ; i < this.currentpage+3; i++){
          showarray.push(i)
        }
      }
      else if(this.currentpage-2 <= 1){
        if (this.totalpages>6){
          for (let i =1 ; i < 7; i++){
            showarray.push(i)
          }
        }
        else {
          for (let i =1 ; i < this.totalpages+1; i++){
            showarray.push(i)
          }
        }
      }
      else if(this.currentpage+2 >= this.totalpages){
        for (let i = this.totalpages-5 ; i < this.totalpages+1 ; i++){
          showarray.push(i)
        }
      }
      return showarray
    }
  },
  
  methods:{
    addpage(){
      if (this.currentpage<this.totalpages){
        this.currentpage +=1
      }
    },
    cutpage(){
      if (1<this.currentpage){
        this.currentpage -=1
      }
    },
    
  }
}
</script>
