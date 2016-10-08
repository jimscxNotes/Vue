### Vue学习笔记
#### 1. Vue 中的filter 使用
* 自定义filter
```
//特别简单的例子 把传递进来的数据改变为百分比的形式
Vue.filter('percentData', function(data) {
  return data + '%'
})
//在html中使用
<span>{{cityData.data | percentData}}</span>
```

#### 2. Vue中的数据渲染
* 数据的的渲染`v-text`和`{{}}`
```
<span v-text="msg"></span>
<!-- same as -->
<span>{{msg}}</span>
```
* 加载html `v-html`
不再使用jquery 的html()或者是append（）方法

#### 3. Vue中的父子组件之间的数据传递
* 父组件向子组件传递数据
要使用props ，一定要首先在子组件中声明父组件要传递的数据使用`props:['xx']`属性,然后父组件直接使用`:xx`向子组件传递数据
```
//父组件
<div>
  <header-nav :home="active"></header-nav>
</div>
//子组件首先要使用props:[]声明
<div><img :src="home==true?'/static/img/Book-green.png':'/static/img/Book.png'"></div>
 props: ['home'],
```
* 子组件向父组件传递数据使用`this.$dispatch("xxx",xxxx)`,父组件使用'event:'便可以接收到子组件传递的数据
```
//这里是子组件
var Form = Vue.extend({
  props: {
    username: {
      type: String,
      default: "Unnamed"
    }
  },
  data: function() {
    return {
      input: "",
    };
  },
  template: `
    <h1>{{username}}'s Todo List</h1>
    <form v-on:submit="add" v-on:submit.prevent>
      <input type="text" v-model="input"/>
      <input type="submit" value='add' />
    </form>
    `,
  methods: {
    add: function() {
      this.$dispatch("add", this.input); //这里就是向父组件发送消息
      this.input = "";
    }
  }
});
//这里是父组件
var List = Vue.extend({
  template: `
    <ul>
      <li v-for='todo in list'>
        <label v-bind:class="{ done : todo.done }" >
          <input type="checkbox" v-model="todo.done"/>
          {{todo.title}}
        </label>
      </li>
    </ul>`,
  props: {
    initList: {
      type: Array
    }
  },
  data: function() {
    return {
      list: []
    }
  },
  events: {
    add: function(input) {
      if(!input) return false;
      this.list.unshift({
        title: input,
        done: false
      });
    }
  }
});
```


