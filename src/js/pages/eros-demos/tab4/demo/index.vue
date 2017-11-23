<template>
    <div>
        <scroller append="tree" class="scroller" paging-enabled="true" scroll-direction="horizontal" @scroll="onscroll" offset-accuracy="0">
             <div class="page" ref="page_1" v-for="n in 10">
                <text>{{n}}</text>
            </div>
           
        </scroller>
    </div>
</template>
<script>

const SCROLL_FULL_WIDTH = 750
const dom = weex.requireModule('dom')

import Tab from './components/tab'
import TabItem1 from './components/tabitem1'
import TabItem2 from './components/tabitem2'
import TabItem3 from './components/tabitem3'

export default {
    globalEvent: {
        appActive() {
            console.log('active')
        },
        appDeactive() {
            console.log('deactive')
        }
    },
    data() {
       return {
            activeIndex: 0
       }
    },
    components: { Tab, TabItem1, TabItem2, TabItem3 },
    methods: {
        
        onscroll(e) {
            let formatOffset = Math.abs(e.contentOffset.x)
            if( formatOffset % SCROLL_FULL_WIDTH === 0 ){
                this.activeIndex = formatOffset / SCROLL_FULL_WIDTH
            }
        }
    }
}
</script>
<style lang="sass" src="./index.scss"></style>
