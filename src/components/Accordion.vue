<template>
    <div class="accordion">
        <slot></slot>
    </div>
</template>
<script>
    export default {
        name: 'Accordion',
        data() {
            return {
                keys: [],
                isOn: false,
                activeKey: 0,
            }
        },
        props: {
            defaultActiveKey: String
        },
        mounted() {
            if (this.defaultActiveKey) {
                this.activeKey = this.defaultActiveKey;
                this.isOn = true;
            }
        },
        methods: {
            addPane(key) {
                if (this.keys.indexOf(key) === -1) {
                    this.keys.push(key);
                }
            },
            toggle(key) {
                if(this.activeKey === key) {
                    this.isOn = !this.isOn;
                } else {
                    this.activeKey = key;
                    this.isOn = true;
                }
            }
        }
    }
</script>
<style lang="less" scoped>

</style>