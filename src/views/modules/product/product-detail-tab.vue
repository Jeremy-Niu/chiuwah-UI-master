<template>
  <div>
    <div>
      <el-form class="stock" :label-position="labelPosition" label-width="20%" :model="Item">
        <el-form-item class="item-group" label="库存上限">
          <el-input class="item" v-model="Item.name"></el-input>
        </el-form-item>
        <el-form-item class="item-group" label="库存下限">
          <el-input class="item" v-model="Item.region"></el-input>
        </el-form-item>
        <el-form-item class="item-group" label="最后进货价">
          <el-input class="item" v-model="Item.type"></el-input>
        </el-form-item>
        <el-form-item class="item-group" label="成本价">
          <el-input class="item" v-model="Item.type"></el-input>
        </el-form-item>
        <el-form-item class="item-group" label="活动形式" prop="desc">
          <el-input type="textarea" v-model="ruleForm.desc"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'product-detail-tab',
  data () {
    return {
      labelPosition: 'left',
      Item: {
        name: 'sdf',
        region: '',
        type: 'ssd'
      },
      ruleForm: {
        des: ''
      }
    }
  }
}
</script>

<style scoped>
.item {
  width: 30%;
}

.stock {
  width: 50%;
}

.item-group {
  margin-bottom: 12px;
  margin-top: 12px;
}
</style>
