<template>
    <div class="tabs">
        <div class="tab-nav w" :class="[{'is-scroll': isScroll}]" ref="nav">
            <i v-if="isScroll"
               class="scroll-icon scroll-left waves iconfont icon-icon_more_new"
               :class="{'disabled': scrollIndex === 0}"
                @click="scrollLeft"></i>
            <div class="tab-nav-content"
                 ref="navContent"
                 :style="getTransformStyle()">
                 <span class="tab" v-for="(tab, index) in tabs"
                       :class="{'is-active': keys[index] === activeKey}"
                       :key="keys[index]"
                       @click="activePane(index)">{{tab}}</span>
            </div>
            <i v-if="isScroll"
               class="scroll-icon scroll-right waves iconfont icon-icon_more_new"
                @click="scrollRight"></i>
        </div>
        <slot></slot>
    </div>
</template>
<script>
    export default {
        name: 'Tabs',
        data() {
            return {
                keys: [],
                tabs: [],
                activeKey: '',
                isScroll: false,
                scrollIndex: 0,
                scrollWidth: 0,
                maxScrollIndex: 0,
                navWidth: 0,
                childWidth: 0,
            }
        },
        props: {
            defaultActiveKey: String
        },
        mounted() {
            if (this.defaultActiveKey) {
                this.activeKey = this.defaultActiveKey;
            }
        },
        methods: {
            calcSize() {
                let navWidth = this.$refs.nav.clientWidth;
                let childWidth = 0;
                let childNodes = this.$refs.navContent.childNodes;
                if (childNodes) {
                    childNodes.forEach((eachNode) => {
                        if (eachNode.nodeType !== 8) {
                            childWidth += eachNode.clientWidth;
                        }
                    })
                }
                if (navWidth < childWidth) {
                    this.isScroll = true;
                    this.maxScrollIndex = Math.floor(childWidth / navWidth);
                }
                this.navWidth = navWidth;
                this.childWidth = childWidth;
            },
            addTab(key, tabName) {
                if (this.keys.indexOf(key) === -1) {
                    this.keys.push(key);
                    this.tabs.push(tabName);
                    if (this.keys.length === 1 && !this.defaultActiveKey) {
                        this.activeKey = key;
                    }
                    this.$nextTick(() => {
                        this.calcSize();
                    });
                }
            },
            activePane(index) {
                this.activeKey = this.keys[index];
            },
            getTransformStyle() {
                let offset = this.scrollIndex * 50;
                if(offset > this.navWidth) {
                    offset = this.navWidth;
                }
                return `transform: translate3d(-${offset}%, 0, 0)`;
            },
            scrollLeft() {
                if(this.scrollIndex > 0) {
                    this.scrollIndex--;
                }
            },
            scrollRight() {
                this.scrollIndex++;
                if(this.scrollIndex > this.maxScrollIndex) {
                    this.scrollIndex = this.maxScrollIndex;
                }
            }
        },
    }
</script>
<style lang="less" scoped>
    .tabs {
        .tab-nav {
            position: relative;
            display: block;
            width: 100%;
            border-bottom: 1PX solid #eaeaea;
            white-space: nowrap;
            &.is-scroll {
                padding: 0 30px;
            }
            .scroll-icon {
                position: absolute;
                bottom: 0;
                width: 30px;
                height: 40px;
                line-height: 40px;
                color: #ccc;
                text-align: center;
                background: white;
                z-index: 2;
                &.disabled {
                    cursor: not-allowed;
                }
                &.scroll-left {
                    left: 0;
                    transform: rotate(180deg);
                }
                &.scroll-right {
                    right: 0;
                }
            }
            .tab-nav-content {
                display: inline-block;
                height: 40px;
                line-height: 40px;
                overflow: hidden;
                font-size: 0;
                vertical-align: bottom;
                transition: transform .4s ease-in-out;
                .tab {
                    display: inline-block;
                    height: 40px;
                    line-height: 40px;
                    padding: 0 16px;
                    color: #999;
                    font-size: 16px;
                    text-align: center;
                    &.is-active {
                        color: #444;
                    }
                }
            }
        }
    }
</style>