<template>
    <div class="swipe" :class="{'is-vertical': isVertical}"
         :style="`width: ${swipeWidth}px; height: ${swipeHeight}px;`">
        <div class="swipe-container"
             :style="`${getContainerWidth()} ${getContainerHeight()} ${getActivePageStyle()}`">
            <slot :width="swipeWidth"></slot>
        </div>
        <div class="swipe-counter-container">
            <div v-for="(eachPage, index) in pages"
                 class="swipe-counter"
                 :class="{'is-active': index === activeIndex}"
                 :key="index"
            ></div>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'Swipe',
        data() {
            return {
                swipeWidth: 0,
                swipeHeight: 0,
                activeIndex: 0,
                position: 0,
                pages: [],
                ready: false,
            }
        },
        props: {
            width: {
                Type: Number
            },
            height: {
                Type: Number
            },
            index: {
                Type: Number,
                default: 0,
            },
            isRepeat: {
                Type: Boolean
            },
            isVertical: {
                Type: Boolean
            },
        },
        mounted() {
            this.activeIndex = this.index;
            let swipe = document.querySelector('.swipe');
            let parent = swipe.parentNode;
            this.calcSize(parent);
            window.addEventListener('resize', () => {
                this.calcSize(parent);
            });
            this.onTouch(swipe);
        },
        methods: {
            updated() {
                let swipe = document.querySelector('.swipe');
                if(swipe) {
                    let parent = swipe.parentNode;
                    let pages = [];
                    this.$children.forEach((eachChild) => {
                        pages.push(eachChild.$el);
                    });
                    this.pages = pages;
                    this.calcSize(parent);
                }
                this.activeIndex = this.index;
                if(this.activeIndex > this.pages.length - 1) {
                    this.activeIndex = this.pages.length - 1;
                }
            },
            prev() {
                this.activeIndex--;
                if (this.activeIndex < 0) {
                    if (this.isRepeat) {
                        this.activeIndex = this.pages.length - 1;
                    } else {
                        this.endPrev();
                    }
                }
            },
            next() {
                this.activeIndex++;
                if (this.activeIndex >= this.pages.length) {
                    if (this.isRepeat) {
                        this.activeIndex = 0;
                    } else {
                        this.endNext();
                    }
                }
            },
            endPrev() {
                this.activeIndex = -0.5;
                setTimeout(() => {
                    this.activeIndex = 0;
                }, 200);
            },
            endNext() {
                this.activeIndex = this.pages.length - 0.5;
                setTimeout(() => {
                    this.activeIndex = this.pages.length - 1;
                }, 200);
            },
            getWidth() {
                return this.swipeWidth;
            },
            onTouch() {
                const OFFSET_DISTANCE = 50;
                let startX = -1;
                let startY = -1;
                let endX = -1;
                let endY = -1;
                let element = this.$el;
                element.addEventListener('touchstart', (e) => {
                    startX = e.touches[0].pageX;
                    startY = e.touches[0].pageY;
                    endX = -1;
                    endY = -1;
                    e.preventDefault();
                });
                element.addEventListener('touchmove', (e) => {
                    endX = e.touches[0].pageX;
                    endY = e.touches[0].pageY;
                    e.preventDefault();
                });
                if(this.isVertical) {
                    element.addEventListener('touchend', (e) => {
                        let offset = startY - endY;
                        let absOffset = Math.abs(offset);
                        if (offset > 0 && absOffset > OFFSET_DISTANCE) {
                            this.next()
                        } else if (offset < 0 && absOffset > OFFSET_DISTANCE) {
                            this.prev();
                        }
                        e.preventDefault();
                    });
                } else {
                    element.addEventListener('touchend', (e) => {
                        let offset = startX - endX;
                        let absOffset = Math.abs(offset);
                        if (offset > 0 && absOffset > OFFSET_DISTANCE) {
                            this.next()
                        } else if (offset < 0 && absOffset > OFFSET_DISTANCE) {
                            this.prev();
                        }
                        e.preventDefault();
                    });
                }
            },
            calcSize(parent) {
                this.$nextTick(() => {
                    if(this.width) {
                        this.swipeWidth = this.width;
                    } else {
                        this.swipeWidth = parent.clientWidth;
                    }
                    if(this.height) {
                        this.swipeHeight = this.height;
                    } else {
                        this.swipeHeight = this.swipeWidth * 0.6;
                    }
                    this.pages.forEach((eachPage) => {
                        eachPage.style.width = this.swipeWidth + 'px';
                        eachPage.style.height = this.swipeHeight + 'px';
                    });
                });
            },
            getContainerWidth() {
                if(this.isVertical) {
                    return `width: ${this.swipeWidth}px;`
                } else {
                    return `width: ${this.swipeWidth * this.pages.length}px;`
                }
            },
            getContainerHeight() {
                if(this.isVertical) {
                    return `height: ${this.swipeHeight * this.pages.length}px;`
                } else {
                    return `height: ${this.swipeHeight}px;`
                }
            },
            getActivePageStyle() {
                if(this.isVertical) {
                    return `transform: translate3d(0px, ${this.activeIndex * this.swipeHeight * -1}px, 0px);`
                } else {
                    return `transform: translate3d(${this.activeIndex * this.swipeWidth * -1}px, 0px, 0px);`
                }
            }
        },
    }
</script>
<style lang="less" scoped>
    .swipe {
        position: relative;
        width: 100%;
        border-bottom: 1px solid #eaeaea;
        cursor: auto;
        overflow: hidden;
        background: #eaeaea;
        .swipe-container {
            height: 100%;
            background: white;
            overflow: hidden;
            text-align: center;
            transition: transform .2s ease-in-out;
            .swipe-page {
                display: inline-block;
                width: 100%;
                height: 100%;
                color: #999;
                text-align: center;
                float: left;
                font-size: 20px;
            }
        }
        .swipe-counter-container {
            position: absolute;
            bottom: 0;
            left: 50%;
            margin: 5px;
            font-size: 0;
            transform: translateX(-50%);
            .swipe-counter {
                display: inline-block;
                width: 16PX;
                height: 2PX;
                margin: 2PX;
                border: 2PX solid #eaeaea;
                border-radius: 1PX;
                background: #999;
                opacity: .5;
                transition: width .4s ease-in-out;
                &.is-active {
                    width: 24px;
                    border: 2PX solid #ccc;
                    background: white;
                }
            }
        }
        &.is-vertical {
            text-align: center;
            .swipe-counter-container {
                left: auto;
                right: 0;
                top: 50%;
                bottom: auto;
                width: 8PX;
                transform: translateY(-50%);
                .swipe-counter {
                    width: 2PX;
                    height: 16PX;
                    transition: height .4s ease-in-out;
                    &.is-active {
                        height: 24px;
                    }
                }
            }
        }
    }
</style>