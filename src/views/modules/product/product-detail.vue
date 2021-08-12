<template>
  <el-dialog
    title="商品详细"
    :close-on-click-modal="false"
    :visible.sync="visible"
    :append-to-body="true"
    width="60%"
    >
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()">
      <div>
        <el-row>
          <el-col :span="24">
        <el-form-item>
          <el-form-item label="商品编号" prop="" align="left">
            <el-input :placeholder="itemNum" v-model="input_itemNum" clearable :style="{width: '20%'}">
            </el-input>
          </el-form-item>
        </el-form-item>
          </el-col>
        <el-form-item>
          <el-form-item label="条形码" prop="" align="left">
            <el-input autosize :placeholder="barCode" clearable :style="{width: '20%'}" :disabled="enablemodify">
            </el-input>
          </el-form-item>
        </el-form-item>
        <el-form-item>
          <el-form-item label="商品名称" prop="" align="left">
            <el-input :placeholder="itemName" clearable :style="{width: '20vh'}" :disabled="enablemodify">
            </el-input>
          </el-form-item>
        </el-form-item>
        </el-row>
      </div>
      <el-form-item>
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="库存资料" name="first">
            <productdetailtab>
            </productdetailtab>
          </el-tab-pane>
          <el-tab-pane label="购买记录" name="second">购买记录</el-tab-pane>
          <el-tab-pane label="供应商信息" name="third">供应商信息</el-tab-pane>
          <el-tab-pane label="库存信息" name="fourth">库存信息</el-tab-pane>
        </el-tabs>
      </el-form-item>
      <el-form>
      </el-form>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click= "enablemodify = false">修改商品</el-button>
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
import productdetailtab from './product-detail-tab.vue'
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
  methods: {
    init (row) {
      this.visible = true
      console.log(row)
      this.itemNum = row.itemNum
      this.itemName = row.itemName
      this.barCode = row.barCode
    },
    // 表单提交
    dataFormSubmit () {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl('/sys/oss/saveConfig'),
            method: 'post',
            data: this.$http.adornData(this.dataForm)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.visible = false
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        }
      })
    }
  },
  components: {
    productdetailtab
  }
}
</script>
