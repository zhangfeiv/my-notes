#### 插件

右键弹框 vue-contextmenujs

百度富文本 vue-ueditor-wrap

轻量级富文本 vue-quill-editor

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

#### .env.development 开发环境

msg=hi

VUE_APP_MSG = 'hello' 只能在客户端使用

##### 用法

console.log(process.env.msg)

console.log(process.env.VUE_APP_MSG)

#### 阻止页面返回

1.使用Vue中插件vue-prevent-browser-back（阻止单个页面）

```
<template>
<div>
　　无法后退
</div>
</template>

<script>
import preventBack from 'vue-prevent-browser-back'
export default {
	mixins: [preventBack]
}
</script>
```

2.使用js原生阻止页面返回（可能对其他页面有影响）

```
history.pushState(null, null, document.URL);
window.addEventListener('popstate', function () {
  history.pushState(null, null, document.URL)
})
```

#### 换肤—动态修改scss变量

皮肤文件 theme.scss

```
$background-main-color1:#E9EDF0;
$background-main-color2:#011E30;
```

解析文件 mixin.scss

```
@import "./theme.scss";    //引入声明的皮肤文件
  
//初次进入调用
@mixin background-main-color($color){ //@mixin 后面的函数名称为自定义。
    background-color: $color;   //背景色默认为参数
    [background-main-color="background-main-color2"] & {    //如果条件成立，背景色则用$background-main-color2
        background-color: $background-main-color2;    //这个$background-main-color2已经在theme.scss中定义过了。  
    }
}
```

页面中调用 home.vue （全局或页面引入mixin.scss）

```
<template>
  <div class="container">
    <button @click="changeTheme">changeTheme</button>
  </div>
</template>

<script>
	export default {
		methods: {
			changeTheme() {
				//第一个"background-main-color" 指的是mixin中我们自定义声明的名称，"background-main-color2"指的是我们传的参数，
        //相当于mixin中的 $color，如果条件成立则会用下面的样式。
				window.document.documentElement.setAttribute("background-main-color","background-main-color2");
			}
		}
	}
</script>


<style>
@import '@/assets/style/mixin.scss';
.container{
	//background-main-color 为mixin中定义的名称，括号里为theme中声明的值。解析为 background:"#E9EDF0"
  @include background-main-color($background-main-color1);
}
</style>
```

#### vue列表实现拖拽

```
// npm install vuedraggable --save
// v-model="list" 如果发生了拖拽，list的数据并不会跟着变化
// :list="list" 如果发生了拖拽，list的数据会跟着变化

// 基础用法
<template>
  <div>
    <draggable :list="list">
      <div style="cursor: pointer" v-for="item in list" :key="item.id" :title="item.title" :name="item.id">
         {{item.title}}
      </div>
  </draggable>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  components: { draggable },
  data() {
      return {
         list: [
          {
           title: "aaa",
           id: 1,
          },
         {
           title: "bbb",
           id: 2,
        }
     ],
    };
  },
};
</script>

// tag 和 componentData
<template>
  <div style="margin: 20px;">
    <draggable tag="el-collapse" :list="list" :component-data="collapseComponentData" >
      <el-collapse-item v-for="item in list" :key="item.id" :title="item.title" :name="item.id" >
        <draggable :list="item.text">
          <div style="cursor: pointer" v-for="(lign, idx) in item.text" :key="idx">{{ lign }}</div>
        </draggable>
      </el-collapse-item>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable';
export default {
  components: { draggable },
  data() {
      return {
         list: [
          {
           title: "一级",
           id: 1,
           text: [  "测试001",  "测试002" ]
          },
         {
           title: "二级",
           id: 2,
           text: [  "测试003",  "测试004"  ]
        }
     ],
     activeNames: [1],
     collapseComponentData: {
        on: {
         change: this.inputChanged
        },
        props: {
         value: this.activeNames
        }
     }
    };
  },
  methods: {
    inputChanged(val) {
       this.activeNames = val;
     }
  }
};
</script>
```

#### transition基础用法

##### 单个元素的过渡

```
<div id="demo">
  <button v-on:click="show = !show">Toggle</button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>

new Vue({
  el: '#demo',
  data: {
    show: true
  }
})

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
```

##### 多个元素的过渡

```
<div id="demo">
  <button @click="show = !show">Toggle</button>
  <transition tag="ul" name="fade" mode="out-in">
      <div v-if="show" key="con">content</div>
      <div v-else key="emp">empty</div>
  </transition>
</div>

new Vue({
  el: '#demo',
  data() {
    return {
      show: true
    }
  }
})
.fade-enter-active, .fade-leave-active {
  transition: all 0.5s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
```

##### 多个组件的过渡

```
<div id="demo">
  <label><input type="radio" v-model="compTag" value="compA" />组件A</label>
  <label><input type="radio" v-model="compTag" value="compB" />组件B</label>
  <transition name="fade" mode="out-in">
    <component :is="compTag"></component>
  </transition>
</div>

new Vue({
  el: '#demo',
  data() {
    return {
      compTag: 'compA'
    }
  },
  components: {
    compA: {
      template: `<div>aaaaaaaaaa</div>`
    },
    compB: {
      template: `<div>bbbbbbbbbb</div>`
    }
  }
})
.fade-enter-active, .fade-leave-active {
  transition: all .5s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
```

##### 列表过渡

```
<div id="list-demo" class="demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>

new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})

.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
```



