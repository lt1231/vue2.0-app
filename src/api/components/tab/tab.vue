<template>
    <div class="tab">
        <cube-tab-bar
        :userTrasition=false
        :showSlider=true
        v-model="selectedLabel"
        :data="tabs"
        ref="tabBar"
        class="border-bottom-1px"
        >
        </cube-tab-bar>
        <div class="slide-wrapper">
            <cube-slide 
            ref="slide"
            :loop=false
            :autoPlay=false
            :show-dots= false
            :initialIndex="index"
            :options="slideOptions"
            @scroll="onScroll"
            @change="onChange"
            >
               <cube-slide-item v-for="(tab,index) in tabs" :key="index">
                   <component ref="component" :is="tab.component" :data="tab.data"></component>
                </cube-slide-item>
            </cube-slide>
        </div>
    </div>
</template>
<script>
// import Goods from 'components/goods/goods' 
// import Seller from 'components/seller/seller'
// import Ratings from 'components/ratings/ratings'
export default {
    name: 'tab',
    props: {
        tabs:{
            type: Array,
            default() {
                return []
            }
        },
        initialIndex:{
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            index: this.initialIndex,
            slideOptions: {
                listenScroll: true,
                probeType: 3,
                // directionLockThreshold: 0
            }
        }
    },
    computed: {
       selectedLabel: {
           get(){
               return this.tabs[this.index].label
           },
           set(newVal) {
               this.index = this.tabs.findIndex((value) => {
                   return value.label === newVal

               })
           }
       }
    },
    mounted() {
        this.onChange(this.index)
    },
    methods: {
        onChange(current) {
            this.index = current
            const instance = this.$refs.component[current]
            if(instance && instance.fetch) {
                instance.fetch()
            }
        },
        onScroll(pos) {
        //    console.log(pos.x)
           const tabBarWidth = this.$refs.tabBar.$el.clientWidth
           const slideWidth = this.$refs.slide.slide.scrollerWidth
           const transform = -pos.x / slideWidth * tabBarWidth
           this.$refs.tabBar.setSliderTransform(transform)
        }
    },
    // components: {
    //     Goods,
    //     Ratings,
    //     Seller
    // }
}
</script>
<style lang="stylus" scoped>
@import "~common/stylus/variable"

.tab
  display: flex
  flex-direction: column
  height : 100%
  >>> .cube-tab
      padding: 10px 0
     .slide-wrapper
      flex: 1
      overflow: hidden
</style>

