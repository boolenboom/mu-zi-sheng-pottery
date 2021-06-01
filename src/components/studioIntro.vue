<template>
    <div class="section studioIntro">
        <div class='background'>
            <div class="comp">
                <div class='Rect-01'>
                    
                </div>
                <div class='Rect-02'>
                    
                </div>
                <div class='Rect-03'>
                    
                </div>
                <div class='Rect-04'>
                    
                </div>
                <div class='Rect-05'>
                    
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
.studioIntro {
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
                transform: rotate(-45deg) scale(2);
            }
        }
        [class*='-01'] {
            top: -25%;
            width: 150%;
            height: 50%;
            img {
                position: relative;
                top: 15%;
                left: -3%;
            }
        }
        [class*='-02'] {
            top: 25%;
            width: 25%;
            height: 25%;
            img {
                position: relative;
                top: -5%;
            }
        }
        [class*='-03'] {
            top: 25%;
            left: 50%;
            width: 100%;
            height: 25%;
            img {
                position: relative;
                top: -15%;
                left: -10%;
            }
        }
        [class*='-04'] {
            bottom: 25%;
            width: 50%;
            height: 25%;
            img {
                position: relative;
                top: 80%;
                left: -50%;
            }
        }
        [class*='-05'] {
            bottom: 0;
            left: 50%;
            width: 25%;
            height: 50%;
            img {
                position: relative;
                top: -15%;
                left: -100%;
            }
        }
    }
}
</style>