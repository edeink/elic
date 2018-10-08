<template>
    <div class="checkbox" :class="{on: value === true, 'is-small': isSmall}" @click="toggle">
        <!--<input type="checkbox" value="true" checked="checked"/>-->
        <div class="handle" :class="{on: value === true}" ></div>
    </div>
</template>
<script>
    export default {
        name: 'EdSwitch',
        data() {
            return {
                value: false
            }
        },
        props: {
            defaultValue: {
                Type: Boolean
            },
            isSmall: {
                Type: Boolean
            }
        },
        mounted() {
            this.value = this.defaultValue;
        },
        methods: {
            toggle() {
                this.value = !this.value;
                this.$emit('toggle', this.value);
            }
        }
    }
</script>
<style lang="less" scoped>
    .checkbox {
        position: relative;
        display: inline-block;
        width: 60px;
        height: 30px;
        border: 1PX solid #eaeaea;
        border-radius: 20px;
        background: white;
        transition: background .3s ease-in-out;
        vertical-align: middle;
        &.is-small {
            width: 58px;
            height: 20px;
            .handle {
                top: -5px;
                &.on {
                    left: 30px;
                    background-color: #444;
                }
            }
            &.on {
                border-color: #999;
                background-color: #999;
            }
        }
        &.on {
            border-color: #444;
            background: #444;
        }
        .handle {
            position: absolute;
            left: 0;
            width: 28px;
            height: 28px;
            border-radius: 15px;
            /*border: 1px solid #eaeaea;*/
            background: white;
            box-shadow: 0 2px 7px rgba(0,0,0,.35), 0 1px 1px rgba(0,0,0,.15);
            transition: left .3s ease-in-out, background-color .3s ease-in-out;
            &.on {
                left: 30px;
            }
        }
    }
</style>