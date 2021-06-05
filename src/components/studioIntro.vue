<template>
    <div id='studioIntro' class="section">
        <div class="content">
            <h3>
                Studio
            </h3>
            <div class="detailimg">
                <img v-for="item,index of IntroImg" 
                v-show="index === currBG" 
                :src="item.path" 
                :key="item.id" alt="">
            </div>
        </div>
        <div class='background'>
            <div class="comp">
                <div v-for='item,index of IntroImg' 
                :key="item.id" 
                :style="item.style" 
                @mouseenter="changeBG(index)" 
                class="Rect" >
                    <img :id='`img` + item.id' :src="item.path" alt="">
                </div>
                <!-- <div class='Rect-01'>
                    <img src="../assets/studioIntro/studio02@2x.jpg" alt="">
                </div>
                <div class='Rect-02'>
                    <img src="../assets/studioIntro/studio05.jpg" alt="">
                </div> -->
                <div class="subtitle">
                    <h2 class="txt">
                        {{subtitletext}}
                    </h2>
                </div>
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
let path = 'assets/studioIntro',
subtitle = dataSet.slice(1).map(e=>e.subtitle);
console.log(dataSet);
export default {
    name:'studioIntro',
    data(){
        return {
            // imgpath:[require('../assets/studioIntro/studio02@2x.jpg')],
            currBG:0,
            subtitletext:'initial'
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
        }
    },
    methods:{
        changeBG(index){
            this.currBG = index;
            this.subtitletext = subtitle[index];
        }
    }
}
</script>
<style lang="scss" scoped>
#studioIntro {
    height: auto;
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
            img{
                filter: blur(30px) grayscale(.8);
                margin: 0 auto;
                width: 80%;
            }
        }
    }
    .background{
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
                position: relative;
                transform: translateY(-50%) rotate(-45deg);
                top:50%;
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
}
</style>