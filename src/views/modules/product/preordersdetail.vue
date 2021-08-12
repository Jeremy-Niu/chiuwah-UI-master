<template>
  <el-container>
    <el-header >
      <el-descriptions-item v-on:cuechange="itemName" label="用户名">{{init}}</el-descriptions-item>
    </el-header>
    <el-main>
      <div class="table-settings">
        <el-input placeholder="请输入内容" prefix-icon="el-icon-search" v-model="input" align="left"
                  style="width: 300px" @keyup.enter.native="searchItem"></el-input>
        <el-select v-model="selectedvalue" @change="searchItem($event)" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <el-button type="primary" icon="el-icon-search" @click="searchItem">搜索</el-button>
      </div>
      <el-table v-loading="loading" :data="items" max-height="700" height="600" border stripe style="width: 100%;"
                @row-dblclick="handledialog"
      >
        <el-table-column prop="itemName" label="名称">
        </el-table-column>
        <el-table-column prop="barcode" label="条形码" width="120">
        </el-table-column>
        <el-table-column prop="itemNum" label="商品编号">
        </el-table-column>
      </el-table>
    </el-main>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      enablemodify: true,
      visible: false,
      dataForm: {},
      dataRule: {},
      input_itemNum: '',
      itemNum: '',
      itemName: '',
      barCode: ''
    }
  },
  computed: {
    init () {
      console.log(this.$route.query.ordernum)
      this.itemName = this.$route.query.ordernum
      return this.itemName
    }
  },
  methods: {
    init () {
      this.visible = true
      this.itemName = this.$route.query.ordernum
      console.log(this.$route.query.ordernum)
    },
    // 表单提交
    dataFormSubmit () {
      console.log(this.$route.query.ordernum)
    },
    getPreorderItems () {
      console.log('sdf')
    }
  },
  created () {
    this.itemName = this.$route.query.ordernum
    this.getPreorderItems()
    this.init()
    console.log(this.$route.query.ordernum)
  },
  mounted () {
    this.itemName = this.$route.query.ordernum
    this.init()
    console.log(this.$route.query.ordernum)
  },
  updated () {
    this.itemName = this.$route.query.ordernum
    this.init()
    console.log(this.$route.query.ordernum)
  }
}
</script>
