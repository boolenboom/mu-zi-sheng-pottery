<template>
    <div id='studioIntro' class="section">
        <div class='background'>
            <div class="comp">
                <div class='Rect-01'>
                    <img src="../assets/studioIntro/studio02@2x.jpg" alt="">
                </div>
                <div class='Rect-02'>
                    <img src="../assets/studioIntro/studio05.jpg" alt="">
                </div>
                <div class='Rect-03'>
                    <img src="../assets/studioIntro/studio04@2x.jpg" alt="">
                </div>
                <div class='Rect-04'>
                    <img src="../assets/studioIntro/studio03@2x.jpg" alt="">
                </div>
                <div class='Rect-05'>
                    <img src="../assets/studioIntro/studio01@2x.jpg" alt="">
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name:'studioIntro',
    data(){
        return null;
    },
}
</script>
<style lang="scss" scoped>
#studioIntro {
    height: auto;
    .comp {
        position: absolute;
        width: 78.75%;
        height: 140%;
        transform: rotate(45deg) translate(14%, -17.9%);
        z-index: 1;
        [class*='Rect-'] {
            background-color: var(--BG-color);
            // outline: 1px solid #000;
            overflow: hidden;
            img {
                transform: rotate(-45deg);
            }
        }
        [class*='-01'] {
            top: -25%;
            width: 150%;
            height: 50%;
            img {
                position: relative;
                top:20%;
                left: -10%;
            }
        }
        [class*='-02'] {
            top: 25%;
            width: 25%;
            height: 25%;
            img {
                position: relative;
                top: -10%;
                left: -5%;
            }
        }
        [class*='-03'] {
            top: 25%;
            left: 50%;
            width: 100%;
            height: 25%;
            img {
                position: relative;
                top: -75%;
                left: -20%;
            }
        }
        [class*='-04'] {
            bottom: 25%;
            width: 50%;
            height: 25%;
            img {
                position: relative;
                top: -90%;
                left: -20%;
            }
        }
        [class*='-05'] {
            bottom: 0;
            left: 50%;
            width: 25%;
            height: 50%;
            img {
                position: relative;
                top:-40%;
                left: -30%;
            }
        }
    }
}
</style>