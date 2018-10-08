<template>
    <div class="tab-pane" v-if="$parent.activeKey === id">
        <slot></slot>
    </div>
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

    }
</style>