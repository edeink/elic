<template>
    <div class="dialog" v-if="isRealShow">
        <div class="dialog-bg" v-if="type !== 'toast'"></div>
        <div class="dialog-content animated"
             :class="[type,
             {fadeIn: isIn && type === 'toast', fadeOut: isOut && type === 'toast'},
             {bounceIn: isIn && type !== 'toast', bounceOut: isOut && type !== 'toast'}]">
            <i class="dialog-close-btn iconfont icon-icon_close_"
               @click="close" v-if="type!=='toast'"></i>
            <span class="dialog-text">{{ text }}</span>
            <div class="answer" v-if="type==='alert'">
                <a class="yes" @click="yes">OK</a>
            </div>
            <div class="answer" v-if="type==='confirm'">
                <a class="no" @click="no">no</a>
                <a class="yes" @click="yes">Yes</a>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'Dialog',
        data() {
            return {
                isRealShow: false,
                isIn: false,
                isOut: false,
            }
        },
        props: {
            isShow: {
                Type: Boolean
            },
            text: {
                Type: String
            },
            type: {
                Type: String
            }
        },
        watch: {
            isShow(value) {
                if(value === true) {
                    this.isRealShow = true;
                    this.isIn = true;
                    setTimeout(() => {
                        this.isIn = false;
                    }, 500);
                    if(this.type === 'toast') {
                        setTimeout(() => {
                            this.$emit('close');
                        }, 1500);
                    }
                } else {
                    this.isOut = true;
                    setTimeout(() => {
                        this.isOut = false;
                        this.isRealShow = false;
                    }, 500);
                }
            }
        },
        methods: {
            close() {
                this.$emit('close');
            },
            no() {
                this.$emit('no');
            },
            yes() {
                this.$emit('yes');
            }
        }
    }
</script>
<style lang="less" scoped>
    .dialog {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        justify-content: center;
        align-items: center;
        display: flex;
        z-index: 20;
    }
    .dialog-bg {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        background: #000;
        opacity: .3;
        z-index: 20;
    }

    .dialog-content {
        position: relative;
        display: inline-block;
        min-width: 200px;
        padding: 24px;
        background: white;
        border-radius: 5px;
        z-index: 21;
        &.toast {
            color: white;
            background-color: #444;
            padding: 16px;
            text-align: center;
        }
        &.alert {
            padding: 24px 24px 16px;
        }
        &.confirm {
            padding: 24px 24px 16px;
        }
        .dialog-close-btn {
            position: absolute;
            display: inline-block;
            top: 2px;
            right: 2px;
            padding: 6px;
            font-size: 12px;
        }
        .answer {
            display: block;
            width: 100%;
            text-align: right;
            margin-top: 16px;
            .no {
                margin-right: 16px;
            }
            .yes {
                font-weight: bold;
            }
        }
    }
</style>
<style>
    .animated {
        animation-duration: .4s;
        animation-fill-mode: both;
    }

    .bounceIn {
        animation-name: bounceIn;
    }

    .bounceOut {
        animation-name: bounceOut;
    }

    .fadeIn {
        animation-name: fadeIn;
    }

    .fadeOut {
        animation-name: fadeOut;
    }

    @keyframes bounceIn {
        0%, 20%, 40%, 60%, 80%, to {
            animation-timing-function: cubic-bezier(.215, .61, .355, 1)
        }

        0% {
            opacity: 0;
            transform: scale3d(.3, .3, .3)
        }

        20% {
            transform: scale3d(1.1, 1.1, 1.1)
        }

        40% {
            transform: scale3d(.9, .9, .9)
        }

        60% {
            opacity: 1;
            transform: scale3d(1.03, 1.03, 1.03)
        }

        80% {
            transform: scale3d(.97, .97, .97)
        }

        to {
            opacity: 1;
            transform: scaleX(1)
        }
    }

    @keyframes bounceOut {
        20% {
            transform: scale3d(.9, .9, .9)
        }

        50%, 55% {
            opacity: 1;
            transform: scale3d(1.1, 1.1, 1.1)
        }

        to {
            opacity: 0;
            transform: scale3d(.3, .3, .3)
        }
    }

    @-webkit-keyframes fadeIn {
        from {
            opacity: 0;
        }

        to {
            opacity: 1;
        }
    }

    @keyframes fadeOut {
        from {
            opacity: 1;
        }

        to {
            opacity: 0;
        }
    }
</style>