<template>
  <div>
    <el-container style="height: 100%; border: 1px solid #eee">
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
      <el-main style="height: 100%">
        <div class="table-settings">
          <el-row>
            <el-col :span="12">
              <el-input placeholder="请输入内容" prefix-icon="el-icon-search" v-model="input" align="left"
                        style="width: 280px" @keyup.enter.native="searchItem"></el-input>
              <el-select v-model="selectedvalue" @change="searchItem($event)" placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
              <el-button type="primary" icon="el-icon-search" @click="searchItem">搜索</el-button>
            </el-col>
            <el-col :span="12">
              <div class="block">
                <span class="demonstration">日期选择</span>
                <el-date-picker
                  v-model="date1"
                  type="daterange"
                  align="right"
                  unlink-panels
                  value-format="yyyy-MM-dd"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  :picker-options="pickerOptions">
                </el-date-picker>
                <el-button type="primary" icon="el-icon-search" @click="dateconfirm">确认日期</el-button>
                <el-button type="primary" size="small" icon="el-icon-edit" @click="$router.push({ name: 'sds' })">修改
                </el-button>
              </div>
            </el-col>
          </el-row>
        </div>
        <el-table v-loading="loading" :data="orders" height="800px" border stripe style="width: 100%;"
                  @row-click="getPreoderNum"
                  @row-dblclick="$router.push({ name: 'sds', params:{ order: 'sdfcv' }, query:{ordernum: oredernum}})"
        >
          <el-table-column prop="orderNum" label="单据编号" width="120">
          </el-table-column>
          <el-table-column prop="typeBusiness" label="业务类型">
          </el-table-column>
          <el-table-column prop="customer.customerName" label="客户">
          </el-table-column>
          <el-table-column prop="customerNum" label="客户编号">
          </el-table-column>
          <el-table-column prop="date" label=日期>
          </el-table-column>
          <el-table-column prop="postCode" label="邮编">
          </el-table-column>
          <el-table-column prop="source" label="source">
          </el-table-column>
          <el-table-column prop="stateTime" label="state-time">
          </el-table-column>
          <el-table-column prop="totalNum" label="合计数量">
          </el-table-column>
          <el-table-column prop="totalAmount" label="合计金额">
          </el-table-column>
          <el-table-column prop="totalTaxAmount" label="合计税额">
          </el-table-column>
          <el-table-column prop="totalTaxBeforeAmount" label="合计税前金额">
          </el-table-column>
          <el-table-column prop="totalWeight" label="合计重量">
          </el-table-column>
          <el-table-column prop="receivables" label="应收款">
          </el-table-column>
          <el-table-column prop="totalAmountWithTax" label="合计有税商品税前金额">
          </el-table-column>
          <el-table-column prop="jobNum" label="工号">
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
      </el-main>
    </el-container>
  </div>
</template>

<script>
import PreorderDetail from './preordersdetail'

export default {
  components: {PreorderDetail},
  props: {},
  data () {
    const listMenu = []
    const orders = []
    const cate = ' '
    const input = ''
    const search = false
    const selectedvalue = ''
    const oredernum = ''
    // const currentdate = new Date()
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
      orders,
      listMenu,
      cate,
      search,
      options,
      selectedvalue,
      oredernum,
      value: '',
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      loading: false,
      preordervisable: false,
      pickerOptions: {
        shortcuts: [{
          text: '最近一周',
          onClick (picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近一个月',
          onClick (picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近三个月',
          onClick (picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            picker.$emit('pick', [start, end])
          }
        }]
      },
      date1: ''
    }
  },
  computed: {},
  watch: {},
  methods: {
    getPreoderNum (row) {
      this.oredernum = row.orderNum
      // console.log(this.oredernum)
    },
    showdetail () {
      this.preordervisable = true
    },
    requestData (string) {
      this.search = true
      this.cate = ''
      this.$http({
        url: this.$http.adornUrl(string),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize,
          'date1': this.date1[0],
          'date2': this.date1[1]
        })
      }).then(({data}) => {
        this.loading = false
        if (data && data.code === 0) {
          this.orders = data.page.list
          this.totalPage = data.page.totalCount
        } else {
          this.orders = []
          this.totalPage = 0
        }
      })
    },
    dateconfirm () {
      let string = '/preorder/general/info/date'
      console.log(this.date1)
      if (this.date1) {
        this.requestData(string)
      } else {
        this.getDataList()
      }
      console.log(this.orders)
    },
    searchItem () {
      if (this.input) {
        console.log(this.selectedvalue)
        if (this.selectedvalue === 'customerNum') {
          let string = '/product/customer/customerNum/' + this.input
          this.loading = true
          this.requestData(string)
        } else if (this.selectedvalue === 'name') {
          this.loading = true
          let string = '/product/customer/name/' + this.input
          this.requestData(string)
        } else if (this.selectedvalue === 'barcode') {
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
        url: this.$http.adornUrl('/preorder/general/list'),
        method: 'get',
        params: this.$http.adornParams({
          'page': this.pageIndex,
          'limit': this.pageSize
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.orders = data.page.list
          this.totalPage = data.page.totalCount
          console.log(data.page.list)
        } else {
          this.orders = []
          this.totalPage = 0
        }
      })
    },
    // 每页数
    sizeChangeHandle (val) {
      this.pageSize = val
      this.getDataList()
    },
    // 当前页
    currentChangeHandle (val) {
      this.pageIndex = val
      this.getDataList()
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
    this.getDataList()
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

