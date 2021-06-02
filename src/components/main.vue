<template>
    <div id='main' class="section">
        <!-- <div class="test-baseline"></div> -->
        <div class="contents">
            <div class="carousel">
                <div class="big-img"></div>
                <div class="text">
                    <h1>Lorem Pormesa</h1>
                    <h3>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Voluptatem, quos </h3>
                </div>
            </div>
            <div class="indicators">
                <div class="item curr"></div>
                <div class="item"></div>
                <div class="item"></div>
            </div>
        </div>
        <div class='background'>
            <div class="Rect-01"></div>
            <div class="full"></div>
        </div>
    </div>
</template>
<script>
export default {
    name:'main',
    data(){
        return null;
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
        transform: translate(-45%, -40%);
        .carousel {
            background-color: rgba($color: #fde067, $alpha: 0.3);
            display: flex;
            flex-direction: row;
            align-items: center;
            .big-img {
                // float: left;
                width: 47.04vh;
                height: 47.04vh;
                border-radius: 50%;
                background-color: rgba($color: #9f9f9f, $alpha: 0.6);
            }
            .text {
                position: relative;
                width: 63.425vh;
                right: 10vh;
                h1 {
                    font-size: 6rem;
                }
                h3 {
                    font-size: 2.25rem;
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
}
</style>