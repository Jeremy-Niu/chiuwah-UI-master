<template>
  <el-dialog
    title="detail"
    :close-on-click-modal="false">
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="活动名称">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="活动区域">
        <el-select v-model="form.region" placeholder="请选择活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="活动时间">
        <el-col :span="11">
          <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
        </el-col>
        <el-col class="line" :span="2">-</el-col>
        <el-col :span="11">
          <el-time-picker placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="即时配送">
        <el-switch v-model="form.delivery"></el-switch>
      </el-form-item>
      <el-form-item label="活动性质">
        <el-checkbox-group v-model="form.type">
          <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
          <el-checkbox label="地推活动" name="type"></el-checkbox>
          <el-checkbox label="线下主题活动" name="type"></el-checkbox>
          <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="特殊资源">
        <el-radio-group v-model="form.resource">
          <el-radio label="线上品牌商赞助"></el-radio>
          <el-radio label="线下场地免费"></el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="活动形式">
        <el-input type="textarea" v-model="form.desc"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
    closeDetail () {
      this.preordervisable = false
    },
    onSubmit () {
      console.log('submit!')
    }
  }
}
</script>

<style scoped>

</style>


<!--<template>-->
<!--  <div class="mod-home">-->
<!--&lt;!&ndash;    <h3>项目介绍</h3>&ndash;&gt;-->
<!--&lt;!&ndash;    <ul>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>renren-fast-vue基于vue、element-ui构建开发，实现<a href="https://gitee.com/renrenio/renren-fast" target="_blank">renren-fast</a>后台管理前端功能，提供一套更优的前端解决方案</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>前后端分离，通过token进行数据交互，可独立部署</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>主题定制，通过scss变量统一一站式定制</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>动态菜单，通过菜单管理统一管理访问路由</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>数据切换，通过mock配置对接口数据／mock模拟数据进行切换</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>发布时，可动态配置CDN静态资源／切换新旧版本</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>演示地址：<a href="http://demo.open.renren.io/renren-fast" target="_blank">http://demo.open.renren.io/renren-fast</a> (账号密码：admin/admin)</li>&ndash;&gt;-->
<!--&lt;!&ndash;    </ul>&ndash;&gt;-->
<!--&lt;!&ndash;    <h3>获取帮助</h3>&ndash;&gt;-->
<!--&lt;!&ndash;    <ul>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>官方社区：<a href="https://www.renren.io/community" target="_blank">https://www.renren.io/community</a></li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>前端Git地址：<a href="https://github.com/renrenio/renren-fast-vue" target="_blank">https://github.com/renrenio/renren-fast-vue</a></li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>后台Git地址：<a href="https://gitee.com/renrenio/renren-fast" target="_blank">https://gitee.com/renrenio/renren-fast</a></li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>代码生成器：<a href="https://gitee.com/renrenio/renren-generator" target="_blank">https://gitee.com/renrenio/renren-generator</a></li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>如需关注项目最新动态，请Watch、Star项目，同时也是对项目最好的支持</li>&ndash;&gt;-->
<!--&lt;!&ndash;    </ul>&ndash;&gt;-->
<!--&lt;!&ndash;    <h3>官方QQ群</h3>&ndash;&gt;-->
<!--&lt;!&ndash;    <ul>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>高级群：324780204(大牛云集，跟大牛学习新技能)</li>&ndash;&gt;-->
<!--&lt;!&ndash;      <li>普通群：145799952(学习交流，互相解答各种疑问)</li>&ndash;&gt;-->
<!--&lt;!&ndash;    </ul>&ndash;&gt;-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--  export default {-->
<!--  }-->
<!--</script>-->

<!--<style>-->
<!--  .mod-home {-->
<!--    line-height: 1.5;-->
<!--  }-->
<!--</style>-->

