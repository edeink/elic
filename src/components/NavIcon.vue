<template>
    <i @click="iconClick" class="iconfont waves" :class="iconClass"></i>
</template>
<script>
    import './css/waves.less';
    export default {
        name: 'Icon',
        props: {
            iconClass: {
                type: String
            }
        },
        methods: {
            iconClick() {
                this.$emit('click')
            }
        }
    }
</script>
<style lang="less" scoped>
    .iconfont {
        display: inline-block;
        width: 45px;
        height: 45px;
        text-align: center;
        font-size: 20px;
    }
</style>
