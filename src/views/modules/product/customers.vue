<template>
  <div>
    <el-container style="height: 800px; border: 1px solid #eee">
      <el-container>
<!--        <el-header style="text-align: right; font-size: 12px">-->
<!--          <span style="text-align: center; font-size: 15px;">{{ cate }}</span>-->
<!--          <el-dropdown>-->
<!--            <i class="el-icon-setting" style="margin-right: 15px"></i>-->
<!--            <el-dropdown-menu slot="dropdown">-->
<!--              <el-dropdown-item>查看</el-dropdown-item>-->
<!--              <el-dropdown-item>新增</el-dropdown-item>-->
<!--              <el-dropdown-item>删除</el-dropdown-item>-->
<!--            </el-dropdown-menu>-->
<!--          </el-dropdown>-->
<!--        </el-header>-->

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
          <el-table v-loading="loading" :data="customers" max-height="600" height="600" border stripe style="width: 100%;">
            <el-table-column prop="customerName" label="客户" width="380">
            </el-table-column>
            <el-table-column prop="customerNum" label="客户编号">
            </el-table-column>
          </el-table>
          <el-pagination
            @size-change="sizeChangeHandle"
            @current-change="currentChangeHandle"
            :current-page="pageIndex"
            :page-sizes="[10, 20, 50, 100]"
            :page-size="pageSize"
            :total="totalPage"
            layout="total, sizes, prev, pager, next, jumper"
          >
          </el-pagination>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  components: {},
  props: {},
  data () {
    const listMenu = []
    const customers = []
    const cate = ' '
    const input = ''
    const search = false
    const selectedvalue = ''
    const options = [{
      value: 'name',
      label: '按客户名称'
    }, {
      value: 'postcode',
      label: '按客户邮编'
    }, {
      value: 'phone',
      label: '按手机号码'
    }, {
      value: 'customerNum',
      label: '按客户编号'
    }]
    return {
      input,
      customers,
      listMenu,
      cate,
      search,
      options,
      selectedvalue,
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
    requestData (string) {
      this.search = true
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
          this.customers = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.orders = []
          this.totalPage = 0
        }
      })
    },
    searchItem () {
      if (this.input != null) {
        console.log(this.selectedvalue)
        if (this.selectedvalue === 'customerNum') {
          let string = '/product/customer/customerNum/' + this.input
          this.loading = true
          this.requestData(string)
        } else if (this.selectedvalue === 'name') {
          this.loading = true
          let string = '/product/customer/name/' + this.input
          this.requestData(string)
        } else if (this.selectedvalue === 'postcode') {
          this.loading = true
          let string = '/product/customer/postcode/' + this.input
          this.requestData(string)
        } else if (this.selectedvalue === 'phone') {
          this.loading = true
          let string = '/product/customer/phone/' + this.input
          this.requestData(string)
        }
      }
    },
    getDataList () {
      this.$http({
        url: this.$http.adornUrl('/product/customer/listall'),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.customers = data.page.list
          this.totalPage = data.page.totalCount
          console.log(data.page.list)
        } else {
          this.customers = []
          this.totalPage = 0
        }
      })
    },
    // 每页数
    sizeChangeHandle (val) {
      this.pageSize = val
      if (this.search) {
        this.searchItem()
      } else {
        this.getDataList()
      }
    },
    // 当前页
    currentChangeHandle (val) {
      this.pageIndex = val
      if (this.search) {
        this.searchItem()
      } else {
        this.getDataList()
      }
    }
  },
  beforeCreate () {
  },
  created () {
    this.getDataList()
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
