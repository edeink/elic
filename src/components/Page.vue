<template>
    <div class="eli-page" v-if="isRealShow" ref="page">
        <div class="page-bg" @click.prevent="clickBg" :class="{'is-active': isActive}"></div>
        <div class="page-container"
             ref="container"
             :class="[dir,{'is-active': isActive}, {'is-full': isFull}, {'is-fit': isFit}]">
            <div v-if="hasHeader"
                 class="title"
                 ref="title">
                <h2 class="has-icon" v-if="isBuiltIn">
                    <Icon iconClass="icon-icon_back_new" @click="back"/>
                    <span>{{title}}</span>
                </h2>
                <slot v-else name="title"></slot>
            </div>
            <div class="content" ref="content">
                <slot name="content"></slot>
                <slot></slot>
            </div>
            <div v-if="hasFooter" class="foot" ref="foot">
                <slot name="foot"></slot>
            </div>
        </div>
    </div>
</template>

<script>
    import Icon from '../components/NavIcon';
    export default {
        name: 'Page',
        components: {
            Icon
        },
        data() {
            return {
                parentContainer: null,
                lockScroll: 0,
                isBuiltInShow: false,
                isRealShow: false,
                isActive: false,
                hasHeader: true,
                hasFooter: true,
            }
        },
        props: {
            isShow: {
                type: Boolean,
                default: false
            },
            isFit: {
                type: Boolean
            },
            isFull: {
                type: Boolean,
                default: true
            },
            dir: {
                type: String,
                default: 'rtl'
            },
            isBuiltIn: {
                type: Boolean,
                default: false
            },
            title: {
                type: String
            }
        },
        watch: {
            isShow(value) {
                this.animateState(value);
                if(value === true) {
                    this.noScroll();
                } else {
                    this.enableScroll();
                }
            },
        },
        mounted() {
            window.addEventListener('resize', () => {
                this.calcContent();
            });
        },
        methods: {
            clickBg() {
                this.$emit('clickBg');
            },
            back() {
                if(this.isBuiltIn) {
                    this.$emit('back');
                }
            },
            calcContent() {
                let titleHeight = 0, containerHeight = 0, footHeight = 0;
                if(this.$refs.container) {
                    containerHeight = this.$refs.container.clientHeight;
                    if (this.$slots.title) {
                        if (this.$refs.title) {
                            titleHeight = this.$refs.title.clientHeight;
                            this.hasHeader = true;
                        } else {
                            this.hasHeader = false;
                        }

                    }
                    if (this.$slots.foot) {
                        if (this.$refs.foot) {
                            footHeight = this.$refs.foot.clientHeight;
                            this.hasFooter = true;
                        } else {
                            this.hasFooter = false;
                        }
                    }
                    this.$refs.content.style.height = (containerHeight - titleHeight - footHeight) + 'px';
                }
            },
            animateState(value) {
                if (value === true) {
                    this.isRealShow = true;
                    setTimeout(() => {
                        this.isActive = true;
                        this.calcContent();
                    }, 50);
                } else {
                    this.isActive = false;
                    setTimeout(() => {
                        this.isRealShow = false;
                    }, 300);
                }
            },
            noScroll() {
                this.$nextTick(() => {
                    let parent = this.parentContainer || this.findParenContainer();
                    if(parent.style.position !== 'fixed') {
                        let scrollY = window.scrollY;
                        let pStyle = parent.style;
                        if(pStyle) {
                            pStyle.position = 'fixed';
                            pStyle.top = `-${scrollY}px`;
                            pStyle.left = '0px';
                            pStyle.right = '0px';
                            pStyle.bottom = '0px';
                        }
                        this.lockScroll = scrollY;
                    }
                });
            },
            enableScroll() {
                if(this.parentContainer) {
                    this.parentContainer.style.position = 'static';
                }
                this.lockScroll = 0;
            },
            findParenContainer() {
                let parent = this.$refs.page.parentElement;
                let body = document.body;
                while(parent !== body && parent.className.indexOf('eli-page') === -1) {
                    parent = parent.parentElement;
                }
                this.parentContainer = parent;
                return parent;
            }
        }
    }
</script>
<style lang="less" scoped>
    .eli-page {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        z-index: 10;
    }
    .page-bg {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        background: #000;
        opacity: 0;
        z-index: 10;
        transition: opacity .2s ease-in-out;
        &.is-active {
            opacity: .3;
        }
    }
    h2 {
        height: 45px;
        line-height: 45px;
        margin: 0;
        padding: 0 15px;
        border-bottom: 1px solid #eaeaea;
        background: white;
        font-size: 18px;
        font-weight: normal;
        &.has-icon {
            padding: 0;
            .iconfont:first-child {
                margin-right: 10px;
            }
        }
        span {
            display: inline-block;
            height: 45px;
            line-height: 45px;
            vertical-align: top;
        }
    }
    .page-container {
        position: fixed;
        background: #fff;
        z-index: 11;
        overflow: hidden;
        /*bottom to top*/
        &.btt {
            left: 0;
            bottom: 0;
            width: 100%;
            height: 0;
            box-shadow: 0 0 10px #999;
            transition: height .3s cubic-bezier(.4,0,0,1), transform .3s cubic-bezier(.4,0,0,1);
            transform: translateY(100%);
            &.is-active {
                height: 75vh;
                transform: translateY(0);
                &.is-full {
                    height: 100vh;
                }
                &.is-fit {
                    height: auto;
                }
            }
        }
        /*left to right*/
        &.ltr {
            top: 0;
            left: -75vw;
            width: 75vw;
            height: 100vh;
            box-shadow: 0 0 10px #999;
            transition: left .3s cubic-bezier(.4,0,0,1);
            &.is-full {
                width: 100vw;
            }
            &.is-active {
                left: 0;
            }
        }
        /*right to left*/
        &.rtl {
            top: 0;
            right: 0;
            width: 75vw;
            height: 100vh;
            box-shadow: 0 0 10px #999;
            transform: translate3d(100%, 0, 0);
            transition: transform .3s cubic-bezier(.4,0,0,1);
            &.is-full {
                width: 100vw;
            }
            &.is-active {
                transform: translate3d(0, 0, 0);
            }
        }
        .content {
            overflow: auto;
        }
    }
</style>