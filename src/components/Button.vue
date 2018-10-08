<template>
    <button class="waves" :class="{block: isBlock, shadow: isShadow, pressed: isPressed}" @click="click">
        <slot></slot>
    </button>
</template>
<script>
    export default {
        name: 'Button',
        data() {
            return {
                isPressed: false,
            }
        },
        props: {
            isBlock: Boolean,
            isShadow: Boolean,
            type: String,
        },
        methods: {
            click() {
                this.isPressed = true;
                setTimeout(() => {
                    this.isPressed = false;
                }, 250);
                if(this.href) {
                    window.location.href = this.href;
                } else {
                    this.$emit('click');
                }
            }
        }
    }
</script>
<style lang="less" scoped>
    button {
        display: inline-block;
        line-height: 36px;
        margin: 8px;
        padding: 0 16px;
        border: none;
        border-radius: 2px;
        color: rgba(0, 0, 0, 0.65);
        background-color: white;
        cursor: pointer;
        font-size: 16px;
        outline-style: none;
        text-align: center;
        text-overflow: ellipsis;
        -webkit-tap-highlight-color: transparent;
        transition: background-color .2s ease-in-out;
        &.block {
            display: block;
            border: 1px solid #d9d9d9;
            width: calc(~'100% - 16px');
        }
        &.pressed {
            background-color: #eee;
        }
        &.shadow {
            box-shadow: 0 1px 3px 0 rgba(0,0,0,.26);
            &.pressed {
                background-color: white;
            }
        }
        &:active {
           background-color: #eee;
        }
    }
</style>