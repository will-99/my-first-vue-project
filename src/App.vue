<template>
  <div id="app">
  <h1>{{title}}</h1>
  <input v-model="newItem" @keyup.enter="addNew"/>
  <button @click="clearAllTasks">删除所有任务</button>
  <ul>
    <li v-for="item in items" :class="{finished: item.isFinished}" @click="toggleFinish(item)" v-bind:style="{backgroundColor: item.bgColor}">{{item.label}}</li>
  </ul>
  </div>
</template>

<script>
import Store from './store';

var colors = [
  '#FE9778', // Agave
  '#FEDA76', '#76DFFE', '#FE769A', '#DFFE76', '#769AFE', // Sandy stone beach
    '#FE6739'//, '#FEE169', '#CDD452', '#F9722E', '#C9313D', // Sushi Maki
    //'#2E95A3', '#50B8B4', '#C6FFFA', '#E2FFA8'  // Agave
   ]

export default {
  data () {
    return {
     title: 'this is a todo list',
     items: Store.fetch(),
     newItem: ''
    }
  },
  watch: {
    items: {
      handler: function (items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggleFinish: function (item) {
      item.isFinished = !item.isFinished;
    },
    addNew: function(){
      var color = colors.pop();
      console.log(color);
      this.items.push({
        label: this.newItem,
        isFinished: false,
        bgColor: color
      })
      this.newItem = '';
      colors.unshift(color);
    },
    clearAllTasks: function(){
      Store.clear();
      this.items = [];
    }
  }
}
</script>

<style>
html{color:#000;background:#FFF;} 
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,form, 
fieldset,input,textarea,p,blockquote,th,td { 
margin:0; 
padding:0; 
} 
table { 
border-collapse:collapse; 
border-spacing:0; 
} 
fieldset,img { 
border:0; 
} 
address,caption,cite,code,dfn,em,strong,th,var { 
font-style:normal; 
font-weight:normal; 
} 
ol,ul { 
list-style:none; 
} 
caption,th { 
text-align:left; 
} 
h1,h2,h3,h4,h5,h6 { 
font-size:100%; 
font-weight:normal; 
} 
q:before,q:after { 
content:”; 
} 
abbr,acronym { border:0; 
} 

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul li {
  cursor: pointer;
  /*background-color: pink;*/
}

.finished {
  text-decoration: line-through;
}
</style>
