<template>
    <div class="rate">
        <i class="rate-icon iconfont icon-icon_review_star" ref="bgRate"></i>
        <i class="rate-icon real-rate iconfont icon-icon_review_star"
           :style="`width: ${rateWidth / 5 * rate }px`"
           ref="realRate"></i>
    </div>
</template>
<script>
    export default {
        name: 'Rate',
        data() {
            return {
                rateWidth: 0
            }
        },
        props: {
            rate: 0,
        },
        mounted() {
            this.$nextTick(() => {
                this.rateWidth = this.$refs.bgRate.clientWidth;
            });
        },
        methods: {
            afterSelect(value) {
                this.$emit('afterRate', value);
            }
        }
    }
</script>
<style lang="less" scoped>
    .rate {
        position: relative;
        .rate-icon {
            display: inline-block;
            font-size: 20px;
            color: #eee;
            &.real-rate {
                position: absolute;
                left: 0;
                color: #444;
                overflow: hidden;
                transition: width .3s ease-in-out   ;
            }
        }
    }
</style>