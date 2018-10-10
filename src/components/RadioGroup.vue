<template>
    <div class="radio-group">
        <slot></slot>
    </div>
</template>
<script>
    export default {
        name: 'RadioGroup',
        data() {
            return {
                value: null
            }
        },
        props: {
            isMulti: {
                type: Boolean,
                default: true
            }
        },
        created() {
            if(this.isMulti) {
                this.value = [];
            } else {
                this.value = '';
            }
        }
    }
</script>
<style lang="less" scoped>

</style>