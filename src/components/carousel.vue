<template>
    <div class="carousel">
        <div class="warpper" @mousedown.prevent="controller($event)" @mousemove.prevent="controller($event)" @mouseup.prevent="controller($event)">
            <div class="title fz-xl veritcal-write">{{sectiontitle}}</div>
            <ul class="contents" :data-offset='offset' :style="`--offset:${offset}%;`">
                <li v-for="i of quantity" class="item" :key="`card${i}`" draggable="false">
                    <div class="pic">
                        <!-- <img src="#" alt="" class="pic"> -->
                    </div>
                    <p class="itemname fz-s">Card {{i}}</p>
                </li>
            </ul>
            <a href="#" class="button fz-m">All</a>
        </div>
        <!-- <div class="ruler"></div> -->
    </div>
</template>

<script src='https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.js' integrity='sha512-otOZr2EcknK9a5aa3BbMR9XOjYKtxxscwyRHN6zmdXuRfJ5uApkHB7cz1laWk2g8RKLzV9qv/fl3RPwfCuoxHQ==' crossorigin='anonymous'></script>
<script>
let path = 'assets/pottery';
let settingFile = 'list.txt';
function scrollhandler(){
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
            console.log(this.getStatus());
        },
        getStatus:function(){
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
            UIcontroller:scrollhandler(),
            offset:0,
            dataSrc:[],
        }
    },
    methods:{
        controller(e){
            // console.log(e,this.moveOffset);
            this.UIcontroller[e.type](-e.x);
            this.offset = this.UIcontroller.getoffset();
        },
        fetchData(){
            let fetchPath = path + '/' + settingFile;
            console.log('ready fetch, path:',fetchPath);
            axios.get(fetchPath).then(function(res){
                console.log('success', res.data.results);
            }).catch(function(err){
                console.log('fetch fail, error msg:', err);
            });
        }
    },
    computed:{
        showImage(){
            let result = [];
            let temp = [...this.dataSrc];
            let count = 0;
            if(this.quantity > this.dataSrc.length){result = this.dataSrc; return result;}
            while(result.length < this.quantity){
                if(this.quantity - result.length === temp.length) return [...result,...temp];
                if(Math.random() > 0.5) result.push(temp.splice(count,1));
                count++;
            }
            return result;
        }
    },
    mounted(){
        let setting={
            initoffset:-120,
            min:0,
            max:(this.quantity - 1) * 140,
            onecycle:140,
        }
        this.UIcontroller.initial(setting);
        this.offset = this.UIcontroller.getoffset();
        this.fetchData();
        console.log('carousel mounted');
    }
};
</script>

<style lang="scss" scoped>

.carousel{
    position: relative;
    z-index: 101;
    width: 100%;
    // font-size: 1.5rem;
    overflow-x: hidden;
    .title{
        top: 10%;
        left: -2%;
        // font-size: 10rem;
        position: absolute;
    }
    .button{
        // font-size: 4rem;
        position: absolute;
        top: 10%;
        right: 0;
    }
    .contents{
        margin-top: calc(var(--fz-m) * 2);
        width: 100%;
        display: flex;
        flex-flow: row nowrap;
        // margin-left: clamp(0px, calc(20% + 5% - 16px + var(--offset)), calc(20% + 5% - 16px)) ;
        margin-left: calc(-1 * (50% - 15%));
        .item{
            list-style-type: none;
            flex:50% 0 0;
            // height: calc(20vh + 300px);
            background-color: #000;
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
                background-color: #000;
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