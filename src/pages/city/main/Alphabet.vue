<template>
  <ul class="list">
        <!-- v-for="(item,key) of cities" :key="key"  {{key}}-->
    <li class="item" 
    v-for="item of letters"
    :key="item"
    :ref="item"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
    @click="handleLetterClick">{{item}}</li>
  </ul>
</template>

<script>
export default {
  name:'cityAlphabet',
  props:{
    cities:Object
  },
  computed:{
    letters (){//构建数组letters替代cities
      const letters=[]
      for (let i in this.cities){
        letters.push(i)
      }
      return letters
    }
  },
  data (){
    return {
      touchStatus:false,
      startY:0,
      timer:null
    }
  },
  updated (){
    this.startY = this.$refs['A'][0].offsetTop
  },
  methods:{
    handleLetterClick (e){
      this.$emit('change',e.target.innerText)
      //console.log(e.target.innerText)
    },
    handleTouchStart (){
      this.touchStatus=true
    },
    handleTouchMove (e){
      // if(this.touchStatus){
      //   //const startY = this.$refs['A'][0].offsetTop
      //   //console.log(startY)用updated可优化
      //   const touchY = e.touches[0].clientY-79
      //   //console.log(touchY)
      //   //const index = Math.floor((touchY - startY) /20)
      //   const index = Math.floor((touchY - this.startY) /20)
      //   //console.log(index)
      //   if (index >= 0 && index <this.letters.length){
      //     this.$emit('change',this.letters[index])
      //   }       
      // }

      if(this.touchStatus){
        if(this.timer){//函数节流
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
          const touchY = e.touches[0].clientY-79
          const index = Math.floor((touchY - this.startY) /20)
          if (index >= 0 && index <this.letters.length){
          this.$emit('change',this.letters[index])
          }
        },16)
      }
    },
    handleTouchEnd (){
      this.touchStatus=false
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '~styles/varibles.styl'
  .list
    display :flex
    flex-direction :column
    justify-content :center
    position :absolute
    top :1.58rem
    right :0
    bottom :0
    width :.4rem
    .item
      line-height :.4rem
      text-align :center
      color :$bgColor
</style>


