<template>
    <div id="app">
        <Nav @leftClick="setNavDialog(true)"
             :leftIcon="['icon-icon_list']"
             :rightIcon="['icon-icon_search,icon-icon_wishlist,icon-icon_shoppingcart_a']"
        />
        <h3>基础</h3>
        <div>
            <EdLine label="Button" item="按钮" @click="setButtonPanel(true)"/>
            <EdLine label="Collapse" item="手风琴" @click="setAccordionPanel(true)"/>
            <EdLine label="Dialog" item="模态框" @click="setDialogPanel(true)"/>
            <EdLine label="Line" item="列表" @click="setLinePanel(true)"></EdLine>
            <EdLine label="Page" item="滑动窗口" @click="setPagePanel(true)"/>
            <EdLine label="Swipe" item="轮播器" @click="setSwipePanel(true)"/>
            <EdLine label="Tabs" item="页签" @click="setTabPanel(true)"/>
        </div>
        <h3>表单元素</h3>
        <div>
            <EdLine label="Input" item="输入行" @click="setInputPanel(true)"/>
            <EdLine label="Form" item="表单元素" @click="setFormPanel(true)"/>
        </div>
        <!--导航栏弹窗-->
        <Page :isFull="false"
              :isShow="isNavDialog"
              dir="ltr"
              @clickBg="setNavDialog(false)">
            <template slot="title">
                <h2>What's Elic</h2>
            </template>
            <template slot="content">
                <div class="elic-desc">
                    <p>一个简洁的组件库</p>
                    <p>包含最基本的组件</p>
                    <p>参考Material Design原则</p>
                    <p>采用黑白灰的色调</p>
                    <p>这就是它的全部，希望你喜欢</p>
                    <p style="text-align: right">-- by edeity</p>
                </div>
            </template>
            <template slot="foot">
                <EdLine label="See more on Github" @click="openLink('https://www.github.com/edeity', true)"></EdLine>
            </template>
        </Page>
        <!--按钮-->
        <Page :isShow="isButtonPanel" :isBuiltIn="true" title="按钮" @back="()=>{this.isButtonPanel = false;}">
            <h3>常规</h3>
            <Button isShadow>Shadow</Button>
            <Button>Simple</Button>
            <Button isBlock>Block</Button>
            <h3>按钮组</h3>
            <ButtonGroup>
                <Button>Group 1</Button>
                <Button>Group 2</Button>
                <Button>Group 3</Button>
            </ButtonGroup>
        </Page>
        <!--手风琴-->
        <Page :isShow="isAccordion" :isBuiltIn="true" title="手风琴" @back="setAccordionPanel(false)">
            <Accordion defaultActiveKey="accordion2">
                <AccordionPane title="Five score years ago" id="accordion1">
                    <p>Five score years ago, a great American, in whose symbolic shadow we stand today, signed the Emancipation Proclamation. This momentous decree came as a great beacon light of hope to millions of Negro slaves who had been seared in the flames of withering injustice. It came as a joyous daybreak to end the long night of bad captivity.</p>
                </AccordionPane>
                <AccordionPane title="But one hundred years later" id="accordion2">
                    <p>But one hundred years later, the Negro still is not free. One hundred years later, the life of the Negro is still sadly crippled by the manacles of segregation and the chains of discrimination. One hundred years later, the Negro lives on a lonely island of poverty in the midst of a vast ocean of material prosperity. One hundred years later, the Negro is still languished in the corners of American society and finds himself an exile in his own land. So we’ve come here today to dramatize a shameful condition.</p>
                </AccordionPane>
                <AccordionPane title="I am not unmindful that" id="accordion3">
                    <p>I am not unmindful that some of you have come here out of great trials and tribulations. Some of you have come fresh from narrow jail cells. Some of you have come from areas where your quest for freedom left you battered by the storms of persecution and staggered by the winds of police brutality. You have been the veterans of creative suffering. Continue to work with the faith that unearned suffering is redemptive.</p>
                </AccordionPane>
                <AccordionPane title="Go back to Mississippi" id="accordion4">
                    <p>Go back to Mississippi, go back to Alabama, go back to South Carolina, go back to Georgia, go back to Louisiana, go back to the slums and ghettos of our northern cities, knowing that somehow this situation can and will be changed. Let us not wallow in the valley of despair.
                        I say to you today, my friends, so even though we face the difficulties of today and tomorrow, I still have a dream. It is a dream deeply rooted in the American dream.</p>
                </AccordionPane>
            </Accordion>
        </Page>
        <!--弹窗层-->
        <Page :isShow="isDialogPanel" :isBuiltIn="true" title="模态框" @back="()=>{setDialogPanel(false)}">
            <EdLine label="Tips" item="tips" @click="openDialog('Say Hello !')"/>
            <EdLine label="alert" item="alert" @click="openDialog('Say Hello !', 'alert')"/>
            <EdLine label="Confirm" item="confirm" @click="openDialog('Are you sure ?', 'confirm')"/>
            <EdLine label="Toast" item="toast" @click="openDialog('Say Hello !', 'toast')"/>
            <EdLine label="Loading" item="loading" @click="openLoading"/>
        </Page>
        <!--列表-->
        <Page :isShow="isLinePanel" :isBuiltIn="true" title="列表" @back="()=>{setLinePanel(false)}">
            <EdLine label="列表" item="副标题" noRightIcon/>
            <EdLine label="列表" item="副标题"/>
            <EdLine label="列表" item="副标题" iconClass="icon-icon_note"/>
            <EdLine label="列表">
                <div slot="item">ITEM SLOT</div>
            </EdLine>
        </Page>
        <!--滑动层弹窗-->
        <Page :isShow="isPagePanel" :isBuiltIn="true" title="滑动窗口" @back="()=>{setPagePanel(false)}">
            <EdLine label="Page" item="Bottom To Top" @click="openPage('btt')"/>
            <EdLine label="Page" item="Right to Left" @click="openPage('rtl')"/>
            <EdLine label="Page" item="Left to Right" @click="openPage('ltr')"/>
            <EdLine label="Page" item="Fix" @click="openPage('btt', true)"/>
            <Page :isFull="false"
                  :isShow="isPage"
                  :isFit="isPageFit"
                  :dir="dialogDirection"
                  @clickBg="hidePage">
                <h2 slot="title">Hello Page</h2>
                <EdLine label="选项1"></EdLine>
                <EdLine label="选项2"></EdLine>
                <EdLine label="选项3"></EdLine>
            </Page>
        </Page>
        <!--图片滚动框弹窗-->
        <Page :isShow="isSwipePanel" :isBuiltIn="true" title="轮播器" @back="()=>{setSwipePanel(false)}">
            <h3>横向滚动</h3>
            <Swipe>
                <SwipeItem v-for="(item, index) in items" :key="index">
                    <div class="hor-text">Item Hor {{index}}</div>
                </SwipeItem>
            </Swipe>
            <h3>垂直滚动</h3>
            <Swipe :isVertical="true">
                <SwipeItem v-for="(item, index) in items" :key="index">
                    <div class="ver-text">Item Ver {{index}}</div>
                </SwipeItem>
            </Swipe>
            <ButtonGroup>
                <Button @click="addItem">addItem</Button>
                <Button @click="removeItem">removeItem</Button>
            </ButtonGroup>
        </Page>
        <!--输入框弹窗-->
        <Page :isShow="isInputPanel" :isBuiltIn="true" title="输入" @back="()=>{setInputPanel(false)}">
            <h3>有标签</h3>
            <Input label="用户名" tips="用户名"/>
            <Input label="密码" tips="密码" type="password"/>
            <h3>无标签</h3>
            <Input tips="用户名"/>
            <h3>浮动型</h3>
            <Input tips="用户名" :isFloat="true"/>
        </Page>
        <!--页签层-->
        <Page :isShow="isTabPanel" :isBuiltIn="true" title="页签" @back="()=>{setTabPanel(false)}">
            <Tabs>
                <TabPane tab="T1" id="1"><div class="tab-pane-demo">T1</div></TabPane>
                <TabPane tab="T2" id="2"><div class="tab-pane-demo">T2</div></TabPane>
                <TabPane tab="T3" id="3"><div class="tab-pane-demo">T3</div></TabPane>
            </Tabs>
            <Tabs>
                <TabPane tab="T1" id="1"><div class="tab-pane-demo">T1</div></TabPane>
                <TabPane tab="T2" id="2"><div class="tab-pane-demo">T2</div></TabPane>
                <TabPane tab="T3" id="3"><div class="tab-pane-demo">T3</div></TabPane>
                <TabPane tab="T4" id="4"><div class="tab-pane-demo">T4</div></TabPane>
                <TabPane tab="T5" id="5"><div class="tab-pane-demo">T5</div></TabPane>
                <TabPane tab="T6" id="6"><div class="tab-pane-demo">T6</div></TabPane>
                <TabPane tab="T7" id="7"><div class="tab-pane-demo">T7</div></TabPane>
                <TabPane tab="T8" id="8"><div class="tab-pane-demo">T8</div></TabPane>
            </Tabs>
        </Page>
        <!--表单层-->
        <Page :isShow="isFormPanel" :isBuiltIn="true" title="表单元素" @back="setFormPanel(false)">
            <EdLine label="开关">
                <EdSwitch slot="item"/>
            </EdLine>
            <EdLine label="开关【小】">
                <EdSwitch :isSmall="true" slot="item"/>
            </EdLine>
            <EdLine label="评分">
                <Rate slot="item" :rate="formRate"/>
            </EdLine>
            <EdLine label="选项">
                <Select slot="item" :defaultValue="formRate"
                        :options="[1, 2, 3, 4.5, 5]" @afterSelect="(value)=>{formRate=value}"/>
            </EdLine>
            <EdLine label="选择[Radio]">
                <Radio slot="item" label="Ratio"></Radio>
            </EdLine>
            <EdLine label="选择[组]">
                <RadioGroup slot="item" :isMulti="false">
                    <Radio label="c1"></Radio>
                    <Radio label="c2"></Radio>
                    <Radio label="c3"></Radio>
                </RadioGroup>
            </EdLine>
            <EdLine label="选择[Checkbox]">
                <Radio slot="item" label="Radio" type="checkbox"></Radio>
            </EdLine>
            <EdLine label="选择[单选]">
                <RadioGroup slot="item" :isMulti="false">
                    <Radio label="c1" type="checkbox"></Radio>
                    <Radio label="c2" type="checkbox"></Radio>
                    <Radio label="c3" type="checkbox"></Radio>
                </RadioGroup>
            </EdLine>
            <EdLine label="选择[多选]">
                <RadioGroup slot="item" :isMulti="true">
                    <Radio label="c1" type="checkbox"></Radio>
                    <Radio label="c2" type="checkbox"></Radio>
                    <Radio label="c3" type="checkbox"></Radio>
                </RadioGroup>
            </EdLine>
        </Page>
        <!--c:Dialog-->
        <Dialog :type="dialogType" :isShow="isDialog" :text="dialogText"
                @close="hideDialog" @yes="sayYes" @no="sayNo"/>
    </div>
</template>

<script>
    // 组件库
    import Icon from './components/NavIcon';
    import Nav from './components/Nav';
    import EdLine from './components/EdLine';
    import Input from './components/Input';
    import Page from './components/Page';
    import Swipe from './components/Swipe';
    import SwipeItem from './components/SwipeItem';
    import Button from './components/Button';
    import ButtonGroup from "./components/ButtonGroup";
    import Dialog from './components/Dialog';
    import Tabs from './components/Tabs';
    import TabPane from './components/TabPane';
    import EdSwitch from './components/EdSwitch';
    import Select from './components/Select';
    import Rate from './components/Rate';
    import Accordion from './components/Accordion';
    import AccordionPane from './components/AccordionPane';
    import Radio from './components/Radio';
    import RadioGroup from './components/RadioGroup';
    // 其他工具类
    import u from './components/libs/utils';

    export default {
        name: 'app',
        components: {
            Icon,
            Nav,
            EdLine,
            Input,
            Page,
            Swipe,
            SwipeItem,
            Button,
            ButtonGroup,
            Dialog,
            Tabs,
            TabPane,
            EdSwitch,
            Select,
            Rate,
            Accordion,
            AccordionPane,
            Radio,
            RadioGroup
        },
        data() {
            return {
                isButtonPanel: false, // 按钮
                isInputPanel: false, // 输入框
                isPagePanel: false, // 图层
                isLinePanel: false, // 列表
                isDialogPanel: false, // 模态框
                isSwipePanel: false, // 轮播图
                isTabPanel: false, // 页签
                isFormPanel: false, // 表单
                isAccordion: false, // 手风琴，

                isNavDialog: false,
                isPage: false,
                isPageFit: false,
                isDialogFull: false,
                dialogDirection: 'btt',
                items: [{}, {}, {}],
                isDialog: false,
                dialogType: false,
                dialogText: '',
                formRate: 3,
            }
        },
        methods: {
            openLink(url, isOpenNewTab) {
                if (isOpenNewTab === true) {
                    window.open(url)
                } else {
                    window.location.href = url;
                }
            },
            setNavDialog(isNavDialog) {
                this.isNavDialog = isNavDialog;
            },
            setButtonPanel(isButtonPanel) {
                this.isButtonPanel = isButtonPanel;
            },
            setInputPanel(isInputPanel) {
                this.isInputPanel = isInputPanel;
            },
            setPagePanel(isPagePanel) {
                this.isPagePanel = isPagePanel;
            },
            setDialogPanel(isDialogPanel) {
                this.isDialogPanel = isDialogPanel;
            },
            setLinePanel(isLinePanel) {
                this.isLinePanel = isLinePanel;
            },
            setSwipePanel(isSwipePanel) {
                this.isSwipePanel = isSwipePanel;
            },
            setTabPanel(isTabPanel) {
                this.isTabPanel = isTabPanel;
            },
            setFormPanel(isFormPanel) {
                this.isFormPanel = isFormPanel;
            },
            setAccordionPanel(isAccordion) {
                this.isAccordion = isAccordion;
            },
            hidePage() {
                this.isPage = false;
            },
            openPage(dir, isPageFit) {
                this.dialogDirection = dir;
                this.isPage = true;
                this.isPageFit = isPageFit;
            },
            addItem() {
                this.items.push({})
            },
            removeItem() {
                this.items.pop();
            },
            openDialog(dialogText, dialogType) {
                this.dialogText = dialogText;
                this.dialogType = dialogType;
                this.isDialog = true;
            },
            openLoading() {
                this.openDialog('Loading...', 'load');
                setTimeout(()=>{
                    this.isDialog = false
                }, 2000);
            },
            hideDialog() {
                this.isDialog = false;
            },
            sayYes() {
                u.log('yes!');
                this.isDialog = false;
            },
            sayNo() {
                u.log('no');
                this.isDialog = false;
            }
        }
    }
</script>

<style lang="less">
    body {
        padding: 0;
        margin: 0;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #444;
        h3 {
            height: 40px;
            line-height: 40px;
            margin: 0;
            padding: 0 15px;
            background: #f5f5f5;
            font-size: 14px;
            font-weight: normal;
        }
        .split {
            display: block;
            background: #eaeaea;
            height: 1PX;
        }
        p {
            padding: 0 15px;
        }
        * {
            box-sizing: border-box;
        }
        .hor-text {
            margin-top: 25%;
        }
        .ver-text {
            margin-top: 25%;
        }
        .foot .line:first-child {
            .label-container {
                border-top: 1PX solid #eaeaea;
                border-bottom: 0;
            }
        }
        .tab-pane-demo {
            padding: 20px;
            text-align: center;
            font-size: 30px;
            background: #f1f1f1;
            color: #888;
        }
        .elic-desc {
            color: #666;
        }
    }
</style>
