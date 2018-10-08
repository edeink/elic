<template>
    <div class="btn-group">
        <slot></slot>
    </div>
</template>
<script>
    export default {
        name: 'ButtonGroup'
    }
</script>
<style lang="less" scoped>
    .btn-group {
        margin-top: 8px;
        margin-bottom: 8px;
        text-align: center;
        button {
            display: inline-block;
            width: auto;
            margin: 0 0 0 -1px;
            border: 1px solid #d9d9d9;
            border-radius: 0;
            &:first-child {
                margin-left: 0;
                border-top-left-radius: 5px;
                border-bottom-left-radius: 5px;
            }
            &:last-child {
                border-top-right-radius: 5px;
                border-bottom-right-radius: 5px;
            }
        }
    }
</style>