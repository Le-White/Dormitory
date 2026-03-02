<template>
  <div>
    <el-breadcrumb separator-icon="ArrowRight" style="margin: 16px">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>学生信息</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card style="margin: 15px; min-height: calc(100vh - 111px)">
      <div>
        <!--    功能区-->
        <div style="margin: 10px 0">
          <!--    搜索区-->
          <div style="margin: 10px 0">
            <el-input v-model="search" clearable placeholder="请输入姓名" prefix-icon="Search"
                      style="width: 20%"/>
            <el-button icon="Search" style="margin-left: 5px" type="primary" @click="load"></el-button>
            <el-button icon="refresh-left" style="margin-left: 10px" type="default" @click="reset"></el-button>
            <div style="float: right">
              <el-tooltip content="添加" placement="top">
                <el-button icon="plus" style="width: 50px" type="primary" @click="add"></el-button>
              </el-tooltip>
              <el-tooltip content="导出Excel" placement="top">
                <el-button 
                  icon="download" 
                  style="width: 50px; margin-left: 10px" 
                  type="success" 
                  @click="exportToExcel"
                ></el-button>
              </el-tooltip>
            </div>
          </div>
        </div>
        <!--    表格-->
        <el-table
          v-loading="loading"
          :data="tableData"
          border
          max-height="705"
          style="width: 100%"
        >
          <el-table-column label="#" type="index"/>
          <el-table-column label="学号" prop="username" sortable/>
          <el-table-column label="姓名" prop="name"/>
          <el-table-column
              :filter-method="filterTag"
              :filters="[
              { text: '男', value: '男' },
              { text: '女', value: '女' },
            ]"
              filter-placement="bottom-end"
              label="性别"
              prop="gender"
          />
          <el-table-column label="年龄" prop="age" sortable/>
          <el-table-column label="手机号" prop="phoneNum"/>
          <el-table-column :show-overflow-tooltip="true" label="邮箱" prop="email"/>
          <!--      操作栏-->
          <el-table-column label="操作" width="130px">
            <template #default="scope">
              <el-button icon="Edit" type="primary" @click="handleEdit(scope.row)"></el-button>
              <el-popconfirm title="确认删除？" @confirm="handleDelete(scope.row.username)">
                <template #reference>
                  <el-button icon="Delete" type="danger"></el-button>
                </template>
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>
        <!--分页-->
        <div style="margin: 10px 0">
          <el-pagination
              :current-page="currentPage"
              :page-size="pageSize"
              :page-sizes="[10, 20,30,40,50]"
              :total="total"
              layout="total, sizes, prev, pager, next, jumper"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
          >
          </el-pagination>
        </div>
        <div>
          <!--      弹窗-->
          <el-dialog v-model="dialogVisible" title="操作" width="30%" @close="cancel">
            <el-form ref="form" :model="form" :rules="rules" label-width="120px">
              <el-form-item label="学号" prop="username">
                <el-input v-model="form.username" :disabled="judgeAddOrEdit" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="密码" prop="password">
                <el-input v-model="form.password" :disabled="disabled" :show-password="showpassword"
                          style="width: 80%"></el-input>
                <el-tooltip content="修改密码" placement="right">
                  <el-icon :style="editDisplay" size="large" style="margin-left: 5px; cursor: pointer"
                           @click="EditPass">
                    <edit/>
                  </el-icon>
                </el-tooltip>
              </el-form-item>
              <el-form-item :style="display" label="确认密码" prop="checkPass">
                <el-input v-model="form.checkPass" :show-password="showpassword" style="width: 80%"
                ></el-input>
              </el-form-item>
              <el-form-item label="姓名" prop="name">
                <el-input v-model="form.name" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="年龄" prop="age">
                <el-input v-model.number="form.age" style="width: 80%"
                ></el-input>
              </el-form-item>
              <el-form-item label="性别" prop="gender">
                <el-radio v-model="form.gender" label="男">男</el-radio>
                <el-radio v-model="form.gender" label="女">女</el-radio>
              </el-form-item>
              <el-form-item label="手机号" prop="phoneNum">
                <el-input v-model.number="form.phoneNum" style="width: 80%"
                ></el-input>
              </el-form-item>
              <el-form-item label="邮箱地址" prop="email">
                <el-input v-model="form.email" style="width: 80%"></el-input>
              </el-form-item>
            </el-form>
            <template #footer>
              <span class="dialog-footer">
                <el-button @click="cancel">取 消</el-button>
                <el-button type="primary" @click="save">确 定</el-button>
              </span>
            </template>
          </el-dialog>
        </div>
        <!-- 添加预览对话框 -->
        <el-dialog
          v-model="previewVisible"
          title="导出预览"
          width="80%"
        >
          <el-table :data="exportData" border style="width: 100%">
            <el-table-column label="账号" prop="账号"/>
            <el-table-column label="姓名" prop="姓名"/>
            <el-table-column label="性别" prop="性别"/>
            <el-table-column label="年龄" prop="年龄"/>
            <el-table-column label="手机号" prop="手机号"/>
            <el-table-column label="邮箱" prop="邮箱"/>
          </el-table>
          <template #footer>
            <div class="dialog-footer">
              <el-button @click="previewVisible = false">取消</el-button>
              <el-button type="primary" @click="confirmExport">确认导出</el-button>
            </div>
          </template>
        </el-dialog>
      </div>
    </el-card>
  </div>
</template>
<script>
import { Search, RefreshLeft, Plus, Download, Edit, Delete } from '@element-plus/icons-vue'
import stuInfoJs from '@/assets/js/StuInfo.js'
import * as XLSX from 'xlsx';
import { ElMessage } from 'element-plus';

export default {
  name: "StuInfo",
  components: {
    Search,
    RefreshLeft,
    Plus,
    Download,
    Edit,
    Delete
  },
  data() {
    return {
      ...stuInfoJs.data(),  // 展开原有的data
      previewVisible: false,
      exportData: [],
      form: {
        checkPass: ''
      }
    }
  },
  created() {
    this.load();
    this.loading = true;
    setTimeout(() => {
      this.loading = false;
    }, 1000);
  },
  methods: {
    ...stuInfoJs.methods,  // 展开原有的methods
    exportToExcel() {
      // 准备导出数据
      this.exportData = this.tableData.map(item => ({
        账号: item.username || '',        // 确保字符串格式
        姓名: item.name || '',            // 确保字符串格式
        性别: item.gender || '未知',      // 添加默认值
        年龄: item.age ? Number(item.age) : 0,  // 确保数字格式
        手机号: item.phoneNum ? String(item.phoneNum) : '',  // 确保字符串格式
        邮箱: item.email || ''            // 确保字符串格式
      }));
      
      // 显示预览对话框
      this.previewVisible = true;
    },
    
    confirmExport() {
      try {
        // 创建工作簿
        const wb = XLSX.utils.book_new();
        
        // 转换数据为工作表
        const ws = XLSX.utils.json_to_sheet(this.exportData, {
          header: ["账号", "姓名", "性别", "年龄", "手机号", "邮箱"]
        });
        
        // 设置列宽
        const colWidth = [
          { wch: 15 }, // 账号
          { wch: 10 }, // 姓名
          { wch: 8 },  // 性别
          { wch: 8 },  // 年龄
          { wch: 15 }, // 手机号
          { wch: 25 }, // 邮箱
         
        ];
        ws['!cols'] = colWidth;
        
        // 添加工作表到工作簿
        XLSX.utils.book_append_sheet(wb, ws, '学生信息');
        
        // 生成文件并下载
        XLSX.writeFile(wb, '学生信息表.xlsx');
        
        // 关闭预览
        this.previewVisible = false;
        
        // 提示成功
        this.$message({
          message: '导出成功',
          type: 'success'
        });
      } catch (error) {
        console.error('Export failed:', error);
        this.$message({
          message: '导出失败，请重试',
          type: 'error'
        });
      }
    }
  }
}
</script>

<style scoped>
.dialog-footer {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  padding-top: 20px;
}

:deep(.el-dialog__body) {
  padding: 20px;
}

:deep(.el-table) {
  margin: 20px 0;
}
</style>