## Vue父子组件互调方法## 父组件调用子组件方法

### 父组件代码

```vue
<template>
    <button @click="getChild">点击调用子组件</button>
    <child ref="child"></child>
</template>
<script>
    import child from "child";
    export default{
        components: {child},
        methods: {
            getChild(){
                // 调用子组件方法
                this.$ref.child.init();
            }
        }
    }
</script>
```

### 子组件代码

```vue
<template>
    {{value}}
</template>
<script>
    export default{
        data(){
            return {
                value: "我是子组件"
            }
        },
        methods: {
            init(){
                this.value = "父组件调用了方法";
            }
        }
    }
</script>
```

## 子组件调用父组件方法
### 父组件代码

```vue
<template>
    {{value}}
    <child @init="init"></child>
</template>
<script>
    import child from "child";
    export default{
        components: {child},
        data(){
            return {
                value: "我是父组件"
            }
        },
        methods: {
            init(){
                this.value = "子组件调用了方法";
            }
        }
    }
</script>
```

### 子组件代码

```vue
<template>
    <button @child="getFather">调用父组件方法</button>
</template>
<script>
    export default{
        methods: {
            getFather(){
                // 调用父组件方法
                this.$emit("init");
            }
        }
    }
</script>
```