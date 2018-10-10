<template>
    <div class="radio_container" :class="{'is-active': checked}" @click.prevent="toggle">
        <span class="box" :class="{
                radio: type === 'radio',
                check: type === 'checkbox',
                'is-active': checked
            }">
            <span class="radio-active" v-if="checked && type === 'radio'"></span>
        </span>
        <span v-if="label" class="box-label" :class="{'is-active': checked}">{{ label }}</span>
    </div>
</template>
<script>
    export default {
        name: 'Radio',
        data() {
            return {
                model: '',
                _radioGroup: null,
            }
        },
        props: {
            label: String,
            type: {
                default: 'radio',
                validator: function (value) {
                    return ['radio', 'checkbox'].indexOf(value) !== -1;
                }
            }
        },
        computed: {
            checked() {
                if(this.isGroup) {
                    if(this._radioGroup.isMulti) {
                        return this._radioGroup.value.indexOf(this.label) !== -1;
                    } else {
                        return this._radioGroup.value === this.label;
                    }
                } else {
                    return this.model === this.label;
                }
            },
            isGroup() {
                let parent = this.$parent;
                while (parent) {
                    if (parent.$options.name !== 'RadioGroup') {
                        parent = parent.$parent;
                    } else {
                        this._radioGroup = parent;
                        return true;
                    }
                }
                return false;
            },
        },
        methods: {
            toggle() {
                if(this.isGroup) {
                    let groupValue = this._radioGroup.value;
                    if(this._radioGroup.isMulti) {
                        let index = groupValue.indexOf(this.label);
                        if(index !== -1) {
                            this._radioGroup.value.splice(index, 1);
                        } else {
                            this._radioGroup.value.push(this.label);
                        }
                    } else {
                        if(groupValue === this.label && this.type === 'checkbox') {
                            this._radioGroup.value = ''
                        } else {
                            this._radioGroup.value = this.label;
                        }
                    }
                } else {
                    if(this.model === this.label && this.type === 'checkbox') {
                        this.model = ''
                    } else {
                        this.model = this.label;
                    }
                }

            },
        }
    }
</script>
<style lang="less" scoped>
    .radio_container {
        display: inline-block;
        cursor: pointer;
        padding: 0 8px;
        -webkit-tap-highlight-color: transparent;
        .box {
            display: inline-block;
            position: relative;
            width: 14px;
            height: 14px;
            border: 1px solid #dcdfe6;
            border-radius: 2px;
            background-color: #fff;
            vertical-align: middle;
            transition: border-color .2s ease-in-out, background-color .2s ease-in-out;
            &.check {
                &:after {
                    content: "";
                    display: block;
                    position: absolute;
                    top: 1px;
                    left: 4px;
                    width: 3px;
                    height: 7px;
                    border: 1px solid #fff;
                    border-left: 0;
                    border-top: 0;
                    transform: rotate(45deg) scaleY(0);
                    transition: transform .15s ease-in .05s;
                    transform-origin: center;
                }
                &.is-active {
                    background-color: #666;
                    border: 1px solid #666;
                    &:after {
                        transform: rotate(45deg);
                    }
                }
            }
            &.radio {
                border-radius: 100%;
                &.is-active {
                    background-color: #666;
                    border: 1px solid #666;
                }
                .radio-active {
                    position: absolute;
                    display: inline-block;
                    left: 4px;
                    top: 4px;
                    width: 4px;
                    height: 4px;
                    background-color: white;
                    border-radius: 100%;
                }
            }
        }
        .box-label {
            margin-left: 8px;
            vertical-align: middle;
            color: #999;
            transition: color .2s ease-in-out;
            &.is-active {
                color: #333;
            }
        }
    }
</style>