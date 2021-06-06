<template>
    <div id='studioIntro' class="section">
        <div class="content">
            <h3>
                Studio
            </h3>
            <transition-group tag="div" class="detailimg" name='fadeout-opacity'>
                <img v-for="item,index of IntroImg" 
                v-show="index === currBG" 
                :src="item.path" 
                :key="item.id" alt="">
            </transition-group>
        </div>
        <div class='background' :class="{show:show}">
            <div class="comp">
                <div v-for='item,index of IntroImg' 
                :key="item.id" 
                :style="item.style" 
                @mouseenter="changeBG(index)" 
                @click="detailshow(index)"
                class="Rect" >
                    <img :id='`img` + item.id' :src="item.path" alt="">
                </div>
                <!-- <div class='Rect-01'>
                    <img src="../assets/studioIntro/studio02@2x.jpg" alt="">
                </div>
                <div class='Rect-02'>
                    <img src="../assets/studioIntro/studio05.jpg" alt="">
                </div> -->
                <transition-group tag="div" class="subtitle" name='fadeout-opacity'>
                    <h2 v-for="item,index of Subtitle" 
                    :key="index" 
                    v-show="index === currBG" 
                    class="txt">
                        {{item}}
                    </h2>
                </transition-group>
                <!-- <div class='Rect-03'>
                    <img src="../assets/studioIntro/studio04@2x.jpg" alt="">
                </div>
                <div class='Rect-04'>
                    <img src="../assets/studioIntro/studio03@2x.jpg" alt="">
                </div>
                <div class='Rect-05'>
                    <img src="../assets/studioIntro/studio01@2x.jpg" alt="">
                </div> -->
            </div>
        </div>
    </div>
</template>
<script>
const 
imgdata = require('../assets/data/studioIntro/contentSet.json'),
dataSet = [...imgdata.set];
let path = 'assets/studioIntro';
console.log(dataSet);
export default {
    name:'studioIntro',
    data(){
        return {
            // imgpath:[require('../assets/studioIntro/studio02@2x.jpg')],
            currBG:0,
            mouseEventPermission:true,
            show:false
            };
    },
    computed:{
        IntroImg:function(){
            const global = dataSet.slice(0,1)[0],
            set = dataSet.slice(1),
            widthratio = 100 / global.widthGrid,
            heightratio = 100 / global.heightGrid;
            let item = set.map(function(obj){ //目前因資料獨立所以不做填空白的功能
                    let top = (obj.pos.y - 1) * heightratio,
                    left = (obj.pos.x - 1) * widthratio,
                    width = obj.size.width * widthratio,
                    height = obj.size.height * heightratio;
                    return {
                        id:obj.id,
                        style:`top:${top}%;
                        left:${left}%;
                        width:${width}%;
                        height:${height}%;`,
                        path:require(`../${path}/${obj.filename}`)
                        };
            });
            console.log(item);
            console.log(this.subtitlename);
            return item;
        },
        Subtitle:function(){
            return dataSet.slice(1).map(e=>e.subtitle);
        }
    },
    methods:{
        changeBG(index){
            if(this.mouseEventPermission) this.currBG = index;
        },
        detailshow(index){
            if(!this.mouseEventPermission) return 0;
            this.changeBG(index);
            this.mouseEventPermission = false;
            this.show = true;
        }
    }
}
</script>
<style lang="scss" scoped>
#studioIntro {
    height: auto;
    background-color: #77c;
    .content{
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: row;
        align-items: center;
        // background-color: rgba($color: #364286, $alpha: .3);
        h3{
            width: 30%;
            font-size: 96px;
            letter-spacing: 24px;
        }
        .detailimg{
            width: 70%;
            height: 100%;
            position: relative;
            img{
                position: absolute;
                filter: blur(30px) grayscale(.8);
                left: 0;
                right: 0;
                margin: 0 auto;
                width: 80%;
            }
        }
    }
    .background{
        transition: transform 1.2s cubic-bezier(.61,-0.25,.26,1);
    .comp {
        position: absolute;
        width: 98.4375%;
        height: 175%;
        transform: rotate(45deg) translate(7%, -24.35%);
        // width: 78.75%;
        // height: 140%;
        // transform: rotate(45deg) translate(10%,-19.7%);  //layout ver.
        z-index: 1;
        [class*='Rect'] {
            background-color: var(--BG-color);
            // outline: 1px solid #000;
            overflow: hidden;
            position: absolute;
            img {
                transform: rotate(-45deg);
            }
        }
        [id*='img0']:hover{
            cursor: pointer;
        }
        #img01{
            position: relative;
                top:15%;
                left: -5%;
        }
        #img02{
            position: relative;
                top: -50%;
                left: -30%;
        }
        #img03{
            position: relative;
                top: -75%;
                left: -20%;
        }
        #img04{
        position: relative;
                top: -90%;
                left: -20%;
        }
        #img05{
            position: relative;
                top:-40%;
                left: -30%;
        }
        .subtitle{
            position: absolute;
            top: 40%;
            left: 20%;
            width: 20%;
            height: 20%;
            // top: 25%;
            // left: 25%;
            // width: 25%;
            // height: 25%; //layout ver.
            .txt{
                position: absolute;
                transform: translateY(-50%) rotate(-45deg);
                top:50%;
                width: 100%;
                font-size: 56px;
            }
        }
        // [class*='-01'] {
        //     top: -25%;
        //     width: 150%;
        //     height: 50%;
        //     img {
        //         position: relative;
        //         top:20%;
        //         left: -10%;
        //     }
        // }
        // [class*='-02'] {
        //     top: 25%;
        //     width: 25%;
        //     height: 25%;
        //     img {
        //         position: relative;
        //         top: -10%;
        //         left: -5%;
        //     }
        // }
        // [class*='-03'] {
        //     top: 25%;
        //     left: 50%;
        //     width: 100%;
        //     height: 25%;
        //     img {
        //         position: relative;
        //         top: -75%;
        //         left: -20%;
        //     }
        // }
        // [class*='-04'] {
        //     bottom: 25%;
        //     width: 50%;
        //     height: 25%;
        //     img {
        //         position: relative;
        //         top: -90%;
        //         left: -20%;
        //     }
        // }
        // [class*='-05'] {
        //     bottom: 0;
        //     left: 50%;
        //     width: 25%;
        //     height: 50%;
        //     img {
        //         position: relative;
        //         top:-40%;
        //         left: -30%;
        //     }
        // }
    }
    }
    .fadeout-opacity-enter-active{
        transition: opacity .3s;
    }
    .fadeout-opacity-leave-active{
        transition: opacity .3s;
    }
    .fadeout-opacity-enter,.fadeout-opacity-leave-to{
        opacity: 0;
    }
    .show{
        transform: translateX(100%);
    }
}
</style>