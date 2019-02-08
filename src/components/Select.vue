<template>
    <div class="select">
        <input class="select-value"
               ref="input"
               @blur="toggleSelectPanel(false)"
               @focus="toggleSelectPanel(true)" :value="selectData "/>
        <i class="select-btn iconfont icon-icon_down"
           @click.prevent="toggleFocusSelectPanel"
           :class="{down: isSelect}"></i>
        <ul class="select-panel"
            :class="{down: isSelect}"
            :style="`height: ${isSelect ? downHeight : 0}px`"
            ref="selectPanel">
            <li v-for="(option, index) in options"
                class="select-option"
                :class="{'is-active': selectData === option}"
                :key="index"
                @click="afterSelect(option)">{{option.value ? option.value : option}}
            </li>
        </ul>
    </div>
</template>
<script>
    import Icon from './NavIcon';

    export default {
        name: 'Select',
        components: {
            Icon
        },
        data() {
            return {
                selectData: '',
                isSelect: false,
                downHeight: 0
            }
        },
        props: {
            options: {
                Type: Array
            },
            defaultValue: {}
        },
        created() {
            this.selectData = this.defaultValue;
        },
        mounted() {
            let childNodes = this.$refs.selectPanel.childNodes;
            if (childNodes) {
                let downHeight = 0;
                childNodes.forEach((eachNode) => {
                    downHeight += eachNode.clientHeight;
                });
                this.downHeight = downHeight;
            }
        },
        methods: {
            toggleFocusSelectPanel() {
                if(this.isSelect) {
                    this.$refs.input.blur();
                } else {
                    this.$refs.input.focus();
                }

            },
            toggleSelectPanel(isSelect) {
                this.isSelect = isSelect;
            },
            afterSelect(value) {
                this.selectData = value;
                this.isSelect = false;
                this.$emit('afterSelect', value);
            }
        }
    }
</script>
<style lang="less" scoped>
    .select {
        position: relative;
        display: inline-block;
        min-width: 100px;
        height: 30px;
        line-height: 30px;
        border-bottom: 1px solid #eaeaea;
        vertical-align: middle;
        .select-value {
            display: block;
            width: 100%;
            height: 100%;
            padding-left: 10px;
            border: none;
            outline: none;
        }
        .select-btn {
            position: absolute;
            right: 0;
            top: 0px;
            width: 20px;
            color: #aaa;
            font-size: 12px;
            text-align: center;
            transition: transform .3s ease-in-out;
            &.down {
                transform: rotate(180deg);
            }
        }
        .select-panel {
            position: absolute;
            top: 29px;
            left: 0;
            display: block;
            width: 100%;
            height: 0;
            margin: 0;
            padding-left: 0;
            background: white;
            border: 1px solid #eaeaea;
            border-top: none;
            list-style: none;
            transition: height .3s ease-in-out;
            overflow: hidden;
            z-index: 10;
            &.down {
                /*min-height: 120px;*/
                max-height: 400px;
                border-top: 1px solid #eaeaea;
                overflow: auto;
                /*box-shadow: 0 2px 4px rgba(0,0,0,0.2);*/
                box-shadow: 0 2px 6px rgba(0, 0, 0, .25), 0 1px 1px rgba(0, 0, 0, .15);
            }
            .select-option {
                display: block;
                padding-left: 8px;
                color: #888;
                &.is-active {
                    color: #444;
                    background: #eee;
                }
            }
        }
    }
</style>