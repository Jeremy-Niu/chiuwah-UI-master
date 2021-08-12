<template>
  <div>
    <el-container style="height:800px; border: 1px solid #eee">
      <el-aside style="background-color: rgb(238, 241, 246); width:18%">
        <el-table ref="menuTable" highlight-current-row :data="listMenu" stripe style=""
                  @row-dblclick="getDataList"
                  @row-click="getSelectType">
          <el-table-column ref="aside-header" prop="category" label="类型" style="width: 100%"></el-table-column>
        </el-table>
      </el-aside>
      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <span style="text-align: center; font-size: 15px;">{{ cate }}</span>
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>查看</el-dropdown-item>
              <el-dropdown-item @click.native="handledialog">新增</el-dropdown-item>
              <el-dropdown-item>删除</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-header>
        <div class="table-settings">
          <el-input placeholder="请输入内容" prefix-icon="el-icon-search" v-model="input" align="left"
                    style="width: 20%" @keyup.enter.native="searchItem"></el-input>
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
        <el-table v-loading="loading" :data="items" height="100%" border stripe style="width: 100%;"
                  @row-dblclick="handledialog">
          <el-table-column prop="itemName" label="名称">
          </el-table-column>
          <el-table-column prop="barcode" label="条形码" width="120">
          </el-table-column>
          <el-table-column prop="itemNum" label="商品编号">
          </el-table-column>
          <el-table-column prop="unit" label="单位">
          </el-table-column>
          <el-table-column prop="retailPrice" label="零售价">
          </el-table-column>
          <el-table-column prop="memberPrice" label="会员价">
          </el-table-column>
          <el-table-column prop="wholesalePrice1" label="批发价1">
          </el-table-column>
          <el-table-column prop="wholesalePrice2" label="批发价2">
          </el-table-column>
          <el-table-column prop="wholesalePrice3" label="批发价3">
          </el-table-column>
          <el-table-column prop="purchasePrice" label="进货价">
          </el-table-column>
          <el-table-column prop="lastPurchasePrice" label="最后进货价">
          </el-table-column>
          <el-table-column prop="number" label="数量">
          </el-table-column>
          <el-table-column prop="lastChangedTime" label="最后变更时间">
          </el-table-column>
          <el-table-column prop="lastSaleTime" label="最后销售时间">
          </el-table-column>
        </el-table>
        <el-pagination
          @size-change="sizeChangeHandle"
          @current-change="currentChangeHandle"
          :current-page="pageIndex"
          :page-sizes="[10, 20, 50, 100]"
          :page-size="pageSize"
          :total="totalPage"
          layout="total, sizes, prev, pager, next, jumper">
        </el-pagination>
        <v-dialog v-if="dialogFormVisible" ref="dialog"></v-dialog>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import dialog from './product-detail'

export default {
  components: {'v-dialog': dialog},
  props: {},
  data () {
    const listMenu = []
    const items = []
    const cate = ' '
    const input = ''
    const search = false
    const selectedvalue = ''
    const dialogFormVisible = false
    const options = [{
      value: 'name',
      label: '按商品名称'
    }, {
      value: 'id',
      label: '按商品编号'
    }, {
      value: 'barcode',
      label: '按商品条形码'
    }, {
      value: 'pinyin',
      label: '按拼音编号'
    }]
    return {
      input,
      items,
      listMenu,
      cate,
      search,
      options,
      selectedvalue,
      dialogFormVisible,
      value: '',
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      loading: false
    }
  },
  computed: {},
  watch: {},
  methods: {
    handledialog (row) {
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs.dialog.init(row)
      })
    },
    requestData (string) {
      this.search = true
      this.cate = ''
      this.$http({
        url: this.$http.adornUrl(string),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize
        })
      }).then(({data}) => {
        this.loading = false
        if (data && data.code === 0) {
          this.items = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.dataList = []
          this.totalPage = 0
        }
      })
    },
    searchItem () {
      if (this.input != null) {
        console.log(this.selectedvalue)
        if (this.selectedvalue === 'id') {
          let string = '/product/info/id/' + this.input
          this.loading = true
          this.requestData(string)
        } else if (this.selectedvalue === 'name') {
          this.loading = true
          let string = '/product/info/name/' + this.input
          this.requestData(string)
        } else if (this.selectedvalue === 'barcode') {
          this.loading = true
          let string = '/product/info/barcode/' + this.input
          this.requestData(string)
        } else if (this.selectedvalue === 'pinyin') {
          this.loading = true
          let string = '/product/info/pinyin/' + this.input
          this.requestData(string)
        }
      }
    },
    getDataList (row, column, cell, event) {
      this.input = ''
      this.search = true
      this.$http({
        url: this.$http.adornUrl('/product/listbytype/' + this.cate),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.items = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.dataList = []
          this.totalPage = 0
        }
      })
    },
    getSelectType (row) {
      this.cate = row.category
      console.log(this.cate)
    },
    // 每页数
    sizeChangeHandle (val) {
      this.pageSize = val
      if (this.search) {
        this.searchItem()
      } else {
        this.getDataList(this.cate)
      }
    },
    // 当前页
    currentChangeHandle (val) {
      this.pageIndex = val
      if (this.search) {
        this.searchItem()
      } else {
        this.getDataList(this.cate)
      }
    },
    getType () {
      this.$http({
        url: this.$http.adornUrl('/category/type'),
        method: 'get'
      }).then(({data}) => {
        this.listMenu = data.data
      })
    }
  },
  beforeCreate () {
  },
  created () {
    this.getType()
  },
  beforeMount () {
  },
  mounted () {
  },
  beforeUpdate () {
  },
  updated () {
  }
}
</script>


<style>
.el-header {
  background-color: #B3C0D1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}

.table-settings {
  padding: 15px;
}

</style>
