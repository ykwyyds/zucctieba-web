<template>
  <div>
    <h1 class="title">学生课表</h1>
    <el-divider></el-divider>
    <el-table :data="tableData" :row-class-name="tableRowClassName">
      <el-table-column label="节次" align="center" width="60">
        <template slot-scope="scope">
          <div>{{ scope.row.time }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周一" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.monday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周二" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.tuesday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周三" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.wednesday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周四" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.thursday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周五" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.friday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周六" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.saturday }}</div>
        </template>
      </el-table-column>
      <el-table-column label="周日" align="center">
        <template slot-scope="scope">
          <div>{{ scope.row.sunday }}</div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Schedule',
  data() {
    return {
      tableData: [
        { time: "1", monday: "数学", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "2", monday: "", tuesday: "语文", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "3", monday: "", tuesday: "", wednesday: "英语", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "4", monday: "", tuesday: "", wednesday: "", thursday: "物理", friday: "", saturday: "", sunday: "" },
        { time: "5", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "化学", saturday: "", sunday: "" },
        { time: "6", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "生物" },
        { time: "7", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "8", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "9", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
        { time: "10", monday: "", tuesday: "", wednesday: "", thursday: "", friday: "", saturday: "", sunday: "" },
      ]
    }
  },
  //从后端调取数据
  mounted() {
    axios.get('/api/tabledata') // 发送 AJAX 请求
      .then(response => {
        this.tableData = response.data; // 将响应数据填充到tableData中
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
// 设置每一行的class
    tableRowClassName({row, rowIndex}) {
      return 'row-' + (rowIndex + 1)
    }
  }
}
</script>

<style scoped>
/* 样式优化 */
.el-table {
  width: 100%;
  border-collapse: collapse;
  table-layout: fixed;
}
.el-table th,
.el-table td {
  padding: 0;
  height: 50px;
  line-height: 50px;
  text-align: center;
  font-size: 14px;
  border: 1px solid #f0f0f0;
}
.el-table th {
  font-weight: bold;
  background-color: #f5f7fa;
}
.el-table td div {
  word-wrap: break-word;
  word-break: break-all;
  padding: 5px;
}
.row-odd {
  background-color: #f9fafc;
}
.title{
  text-align: center;
}
</style>
