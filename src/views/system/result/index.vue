<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">成绩总分</label>
        <el-input v-model="query.resultTotal" clearable placeholder="成绩总分" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'
      -->
      <el-button
        v-if="crud.optShow.download"
        :loading="crud.downloadLoading"
        :disabled="!crud.data.length"
        class="filter-item"
        size="mini"
        type="warning"
        icon="el-icon-download"
        @click="crud.doExport"
      >导出</el-button>
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="成绩ID">
            <el-input v-model="form.resultId" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="成绩总分" prop="resultTotal">
            <el-input v-model="form.resultTotal" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="详细成绩">
            <el-input v-model="form.result" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="用户" prop="userId">
            <el-input v-model="form.userId" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="resultId" label="成绩ID" />
        <el-table-column prop="resultTotal" label="成绩总分" />
        <el-table-column prop="user.nickName" label="用户" />
        <el-table-column prop="user.school.schoolName" label="学校名称" />
        <el-table-column prop="result_1" label="题目1" />
        <el-table-column prop="result_2" label="题目2" />
        <el-table-column prop="result_3" label="题目3" />
        <el-table-column prop="result_4" label="题目4" />
        <el-table-column prop="result_5" label="题目5" />
        <el-table-column prop="result_6" label="题目6" />
        <el-table-column prop="result_7" label="题目7" />
        <el-table-column prop="result_8" label="题目8" />
        <el-table-column prop="result_9" label="题目9" />
        <el-table-column prop="result_10" label="题目10" />
        <!--
        <el-table-column v-permission="['admin','result:edit','result:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
        -->
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudResult from '@/api/system/result'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { resultId: null, resultTotal: null, result: null, userId: null }
export default {
  name: 'Result',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: 'result', url: 'api/result', sort: 'resultId,desc', crudMethod: { ...crudResult }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'result:add'],
        edit: ['admin', 'result:edit'],
        del: ['admin', 'result:del']
      },
      rules: {
        resultTotal: [
          { required: true, message: '成绩总分不能为空', trigger: 'blur' }
        ],
        userId: [
          { required: true, message: '用户不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'resultTotal', display_name: '成绩总分' }
      ]
    }
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
