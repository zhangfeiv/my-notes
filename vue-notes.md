#### 插件

右键弹框 vue-contextmenujs

百度富文本 vue-ueditor-wrap

流程图 g6



#### 树形组件

https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-tree-view?from-embed

##### 父组件

```
<template>
    <ul id="demo">
        <tree-item
            v-for="(item, index) in treeData"
            :key="index"
            :item="item"
            :activeId="activeId"
            @handleActiveName="handleActiveName"
            style="cursor: pointer;"
        ></tree-item>
    </ul>
</template>

<script>
var treeData = [
    { name: 'hello', id: 1 },
    { name: 'wat', id: 2 },
    {
        name: 'child folder',
        id: 3,
        children: [
            {
                id: 4,
                name: 'child folder',
                children: [
                    { name: 'hello', id: 5 },
                    { name: 'wat', id: 6 },
                    {
                        name: 'hi vue',
                        id: 9,
                        children: [
                            { name: 'hello', id: 10 },
                            { name: 'wat', id: 11 }
                        ]
                    }
                ]
            },
            { name: 'hello', id: 7 },
            { name: 'wat', id: 8 }
        ]
    }
];
import TreeItem from '@/components/TreeItem';
export default {
    components: {
        TreeItem
    },
    data() {
        return {
            treeData: treeData,
            activeId: null
        };
    },
    methods: {
        handleActiveName(val) {
            this.activeId = val;
        }
    }
};
</script>

<style lang="scss" scoped></style>
```

##### 子组件

```
<template>
    <li>
        <div :class="[activeId === item.id ? 'active' : '']" @click="toggle(item.id)">
            {{ item.name }}
            <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
        </div>
        <ul v-show="isOpen" v-if="isFolder">
            <tree-item
                v-for="(child, index) in item.children"
                :key="index"
                :item="child"
                :activeId="activeId"
                @handleActiveName="$emit('handleActiveName', $event)"
            ></tree-item>
        </ul>
    </li>
</template>

<script>
export default {
    name: 'treeItem',
    props: {
        item: Object,
        activeId: Number
    },
    data: function() {
        return {
            isOpen: false
        };
    },
    computed: {
        isFolder: function() {
            return this.item.children && this.item.children.length;
        }
    },
    methods: {
        toggle: function(val) {
            this.$emit('handleActiveName', val);
            if (this.isFolder) {
                this.isOpen = !this.isOpen;
            }
        }
    }
};
</script>

<style lang="scss" scoped>
.active {
    background: pink;
}
</style>
```



#### el-form-item 循环校验

```
<template>
  <div>
    <el-form :model="ruleForm" label-width="100px" ref="ruleForm" class="demo-ruleForm">
      <el-form-item label="标题" prop="title" :rules="rules.title">
        <el-input v-model="ruleForm.title"></el-input>
      </el-form-item>
      <div v-for="(item, index) in ruleForm.userList" :key="index">
        <el-form-item label="姓名：" :prop="'userList.' + index + '.name'" :rules="rules.name">
          <el-input v-model="item.name" size="mini" />
        </el-form-item>
        <el-form-item label="手机号：" :prop="'userList.' + index + '.phone'" :rules="rules.phone">
          <el-input v-model="item.phone" size="mini" />
        </el-form-item>
      </div>
      <el-form-item>
        <el-button type="primary" @click="submitForm">立即创建</el-button>
        <el-button @click="resetForm">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
        title: '',
        userList: [
          {
            name: '',
            phone: ''
          },
          {
            name: '',
            phone: ''
          }
        ]
      },
      rules: {
        title: [
          {
            required: true,
            message: '请输入标题',
            trigger: 'blur'
          }
        ],
        phone: [
          {
            required: true,
            message: '请输入手机号',
            trigger: 'blur'
          }
        ],
        name: [
          {
            required: true,
            message: '请输入姓名',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    submitForm() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm() {
      this.$refs.ruleForm.resetFields()
    }
  }
}
</script>

```



#### el-dialog  el-popover

是否插入到body属性：append-to-body



#### scss换肤

##### 皮肤文件 theme.scss

```
$background-main-color1:#E9EDF0;    //背景色
$background-main-color2:#011E30;    //背景色

$font-main-color1:#4893e6;   //主要字体颜色
$font-main-color2:#168ABD;   //主要字体颜色
```

##### 解析文件 mixin.scss

```
@import "./theme.scss";    //引入声明的皮肤文件

//初次进入调用
@mixin background-main-color($color){ //@mixin 后面的函数名称为自定义。
    background-color: $color;   //背景颜色默认为参数
    [background-main-color="background-main-color2"] & {    //如果条件成立，背景色则用$background-main-color2
        background-color: $background-main-color2;    //这个$background-main-color2已经在theme.scss中定义过了。  
    }
}

@mixin font-main-color($color){
    color: $color;   //字体颜色默认为参数
    [font-main-color="font-main-color2"] & {    //2
        color: $font-main-color2;
    }
}
```

##### 调用文件 xx.vue

（注意如果在某个scss中调用，需要将mixin和theme文件都引入；如果在单个vue中调用，假设你全局引入了scss，则不需要在页面中引入这两个文件）

```
.all{
  @include background-main-color($background-main-color1);
  //background-main-color 为mixin中定义的名称，括号里为theme中声明的值。
  解析为 background:"#E9EDF0"
}
```

修改文件 xx.vue

```
window.document.documentElement.setAttribute("background-main-color","background-main-color2");
//第一个"background-main-color" 指的是mixin中我们自定义声明的名称，"background-main-color2"指的是我们传的参数，
//相当于mixin中的 $color，如果条件成立则会用下面的样式。
```



#### el-scrollbar 滚动条

```
.el-scrollbar {
    height: 500px;
    /deep/ .el-scrollbar__wrap {
      overflow-y: scroll;
      overflow-x: hidden;
      width: 110%;
      height: 100%;
    }
  }
```



#### 查看webpack配置

vue inspect -v 查看所有配置

vue inspect --mode <mode>（如vue inspect --mode development）查看指定环境的配置

vue inspect --rules 查看所有已配置规则名称列表

vue inspect --rule <ruleName>（如 vue inspect --rule svg） 查看指定配置规则

vue inspect --plugins 查看所有已配置插件名称列表

vue inspect --plugin <pluginName>（如vue inspect --plugin html）查看指定插件配置

vue inspect -h 帮助信息