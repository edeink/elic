<template>
    <div class="dialog" v-if="isRealShow">
        <div class="dialog-bg" v-if="!isSimple"></div>
        <div class="dialog-content animated"
             :class="[type,
             {fadeIn: isIn && isSimple, fadeOut: isOut && isSimple},
             {bounceIn: isIn && !isSimple, bounceOut: isOut && !isSimple}]">
            <i class="dialog-close-btn iconfont icon-icon_close_"
               @click="close" v-if="!isSimple"></i>
            <!--loading-->
            <div class="spinner" v-if="type === 'load'">
                <div class="double-bounce1"></div>
                <div class="double-bounce2"></div>
            </div>
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
        computed: {
            isSimple() {
                return this.type === 'toast' || this.type === 'load';
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
            min-width: 160px;
            color: white;
            background-color: #444;
            padding: 8px;
            text-align: center;
        }
        &.alert {
            padding: 24px 24px 16px;
        }
        &.confirm {
            padding: 24px 24px 16px;
        }
        &.load {
            min-width: auto;
            padding: 8px;
            background: transparent;
            text-align: center;
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
<style>
    .spinner {
        width: 40px;
        height: 40px;
        margin: 0 auto 8px;
        position: relative;
    }

    .double-bounce1, .double-bounce2 {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #999;
        opacity: 0.6;
        position: absolute;
        top: 0;
        left: 0;

        -webkit-animation: sk-bounce 2.0s infinite ease-in-out;
        animation: sk-bounce 2.0s infinite ease-in-out;
    }

    .double-bounce2 {
        -webkit-animation-delay: -1.0s;
        animation-delay: -1.0s;
    }

    @-webkit-keyframes sk-bounce {
        0%, 100% { -webkit-transform: scale(0.0) }
        50% { -webkit-transform: scale(1.0) }
    }

    @keyframes sk-bounce {
        0%, 100% {
            transform: scale(0.0);
            -webkit-transform: scale(0.0);
        } 50% {
              transform: scale(1.0);
              -webkit-transform: scale(1.0);
          }
    }
</style>