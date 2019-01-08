<template>
    <div class="container">
        <list class="list" :style="{height: page_height}">
            <cell style="height: 120px;background-color: red">
                <text class="text">这是头部</text>
            </cell>
            <header class="table_box">
                <div class="table_left">
                    <div class="table_bd">
                        <div class="table_th header">
                            <div class="table_td fixed_td">
                                <text class="text">左边</text>
                            </div>
                        </div>
                    </div>
                </div>
                <scroller ref="scroller_right_head" :scrollable="false" class="table_right" scroll-direction="horizontal">
                    <div class="table_bd">
                        <div class="table_th header">
                            <div class="table_td fixed_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td fixed_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td fixed_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td fixed_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td fixed_td">
                                <text class="text">12542</text>
                            </div>
                        </div>
                    </div>
                </scroller>
            </header>
            <cell class="table_box">
                <div class="table_left">
                    <div class="table_bd">
                        <div class="table_th header">
                            <div class="table_td">
                                <text class="text">左边</text>
                            </div>
                        </div>
                        <div class="table_th" v-for="(item, index) in data_item" :key="index">
                            <div class="table_td">
                                <text class="text">左边</text>
                            </div>
                        </div>
                    </div>
                </div>
                <scroller ref="scroller_right" class="table_right" scroll-direction="horizontal">
                    <div class="table_bd">
                        <div class="table_th header">
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                        </div>
                        <div class="table_th" v-for="(item, index) in data_item" :key="index">
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                            <div class="table_td">
                                <text class="text">12542</text>
                            </div>
                        </div>
                    </div>
                </scroller>
            </cell>
        </list>
    </div>
</template>
<script>
import { Utils } from 'weex-ui'
let _self;
export default {
    data: function() {
        return {
            data_item: [],
            bindingx_token: null,
        }
    },
    created() {
        _self = this;
        for (let i = 0; i < 100; i++) {
            this.data_item.push(i);
        }
        setTimeout(() => {
            this.bindingXscroll();
        }, 1000)
    },
    eros: {
        beforeBackAppear(params, options) {
            this.bindingXscroll();
        },
        beforeDisappear(options) {
            // 离开页面
            this.unbindXscroll();
        },
    },
    methods: {
        bindingXscroll() {
            let scroller_right = this.$refs.scroller_right.ref;
            let scroller_right_head = this.$refs.scroller_right_head.ref;
            this.$bindingx.bind({
                eventType: 'scroll',
                anchor: scroller_right,
                props: [{
                    element: scroller_right_head,
                    property: 'scroll.contentOffsetX',
                    expression: 'x+0'
                }]
            }, (e) => {
                this.bindingx_token = e.token;
                // console.log('bindingx', e);
            });
        },
        unbindXscroll() {
            if (this.bindingx_token) {
                this.$bindingx.unbind({
                    token: this.bindingx_token,
                    eventType: 'scroll'
                });
            }

        },
    },
    components: {
        
    },
    computed: {
        page_height() {
            return Utils.env.getPageHeight();
        }
    },
}

</script>
<style scoped >
.container {
    margin-top: 40px;
}
.list {
    width: 750px;
}

.table_box {
    width: 750px;
    flex-direction: row;
}

.table_left {
    width: 220px;
    background-color: #f60;
}

.table_right {
    flex: 1;
    background-color: #f90;
    flex-direction: row;
}

.table_th {
    flex-direction: row;
}

.table_td {
    padding: 20px;
    width: 250px;
    border-bottom-color: #ddd;
    border-bottom-width: 1px;
}

.fixed_td {
    border-bottom-color: red;
}

</style>
