<template>
  <el-dialog
    title="云"
    :close-on-click-modal="false"
    :visible.sync="visible"
    :append-to-body = "true">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="120px">
      <el-form-item size="mini" label="存储类型">
        <el-radio-group v-model="dataForm.type">

        </el-radio-group>
      </el-form-item>

      <el-form-item>
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="用户管理" name="first">商品图片</el-tab-pane>
          <el-tab-pane label="配置管理" name="second">历史价格</el-tab-pane>
          <el-tab-pane label="角色管理" name="third">供应商信息</el-tab-pane>
          <el-tab-pane label="定时任务补偿" name="fourth">库存信息</el-tab-pane>
        </el-tabs>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      visible: false,
      dataForm: {},
      dataRule: {}
    }
  },
  methods: {
    init () {
      this.visible = true
      console.log('sdfsd')
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
  }
}
</script>

