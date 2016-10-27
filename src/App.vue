<template>
  <div id="app">
    <img src="./assets/logo.png">
    <tree class="tree-wrap" :model="models" :methods="method" ></tree>
  </div>
</template>

<script>
import Tree from './components/tree/'

export default {
  name: 'app',
  data () {
    return {
      models: {
        code: '1',
        name: '第一集',
        open: true,
        checked: true,
        children: [
          {
            code: '11',
            name: '第一回',
            open: true,
            checked: true,
            children: [
              {
                code: '111',
                name: '第一节',
                open: true,
                checked: true,
                children: [
                  {
                    code: '1111',
                    name: '第一幕',
                    open: true,
                    checked: false
                  },
                  {
                    code: '1112',
                    name: '第二幕',
                    open: true,
                    checked: false
                  }
                ]
              }
            ]
          },
          {
            code: '12',
            name: '第二回',
            open: true,
            checked: false
          }
        ]
      },
      method: {
        click: function (m) {
          console.log('点击回调', m.name)
        }
      }
    }
  },
  methods: {
    click: function (m) {
      console.log(m)
    }
  },
  components: {
    Tree
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  font-size: 14px;
}
ul, li {
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.tree-wrap {
  width: 300px;
  margin: auto;
  text-align: left;
}
</style>
