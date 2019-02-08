<template>
    <nav>
        <div class="placeholder-nav"></div>
        <div class="real-nav">
            <div class="left-icon-container" v-if="leftIcon">
                <Icon :iconClass="leftIcon" @click="leftClick"/>
            </div>
            <div class="title-container">
                <h1>Elic</h1>
            </div>
            <div class="right-icon-container" v-if="rightIconArray">
                <ul class="nav-icon-list">
                    <li v-for="(eachIcon, index) in rightIconArray" :key="index">
                        <Icon :iconClass="eachIcon"/>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    import Icon from './NavIcon';
    export default {
        name: 'Nav',
        components: {
            Icon
        },
        props: {
            leftIcon: {
                type: String
            },
            rightIconArray: {
                type: Array
            }
        },
        methods: {
            leftClick() {
                this.$emit('leftClick');
            }
        }
    }
</script>

<style lang="less" scoped>
    .placeholder-nav {
        width: 100%;
        height: 45px;
    }
    .real-nav {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 45px;
        line-height: 45px;
        background: white;
        font-size: 0;
        box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        z-index: 2;
        .nav-icon {
            display: inline-block;
            width: 45px;
            height: 45px;
            text-align: center;
            font-size: 20px;
        }
        .left-icon-container {
            display: inline-block;
            height: 100%;
            margin-right: 10px;
            vertical-align: top;
            text-align: center;
        }
        .title-container {
            display: inline-block;
            height: 100%;
            font-size: 14px;
            vertical-align: top;
            text-align: center;
            h1 {
                margin: 0;
            }
        }
        .right-icon-container {
            display: inline-block;
            float: right;
            vertical-align: top;
            .nav-icon-list {
                height: 45px;
                line-height: 45px;
                padding-left: 0;
                li {
                   height: 100%;
                    display: inline-block;
                }
            }
        }
    }
</style>
