<template>
    <div id='main' class="section">
        <!-- <div class="test-baseline"></div> -->
        <div class="contents">
            <div class="carousel">
                <transition-group :name="fadeoutmove" tag="div" class="big-img">
                    <img v-for="item,index of carouselImg" 
                    :key='`img`+item.id' 
                    :src="item.path"
                    v-show="groupNum == index"
                    alt="">
                </transition-group>
                <div class="text">
                    <transition-group :name='fadeoutmove' class="control">
                        <div
                        class="set" 
                        v-for="item,index of carouselContent" 
                        v-show="groupNum == index"
                        :key="`group`+item.id">
                            <h1 :key="`title`+item.id">{{item.text.title}}</h1>
                            <h3 :key="`subtitle`+item.id">{{item.text.subtitle}}</h3>
                        </div>
                    </transition-group>
                </div>
            </div>
            <div class="indicators">
                <div 
                v-for="item,index of carouselContent"
                :key="item.id"
                class="item" :class="{curr:groupNum == index}"
                @click="groupNum = index"></div>
            </div>
        </div>
        <div class='background'>
            <div class="Rect-01"></div>
            <div class="full"></div>
        </div>
    </div>
</template>
<script>
const dataSet = require('../assets/data/attractSection/contentSet.json');
console.log('attractSection:');
console.log(dataSet);
let timeNum = 5;
export default {
    name:'mainlayout',
    data(){
        return {
            groupNum:0,
            fadeoutmove:'fadeout-rightmove',
            timer:null,
            time:timeNum
        }
    },
    methods:{
        countdown(){
            this.time--;
            if(this.time == 0){
                this.groupNum = 
                this.groupNum == dataSet.length - 1 ? 0 : this.groupNum + 1;
            }
        }
    },
    computed:{
        carouselContent:function(){
            return dataSet;
        },
        carouselImg:function(){
            return dataSet.map(function(obj){
                return{
                    id:obj.id,
                    path:require(`../assets/attractSection/${obj.filename}`)
                };
            })
        }
    },
    watch:{
        groupNum:function(newVal,oldVal){
            this.fadeoutmove = newVal > oldVal ? 'fadeout-rightmove' : 'fadeout-leftmove';
            this.time = timeNum;
        }
    },
    mounted() {
        this.timer=setInterval(this.countdown, 1000);
    },
    beforeDestroy() {
        clearInterval(this.timer);
    }
}
</script>
<style lang="scss" scoped>
#main {
    .test-baseline {
        z-index: 20;
        position: absolute;
        height: 664px;
        width: 665px;
        top: 330px;
        left: 586px;
        outline: solid 1px #000;
    }
    .contents {
        z-index: 10;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-45%, -50%);
        .carousel {
            // background-color: rgba($color: #fde067, $alpha: 0.3);
            display: flex;
            flex-direction: row;
            align-items: center;
            .big-img {
                // float: left;
                // width: 47.04vh;
                // height: 47.04vh;
                position: relative;
                width: 55vh;
                height: 55vh;
                border-radius: 50%;
                background-color: rgba($color: #9f9f9f, $alpha: 0.6);
                img{
                    position: absolute;
                    top: 0;
                    bottom: 0;
                    right: 0;
                    left: 0;
                    margin: auto;
                }
            }
            .text {
                position: relative;
                width: 63.425vh;
                height: 40vh;
                right: 20vh;
                .set{
                    height: 100%;
                    position: absolute;
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                    h1 {
                        font-size: 6rem;
                    }
                    h3 {
                        font-size: 2.25rem;
                    }
                }
                
            }
        }
        .indicators {
            display: flex;
            flex-direction: row;
            justify-content: center;
            .item {
                width: 24px;
                height: 24px;
                border-radius: 50%;
                background-color: #ECECEC;
                margin: 62px 0;
                &:hover {
                    cursor: pointer;
                }
            }
            .item:not(:first-child) {
                margin-left: 64px;
            }
            .item.curr {
                background-color: #939393;
            }
        }
    }
    .background {
        [class*='-01'] {
            z-index: 1;
            width: 73.4375%;
            height: 87.037%;
            transform: rotate(45deg) translateX(-17.6%) translateY(-20%);
        }
        .full {
            position: absolute;
            z-index: -1;
            top: 0;
            left: 0;
            width: 100%;
            @media (min-width: 177vh) {
                left: calc((100vw - 177.778vh) / -2);
                width: 100vw;
            }
            height: 100%;
            background-color: var(--BG-color);
        }
    }
    .fadeout-leftmove-leave-active,
    .fadeout-rightmove-enter-active{
        transition: transform 1s ease,opacity 1s ease;
    }
    .fadeout-rightmove-leave-active,
    .fadeout-leftmove-enter-active{
        transition: transform 1s ease,opacity 1s ease;
    }
    .fadeout-rightmove-enter,
    .fadeout-leftmove-leave-to{
        transform: translateX(-25%);
        opacity: 0;
    }
    .fadeout-rightmove-enter-to,
    .fadeout-rightmove-leave,
    .fadeout-leftmove-enter-to,
    .fadeout-leftmove-leave{
        transform: translateX(0);
        opacity: 1;
    }
    .fadeout-rightmove-leave-to,
    .fadeout-leftmove-enter{
        transform: translateX(25%);
        opacity: 0;
    }
}
</style>