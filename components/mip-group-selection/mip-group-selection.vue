<template>
  	<div class="wrapper">
        <h2>123</h2>
        <!-- <div class="item" v-for="item in list">
            <div class="item-name">{{ item.key }}</div>
        </div> -->
        <slot></slot>
  	</div>
</template>

<script>
export default {
    props: ['list'],
    computed : {},
    mounted () {},
    firstInviewCallback () {
        let me = this;
        let ele = this.$element;
        debugger;
        let dataUrl = ele.dataset.src;

        // 从本地和远程获取数据
        this.getData(dataUrl).then(function (data) {
            if (!data) {
                console.error('mip-city-selection 需要配置分组选项。可以配置到组件中，也可以配置远程数据。');
            }

            return templates.render(ele, data);
        }).then(function (html) {
            return;
            me.renderHtml(html);
            // 修改最下方分组的样式，增加marginBottom, 保证滚动后分组标题可以在页面最上方
            me.modifyMarginBottom();
            // 绑定侧边栏快捷选择事件
            me.bindSidebarClickEvent();
            // 绑定列表元素选择事件
            me.bindItemClickEvent();
        }, function (dat) {
            console.error(dat);
        });

        // 从本地和远程获取数据
        this.getData(dataUrl).then(function (data) {
            if (!data) {
                console.error('mip-city-selection 需要配置分组选项。可以配置到组件中，也可以配置远程数据。');
            }
            return;
        })
    },
    methods: {
        // 从本地和远程获取数据
        getData (url) {
            let me = this;
            let ele = this.$element;
            let groupData;

            // 远程获取数据是异步的，需要promise一下
            return new Promise(function (resolve, reject) {
                let groupData;
                if (url) {
                    console.log(`远程获取数据`);
                    // 优先远程获取数据，覆盖本地配置数据
                    fetch(url, {
                        credentials: 'include'
                    }).then(function (res) {
                        if (res.ok) {
                            res.json().then(function (data) {
                                resolve(data);
                            });
                        }
                        else {
                            reject('mip-city-selection 组件 Fetch 请求失败!');
                        }
                    }).catch(function (e) {
                        reject('mip-city-selection 组件 Fetch 请求失败!');
                    });
                }
                else if (me.$props.list) {
                    // 读取在页面中配置的数据
                    try {
                        // groupData = JSON.parse(groupData.textContent);
                        groupData = me.$props.list;
                        console.log(groupData);
                    }
                    catch (e) {
                        reject('mip-city-selection 组件 json 配置错误, 请检查 json 格式。');
                    }
                    resolve(groupData);
                }
            });
        },
        // 渲染数据
        renderHtml () {

        },
        // 修改最下方分组的样式，增加marginBottom, 保证滚动后分组标题可以在页面最上方
        modifyMarginBottom () {

        },
        // 绑定侧边栏快捷选择事件
        bindSidebarClickEvent () {

        },
        // 绑定列表元素选择事件
        bindItemClickEvent () {

        }
    }
}
</script>

<style scoped>
mip-group-selection {
    .mip-group-selection-content {
        overflow-x: hidden;
        overflow-y: scroll;
        -webkit-overflow-scrolling: touch;
        -webkit-box-sizing: border-box;
                box-sizing: border-box;
        width: 100%;
        height: 100%;
        padding: 0 8px 0 8px;
        color: #333;
    }

    .mip-group-selection-group {
        margin-top: 18px;
    }

    .mip-group-selection-title {
        font-weight: bold;
        color: #38f;
        padding: 0 10px;
    }

    .mip-group-selection-part-history {
        display: none;
    }

    .mip-group-selection-btn {
        height: 38px;
        line-height: 38px;
        text-align: center;
        background: #f8f8f8;
    }

    .mip-group-selection-item {
        height: 39px;
        border-bottom: 1px solid #eee;
        line-height: 39px;
        padding: 0 10px;
        display: block;
        -webkit-tap-highlight-color: rgba(0,0,0,.1);
    }
    .mip-group-selection-item.down {
        background: rgba(0,0,0,0.1);
    }

    .mip-group-selection-sidebar-wrapper {
        top: 0;
        right: 10px;
        bottom: 0;
        margin: 10px 0;
    }

    .mip-group-selection-sidebar {
        z-index: 50;
        right: 7px;
        overflow: scroll;
        -webkit-overflow-scrolling: touch;
        box-sizing: border-box;
        max-height: 100%;
        padding: 20px 0;
        border: 1px solid rgba(0, 0, 0, .05);
        text-align: center;
        color: #666;
        border-radius: 10px;
        background: rgba(255,255,255,.5);
    }

    .mip-group-selection-link {
        display: block;
        padding: 0 10px;
        font-size: 13px;
        font-weight: bold;
        line-height: 3;
        color: #38f;
        border-radius: 50%;
    }

    .mip-group-selection-letter-top {
        position: absolute;
        z-index: 40;
        top: 44px;
        left: 0;
        display: none;
        -webkit-box-sizing: border-box;
                box-sizing: border-box;
        width: 100%;
        height: 50px;
        padding-left: 16px;
        line-height: 50px;
        background: #fff;
    }

    .mip-group-selection-large-char {
        position: absolute;
        top: 50%;
        left: 50%;
        display: none;
        width: 78px;
        height: 78px;
        margin-top: -39px;
        margin-left: -39px;
        font-size: 36px;
        line-height: 78px;
        text-align: center;
        color: #fff;
        border-radius: 3px;
        background: rgba(51, 51, 51, .4);
    }
}
</style>
