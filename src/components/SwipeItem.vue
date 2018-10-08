<template>
    <div class="swipe-page">
        <slot></slot>
    </div>
</template>
<script>
    export default {
        name: 'SwipeItem',
        data() {
            return {
                width: 0,
            }
        },
        mounted() {
            this.$parent.updated();
        },
        destroyed() {
            this.$parent.updated();
        }
    }
</script>