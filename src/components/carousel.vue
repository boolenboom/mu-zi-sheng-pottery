<template>
    <div class="carousel">
        <div class="warpper" @mousedown.prevent="controller($event)" @mousemove.prevent="controller($event)" @mouseup.prevent="controller($event)">
            <div class="title">{{sectiontitle}}</div>
            <ul class="contents" :data-offset='offset' :style="`--offset:${offset}px;`">
                <li v-for="i of quantity" class="item" :key="i" draggable="false">
                    <div class="pic">
                        <!-- <img src="#" alt="" class="pic"> -->
                    </div>
                    <p class="itemname">Card name</p>
                </li>
            </ul>
            <a href="#" class="button">All</a>
        </div>
    </div>
</template>

<script>
function createState(){
    let on = false;
    let offset = 0;
    let startpoint = 0;
    let currentoffset = 0;
    return {
        mousedown:function(val){
            on = true;
            startpoint = val;
        },
        mousemove:function(val){
            if(on) currentoffset = val - startpoint;
        },
        mouseup:function(val){
            on = false;
            offset += currentoffset;
            currentoffset = 0;
            console.log('slide event end ,end point:',val);
            console.log(this.getstate());
        },
        getstate:function(){
            return [on,offset,startpoint,currentoffset];
        },
        getoffset:function(){
            return offset + currentoffset;
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
            UIcontroller:createState(),
            offset:0
        }
    },
    methods:{
        controller(e){
            // console.log(e,this.moveOffset);
            this.UIcontroller[e.type](e.x);
            this.offset = this.UIcontroller.getoffset();
        }
    }
};
</script>

<style lang="scss" scoped>

.carousel{
    position: relative;
    z-index: 30;
    width: 100%;
    font-size: 1.5rem;
    overflow-x: hidden;
    .title{
        top: 10%;
        font-size: 12rem;
        writing-mode: vertical-lr;
        position: absolute;
    }
    .button{
        font-size: 4rem;
        position: absolute;
        top: 10%;
        right: 0;
    }
    .contents{
        margin-top: 10rem;
        width: 100%;
        display: flex;
        flex-flow: row nowrap;
        margin-left: clamp(0px, calc(20% + 5% - 16px + var(--offset)), calc(20% + 5% - 16px)) ;
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
}
.warpper{
    width: 100%;
}
</style>