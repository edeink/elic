<template>
    <div class="accordion-pane">
        <div class="pane-title">{{title}}
            <a class="icon-btn"
               :class="{'is-active': isActive}"
               @click="toggle(id)">
                <i class="pane-icon iconfont icon-icon_add_new"></i>
            </a>
        </div>
        <div class="pane-container"
             :class="{'is-active': isActive}"
             :style="`height: ${isActive ? contentHeight : 0}px`">
            <div class="pane-content" ref="content">
                <slot></slot>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'AccordionPane',
        data() {
            return {
                contentHeight: '',
            }
        },
        props: {
            id: {
                type: String,
                required: true
            },
            title: {
                type: String
            }
        },
        computed: {
            isActive() {
                return this.$parent.activeKey === this.id && this.$parent.isOn;
            }
        },
        mounted() {
            this.$parent.addPane(this.id);
        },
        beforeUpdate() {
            let contentHeight = 0;
            let $content = this.$refs.content;
            contentHeight = $content.offsetHeight;
            this.contentHeight = contentHeight;
        },
        methods: {
            toggle(id) {
                this.$parent.toggle(id);
            }
        }
    }
</script>
<style lang="less" scoped>
    .accordion-pane {
        .pane-title {
            height: 40px;
            line-height: 40px;
            margin: 0 16px;
            border-bottom: 1PX solid #eaeaea;
            .icon-btn {
                width: 40px;
                height: 40px;
                line-height: 40px;
                float: right;
                color: #444;
                text-align: center;
                transform-origin: center;
                .pane-icon {
                    display: inline-block;
                    width: 40px;
                    height: 40px;
                    transition: transform .3s ease-in-out;
                }
                &.is-active {
                    .pane-icon {
                        transform: rotate(45deg);
                    }
                }
            }
        }
        .pane-container {
            overflow: hidden;
            transition: height .3s ease-in-out;
            .pane-content {
                float: left;
                max-height: 500px;
                overflow: auto;
            }
        }
    }
</style>