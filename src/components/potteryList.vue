<template>
    <div id="potteryList" class="section">
        <div class='background'>
            <div class="Rect-07"></div>
            <div class="Rect-08"></div>
            <div class="full"></div>
        </div>
    </div>
</template>
<script>
export default {
    name:'potteryList',
}
</script>
<style lang="scss" scoped>
#potteryList {
    [class*='-07'] {
        width: 73.4375%;
        height: 87.037%;
        background-color: #fff;
        transform: rotate(45deg) translate(54.6%, -42.4%);
    }
    [class*='-08'] {
        top: 0;
        left: 0;
        @media (min-width: 177vh) {
            left: calc((100vw - 177.778vh) / -2);
            width: 100vw;
        }
        width: 100%;
        height: 60%;
        transform: translateY(26%);
        background-color: var(--BG-color);
    }
    .full {
        position: absolute;
        z-index: -1;
        top: 0;
        left: 0;
        @media (min-width: 177vh) {
            left: calc((100vw - 177.778vh) / -2);
            width: 100vw;
        }
        width: 100%;
        height: 100%;
        background-color: var(--BG-color);
    }
}
</style>