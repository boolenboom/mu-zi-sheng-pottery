<template>
    <div class="carousel">
        <div class="warpper" @mousedown.prevent="controller($event)" @mousemove.prevent="controller($event)" @mouseup.prevent="controller($event)">
            <div class="title fz-xl">{{sectiontitle}}</div>
            <ul class="contents" :data-offset='offset' :style="`--offset:${offset}%;`">
                <li v-for="i of quantity" class="item" :key="i" draggable="false">
                    <div class="pic">
                        <!-- <img src="#" alt="" class="pic"> -->
                    </div>
                    <p class="itemname fz-s">Card name</p>
                </li>
            </ul>
            <a href="#" class="button fz-m">All</a>
        </div>
        <!-- <div class="ruler"></div> -->
    </div>
</template>

<script>
// function debounce(func, wait = 100, immediate = true) {
//         var timeout;
//         return function() {
//             var context = this, args = arguments;
//             var later = function() {
//                 timeout = null;
//                 if (!immediate) func.apply(context, args);
//             };
//             var callNow = immediate && !timeout;
//             clearTimeout(timeout);
//             timeout = setTimeout(later, wait);
//             if (callNow) func.apply(context, args);
//         };
//     };
function createStatus(){
    let on = false;
    let speed = 0.8;
    let startpoint = 0;
    let offset = 0,currentoffset = 0,initoffset = 0;
    let min = 0,max = 0;
    let onecycle = 0;
    let clamp = function(min,val,max){
            return Math.min(max,Math.max(min,val));
        },
        snap = function(val){
            return Math.round(val / onecycle ) * onecycle;
        };
        // damp = function(result,val,middle){
        //     let coefficient = (middle - result)/(val + result);
        //     console.log(coefficient);
        //     return val;
        // }
    return {
        mousedown:function(val){
            on = true;
            startpoint = val;
        },
        mousemove:function(val){
            if(!on) return; 
            console.log('mousemove');
            currentoffset = (val - startpoint) * speed;
        },
        mouseup:function(val){
            let temp = currentoffset;
            currentoffset = 0;
            on = false;
            offset += temp;
            offset = clamp(min,offset,max);
            offset = snap(offset);
            console.log('slide event end ,end point:',val);
            console.log(this.getstate());
        },
        getstate:function(){
            return [on,offset,startpoint,currentoffset,initoffset];
        },
        getoffset:function(){
            return -(initoffset + offset + currentoffset);
        },
        initial:function(obj){
            initoffset = obj.initoffset;
            min = obj.min;
            max = obj.max;
            onecycle = obj.onecycle;
        }
    }
}
export default {
    name:'carousel',
    props:{
        sectiontitle:{type:String,default:'Intro'},
        quantity:{type:Number,default:3},
        countdown:{type:Number,default:3}
    },
    data(){
        return{
            UIcontroller:createStatus(),
            offset:0
        }
    },
    methods:{
        controller(e){
            // console.log(e,this.moveOffset);
            this.UIcontroller[e.type](-e.x);
            this.offset = this.UIcontroller.getoffset();
        }
    },
    mounted:function(){
        let setting={
            initoffset:-120,
            min:0,
            max:(this.quantity - 1) * 140,
            onecycle:140
        }
        this.UIcontroller.initial(setting);
        this.offset = this.UIcontroller.getoffset();
        console.log('mounted');
    }
};
</script>

<style lang="scss" scoped>

.carousel{
    position: relative;
    z-index: 30;
    width: 100%;
    // font-size: 1.5rem;
    overflow-x: hidden;
    .title{
        top: 10%;
        left: -2%;
        // font-size: 10rem;
        writing-mode: vertical-lr;
        position: absolute;
    }
    .button{
        // font-size: 4rem;
        position: absolute;
        top: 10%;
        right: 0;
    }
    .contents{
        margin-top: 10rem;
        width: 100%;
        display: flex;
        flex-flow: row nowrap;
        // margin-left: clamp(0px, calc(20% + 5% - 16px + var(--offset)), calc(20% + 5% - 16px)) ;
        margin-left: calc(-1 * (50% - 15%));
        .item{
            list-style-type: none;
            flex:50% 0 0;
            // height: calc(20vh + 300px);
            outline: 1px solid #000;
            padding: 16px;
            display: flex;
            flex-flow: column nowrap;
            align-items: center;
            justify-content: center;
            box-sizing: border-box;
            transform: translateX(var(--offset));
            transition: transform .3s cubic-bezier(.1,.7,.5,1);
            .pic{
                min-width: 192px;
                width: 100%;
                height: 600px;
                border-radius: 12px;
                background-color: var(--main-color);
            }
            .itmename{
                width: 100%;
                margin-top: 3rem;
            }
            &:not(:first-child){
                margin-left: 20%;
            }
        }
    }
    .ruler{
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        margin: auto;
        width: 50%;
        height: 80vh;
        border: 1px solid #fff;
    }
}
.warpper{
    width: 100%;
}
</style>