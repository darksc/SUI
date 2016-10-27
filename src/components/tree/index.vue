<template>
  <ul>
    <li>
      <div class="item">
        <span v-if="model.children === undefined">[o]</span>
        <span @click.stop="open()" v-if="model.children !== undefined">[{{model.open ? '-' : '+'}}]</span>
        <input type="checkbox" @click="check(model)" v-model="model.checked"/>
        <span @click.stop="click()">{{model.name}}</span>
      </div>
      <tree v-for="item in model.children" :model="item" :methods="methods" v-show="model.open"></tree>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'tree',
  props: {
    model: {
      type: Object
    },
    methods: {
      type: Object
    }
  },
  methods: {
    // 点击节点
    click: function () {
      console.log('当前点击节点', this.model.name)
      if (this.methods.click !== undefined && this.methods !== undefined) {
        this.methods.click(this.model)
      }
    },
    open: function () {
      this.model.open = !this.model.open
      console.log('当前收缩节点', this.model.name, this.model.open)
    },
    // 选中节点
    check: function (model) {
      setTimeout(() => {
        console.log('当前选中节点', model.name, model.checked)
        // 检查同级节点
        let childrenCheck = this.checkChildren(this.$parent)
        // 通过同级节点状态，改变父级节点
        this.parentCheck(this.$parent, childrenCheck)
        // 选中子集节点
        if (model.children === undefined) return
        this.checkAll(model, model.checked)
      }, 1)
    },

    checkAll: function (model, checked) {
      model.checked = checked
      // 选中子集节点
      for (let i in model.children) {
        this.checkAll(model.children[i], model.checked)
      }
    },

    // 检查同级节点状态
    checkChildren: function (parent) {
      if (parent.$options._componentTag === 'tree') {
        for (let i in parent.model.children) {
          if (!parent.model.children[i].checked) {
            return false
          }
        }
        return true
      }
    },

    // 改变父级节点状态
    parentCheck: function (parent, check) {
      if (parent.$options._componentTag === 'tree') {
        parent.model.checked = check
        this.parentCheck(parent.$parent)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.item {
    cursor: pointer;
    font-family: monospace;
    color: #333;
}
ul {
    padding-left: 1em;
    list-style: none;
}

</style>
