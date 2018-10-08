<template>
    <div class="line" @click.prevent="lineClick">
        <div class="label-container">
            <span class="label-title">
                <i v-if="iconClass" class="iconfont" :class="iconClass"></i>
                {{ label }}
            </span>
        </div>
        <div class="item-container" v-if="item">
            <span class="item-title" :class="{'no-icon' : noRightIcon}">{{ item }}</span>
            <i v-if="!noRightIcon" class="item-icon iconfont icon-icon_more_new"/>
        </div>
        <div class="item-container no-item" v-else>
            <slot name="item"></slot>
        </div>
    </div>
</template>
<script>
    export default{
        name: 'EdLine',
        props: {
            label: {
                type: String,
            },
            item: {
                type: String
            },
            noRightIcon: {
                type: Boolean,
            },
            iconClass: {
                type: String
            }
        },
        methods: {
            lineClick() {
                this.$emit('click');
            }
        }
    }
</script>
<style lang="less" scoped>
    @lineHeight: 40px;
    .line {
        position: relative;
        width: 100%;
        height: @lineHeight;
        line-height: @lineHeight;
        padding-left: 15px;
        font-size: 14px;
        /*&:last-child {*/
            /*.label-container {*/
                /*border-bottom: none;*/
            /*}*/
        /*}*/
        .label-container {
            height: @lineHeight;
            line-height: @lineHeight;
            border-bottom: 1px solid #eaeaea;
            color: #444;
        }
        .item-container {
            position: absolute;
            top: 0;
            right: 0;
            height: @lineHeight;
            line-height: @lineHeight;
            &.no-item {
                padding-right: 14px;
            }
            .item-title {
                color: #aaa;
                vertical-align: top;
                font-size: 12px;
                &.no-icon {
                    margin-right: 20px;
                }
            }
            .item-icon {
                display: inline-block;
                width: 30px;
                color: #ddd;
                text-align: center;
            }
        }
    }
</style>
