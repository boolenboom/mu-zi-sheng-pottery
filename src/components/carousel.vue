<template>
    <div class="carousel" style="`--main-color:#fff`;">
        <div class="warpper">
            <div class="title">{{sectiontitle}}</div>
            <ul class="contents">
                <li v-for="i of quantity" class="item" :key="i">
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
export default {
    name:'carousel',
    props:{
        sectiontitle:{type:String,default:'Intro'},
        quantity:{type:Number,default:3},
        countdown:{type:Number,default:3}
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
        margin-left: calc(20% + 5% - 16px);
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