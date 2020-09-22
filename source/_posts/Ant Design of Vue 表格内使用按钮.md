---
title: Ant Design of Vue 表格内使用按钮代码
date: 2019.07.01 10:06
---


```vue

<template>
    <div>
        <a-table :columns="columns" :dataSource="data" bordered rowKey="guid">
            <span slot="action" slot-scope="text, record">
                <a-button type="primary" @click="show(record.id)">查看</a-button>
                <a-divider type="vertical" />
                <a-button type="success" @click="edit(record.id)">修改</a-button>
                <a-divider type="vertical" />
                <a-button type="danger" @click="del(record.id)">删除</a-button>
            </span>
        </a-table>
    </div>
</template>

<script>
const columns = [
    {
        title: '服务名',
        dataIndex: 'name',
        scopedSlots: { customRender: 'name' }
    },
    {
        title: '激活状态',
        dataIndex: 'isEnabled',
        customRender: function(text, record, index) {
            return record.isEnabled ? '激活' : '禁用';
        }
    },
    {
        title: '描述',
        dataIndex: 'description'
    },
    {
        title: '操作',
        dataIndex: 'operation',
        fixed: 'right',
        width: 260,
        scopedSlots: { customRender: 'action' }
    }
];
export default {
    data() {
        return {
            pluginId: '',
            pluginName: '',
            // 表格显示
            data: [],
            // 表格标题
            columns,
            targetId: ''
        };
    },
    watch: {
        $route(to, from) {
            //监听路由是否变化
            if (to.query.id != from.query.id) {
                this.id = to.query.id;
                this.pluginName = this.$route.query.name;
                this.init(); //重新加载数据
            }
        }
    },
    mounted() {
        this.pluginId = this.$route.query.id;
        this.pluginName = this.$route.query.name;
        this.init();
    },
    methods: {
        init() {
            this.$http.get('service/plugins/services').then(res => {
                var arr = [];
                let name = this.pluginName;
                for (let i = 0; i < res.services.length; i++) {
                    if (res.services[i].type == name) {
                        arr.push(res.services[i]);
                    }
                }
                this.data = arr;
            });
        },
        // 根据id获取服务信息
        getInfoById(id) {
            for (let i = 0; i < this.data.length; i++) {
                if (this.data[i].id == id) {
                    return this.data[i];
                }
            }
        },
        show(id) {
            console.log(id);
        },
        edit(id) {
            console.log(id);
        },
        del(id) {
            var info = this.getInfoById(id);
            this.ModalText = '您确定要删除 ' + info.name + ' 吗?';
            this.visible = true;
            this.targetId = id;
        }
    }
};
</script>

<style scoped="scoped">
</style>
```
