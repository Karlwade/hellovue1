<!-- eslint-disable vue/valid-template-root -->
<template>
  <el-container>
    <el-main>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column header-align="center" prop="itemName" label="案件名">
          <template slot-scope="{ row, $index }">
            <el-input v-if="ticketsIndex == $index" v-model="row.itemName"></el-input>
            <span v-if="ticketsIndex != $index">{{ row.itemName }}</span>
          </template>
        </el-table-column>
        <el-table-column header-align="center" prop="price" label="负责人">
          <template slot-scope="{ row, $index }">
            <el-select v-if="ticketsIndex == $index" v-model="row.name">
              <el-option v-for="item in names" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
            <!-- <el-input v-if="ticketsIndex == $index" v-model="row.price"></el-input> -->
            <span v-if="ticketsIndex != $index">{{ row.name}}</span>
          </template>
        </el-table-column>
        <el-table-column header-align="center" prop="address" label="金额">
          <template slot-scope="{ row, $index }">
            <el-input v-if="ticketsIndex == $index" v-model="row.value"></el-input>
            <span v-if="ticketsIndex != $index">{{ row.value }}</span>
          </template>
        </el-table-column>
        <el-table-column fixed="right" label="操作">
          <template slot-scope="{ row, $index }">
            <el-button
              v-if="ticketsIndex == $index"
              @click="ticketsClick($index, 'save')"
              type="text"
              size="small"
              >保存
            </el-button>
            <el-button
              v-if="ticketsIndex != $index"
              @click="ticketsClick($index, 'edit')"
              type="text"
              size="small"
              >编辑
            </el-button>
            <el-button @click="ticketsClick($index, 'delete')" type="text" size="small"
              >移除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
      <div class="textCenter">
        <br />
        <el-button type="primary" plain @click="addTickets">添加记录</el-button>
        <el-button type="primary" plain @click="calculate">计算结果</el-button>
      </div>
      <el-descriptions title="计算结果" border="true">
        <el-descriptions-item label="收入总额" v-model="totalResult">{{ totalResult }}</el-descriptions-item>
        <el-descriptions-item label="黄应收">{{ totalResult }}</el-descriptions-item>
        <el-descriptions-item label="章应收">苏州市</el-descriptions-item>
      </el-descriptions>

    </el-main>
  </el-container>
</template>

<script>
export default {
  components: {
    // 全局注册
  },
  data () {
    return {
      tableData: [], // table数组
      ticketsIndex: 0, // 票种table当前正在编辑的行
      names: [{value: '黄', label: '黄'}, {value: '章', label: '章'}],
      totalResult: 0
    }
  },
  created: function () {
    // 在模板渲染成html前调用。执行顺序:父组件-子组件
  },
  mounted () {
    // 在模板渲染成html后调用。执行顺序:子组件-父组件
  },
  methods: {
    // 监听方法click事件等，执行drawFeatures方法
    ticketsClick ($index, now) {
      // 票种点击事件
      if (now === 'save') {
        // 表示当前点击的是保存按钮
        this.ticketsIndex = null // 把正在修改的行，置为只读状态
      }
      if (now === 'edit') {
        // 表示当前点击的是修改按钮，把当前行置为编辑状态
        this.ticketsIndex = $index
      }
      if (now === 'delete') {
        // 表示当前点击的是删除按钮
        this.tableData($index, 1)
      }
    },
    addTickets () {
      // 添加一行票种
      if (JSON.stringify(this.tableData[this.tableData.length - 1]) !== '{}') {
        var obj = {}
        this.tableData.push(obj)
        this.ticketsIndex = this.tableData.length - 1 // 默认编辑当前添加的那条数据
      } else {
        this.$message.warning('已经存在一条空数据了!') // 失败提示后端返回消息
      }
    },
    calculate () {
      var sum = 0
      this.tableData.forEach(element => {
        sum = parseFloat(sum) + parseFloat(element.value)
      })
      this.totalResult = sum
    }
  },
  watch: {
    // 去监听一个值的变化，然后执行相对应的函数
  }
}
</script>
