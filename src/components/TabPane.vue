<template>
    <transition name="fade">
        <div class="tab-pane" v-if="$parent.activeKey === id">
            <slot></slot>
        </div>
    </transition>
</template>
<script>
    export default {
        name: 'TabPane',
        props: {
            tab: {
                default: 'String',
            },
            id: {
                default: 'String',
                required: true
            }
        },
        mounted() {
            this.$parent.addTab(this.id, this.tab);
        }
    }
</script>
<style lang="less" scoped>
    .tab-pane {
        position: relative;
        left: 0;
        top: 0;
        width: 100%;
    }
    .fade-enter-active, .fade-leave-active {
        transition: left .5s;
    }


    .fade-enter {
        position: absolute;
        top: 41px;
        left: 100%;
    }
    .fade-leave-to {
        position: absolute;
        top: 41px;
        left: -100%;
    }
</style>