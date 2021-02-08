##### 插件

右键弹框 vue-contextmenujs

百度富文本 vue-ueditor-wrap

流程图 g6



##### 树形组件

https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-tree-view?from-embed

父组件

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

子组件

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



##### el-form-item循环校验

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



##### el-dialog  el-popover  

是否插入到body属性：append-to-body