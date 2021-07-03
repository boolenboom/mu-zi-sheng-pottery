<template>
    <section id='main'>
        <!-- <div class="test-baseline"></div> -->
        <div class="wrapper" :style="`--order-delay:${orderdelay}s;`">
            <div class="title">
                <transition-group :name='fadeoutmove' class="control" :class="animationList" @change='changestate($event)'>
                    <h1 
                    v-for="item,index of carouselContent" 
                    v-show="groupNum == index"
                    :key="`title`+item.id">
                        {{item.text.title}}
                    </h1>
                </transition-group>
            </div>
            <div class="content" :class="{slidein:(state==='ready')}">
                <div class="marquee top">
                    <p v-for="i of 6" :key="i" :id='i' :class="animationList">
                        / Art·Handicrafts·Practical /
                    </p>
                </div>
                <transition-group :name="fadeoutmove" tag="div" class="big-img" :class="animationList">
                    <img v-for="item,index of carouselImg" 
                    :key='`img`+item.id' 
                    :src="item.path"
                    v-show="groupNum == index"
                    alt="">
                </transition-group>
                <div class="marquee bottom">
                    <p v-for="i of 6" :key="i" :id='i' :class="animationList">
                        / Art·Handicrafts·Practical /
                    </p>
                </div>
            </div>
            <div class="indicators" :class="animationList">
                <div 
                v-for="item,index of carouselContent"
                :key="item.id"
                class="item" :class="{curr:groupNum == index}"
                @click="groupNum = index"></div>
            </div>
        </div>
    </section>
</template>
<script>
const dataSet = require('../assets/data/attractSection/contentSet.json');
console.log('attractSection:');
console.log(dataSet);
let timeNum = 5;
let classlist = {
    ready:'fadeactive fade',
    run:'fadeactive',
    leave:'fadeactive fade'
}
export default {
    name:'mainlayout',
    data(){
        return {
            groupNum:0,
            fadeoutmove:'fadeout-rightmove',
            timer:null,
            time:timeNum,
            state: 'ready',
            orderdelay:1.2
        }
    },
    props:{
        timetoleave:{
            type:Boolean,
            default:false
        }
    },
    methods:{
        countdown(){
            this.time--;
            if(this.time == 0){
                this.groupNum = 
                this.groupNum == dataSet.length - 1 ? 0 : this.groupNum + 1;
            }
        },
        changestate(e){
            console.log(e);
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
        },
        animationList:function(){
            return classlist[this.state];
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
        setTimeout(()=>this.state='run',100);
    },
    beforeDestroy() {
        clearInterval(this.timer);
    }
}
</script>
<style lang="scss" scoped>
#main {
    background: #85B8CB;
    .wrapper{
        position: relative;
        margin: 0 auto;
        height: 100vh;
        width: 177.778vh;
        @media (max-width: 177vh) {
            margin: 0 calc((100% - 177.778vh) / 2);
        }
        .title{
            position: absolute;
            top: 20%;
            right: 10%;
            @media (max-width: 177vh) {
                right:30%;
            }
            // transform: translate(50%,-50%);
            h1{
                position: absolute;
                height: 90vh;
                writing-mode: vertical-lr;
                font-size: 9rem;
            }
        }
        .content{
            background-color: #fff;
            position: relative;
            width: 73.4375%;
            height: 87.037%;
            transform: rotate(45deg) translateX(-17.6%) translateY(-20%);
            transition: transform var(--order-delay) ease-in-out ;
            &.slidein{
                transform: rotate(45deg) translateX(-100%) translateY(-20%);
            }
            .marquee{
                position: absolute;
                font-size: 24px;
                width: 100%;
                border-top: 1px solid #000;
                border-bottom: 1px solid #000;
                padding: 8px 0;
                overflow-x: hidden;
                display: flex;
                flex-flow: row nowrap;
                &.top{
                    transform: translateY(calc(-100% - 24px));
                }
                &.bottom{
                    transform: translateY(24px);
                }
                p{
                    flex: 1 0 25rem;
                    animation: marquee-left 8s linear infinite;
                }
                @keyframes marquee-left {
                    to{
                        transform: translateX(-100%);
                    }
                }
            }
            .big-img{
                position: relative;
                height: 100%;
                overflow: hidden;
                img{
                    transform: rotate(-45deg);
                    position: absolute;
                    width: 90vh;
                    height: 90vh;
                    top: 15%;
                    left: 45%;
                }
            }
        }
        .indicators {
            display: flex;
            flex-direction: row;
            justify-content: center;
            .item {
                z-index: 50;
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
    .test-baseline {
        z-index: 20;
        position: absolute;
        height: 664px;
        width: 665px;
        top: 330px;
        left: 586px;
        outline: solid 1px #000;
    }
    .fadeout-rightmove-leave-active,
    .fadeout-leftmove-enter-active,
    .fadeout-leftmove-leave-active,
    .fadeout-rightmove-enter-active{
        transition: transform .8s ease,opacity .8s ease;
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
    .fadeactive{
        transition: opacity 1s var(--order-delay) ease;
    }
    .fade{
        opacity: 0;
    }
}
</style>