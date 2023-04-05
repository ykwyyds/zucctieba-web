<template>
  <div>
    <h1 class="title">增加、修改课表</h1>
    <el-divider></el-divider>
    <div class="classschedule">
      <!-- 课程表列表 -->
      <el-table :data="courseList" style="width: 100%">
        <el-table-column prop="courseName" label="课程名称"></el-table-column>
        <el-table-column prop="teacher" label="授课教师"></el-table-column>
        <el-table-column prop="time" label="上课时间"></el-table-column>
        <el-table-column prop="location" label="上课地点"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button type="primary" size="mini" @click="editCourse(scope.row)">编辑</el-button>
            <el-button type="danger" size="mini" @click="deleteCourse(scope.$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>

      <!-- 编辑课程表的对话框 -->
      <el-dialog title="编辑课程" :visible.sync="editDialogVisible">
        <el-form ref="editForm" :model="editForm" label-width="80px">
          <el-form-item label="课程名称">
            <el-input v-model="editForm.courseName"></el-input>
          </el-form-item>
          <el-form-item label="授课教师">
            <el-input v-model="editForm.teacher"></el-input>
          </el-form-item>
          <el-form-item label="上课时间">
            <el-input v-model="editForm.time"></el-input>
          </el-form-item>
          <el-form-item label="上课地点">
            <el-input v-model="editForm.location"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="editDialogVisible = false">取消</el-button>
          <el-button type="primary" @click="saveEdit">保存</el-button>
        </div>
      </el-dialog>
      <!-- 增加课程表 -->
      <p></p>
      <el-form ref="form" :model="form" label-width="80px" style="margin-bottom: 20px;">
        <el-form-item label="课程名称">
          <el-input v-model="form.courseName"></el-input>
        </el-form-item>
        <el-form-item label="授课教师">
          <el-input v-model="form.teacher"></el-input>
        </el-form-item>
        <el-form-item label="上课时间">
          <el-input v-model="form.time"></el-input>
        </el-form-item>
        <el-form-item label="上课地点">
          <el-input v-model="form.location"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="addCourse">增加课程</el-button>
        </el-form-item>
      </el-form>

      <!-- 查询课程表 -->
      <el-form label-width="80px" style="margin-bottom: 20px;">
        <el-form-item label="课程名称">
          <el-input v-model="search.courseName"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="searchCourse">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  name: 'ClassSchedule',
  data() {
    return {
      form: {
        courseName: "",
        teacher: "",
        time: "",
        location: "",
      },
      search: {
        courseName: "",
      },
      courseList: [
        {
          courseName: "数学",
          teacher: "张三",
          time: "周一 1-2节",
          location: "教学楼201",
        },
        {
          courseName: "英语",
          teacher: "李四",
          time: "周二 3-4节",
          location: "教学楼202",
        },
      ],
      editDialogVisible: false,
      editForm: {
        courseName: "",
        teacher: "",
        time: "",
        location: "",
      },
      editIndex: -1,
    };
  },
  methods: {
    addCourse() {
      this.courseList.push(this.form);
      this.form = {
        courseName: "",
        teacher: "",
        time: "",
        location: "",
      };
      this.$refs.form.resetFields();
    },
    searchCourse() {
      if (!this.search.courseName.trim()) {
        this.courseList = [
          {
            courseName: "数学",
            teacher: "张三",
            time: "周一 1-2节",
            location: "教学楼201",
          },
          {
            courseName: "英语",
            teacher: "李四",
            time: "周二 3-4节",
            location: "教学楼202",
          },
        ];
        return;
      }

      // 根据输入框中的内容过滤课程表
      const filterStr = this.search.courseName.trim().toLowerCase();
      this.courseList = this.courseList.filter(
        (course) =>
          course.courseName.toLowerCase().indexOf(filterStr) !== -1 ||
          course.teacher.toLowerCase().indexOf(filterStr) !== -1 ||
          course.time.toLowerCase().indexOf(filterStr) !== -1 ||
          course.location.toLowerCase().indexOf(filterStr) !== -1
      );
    },
    deleteCourse(index) {
      this.courseList.splice(index, 1);
    },
    editCourse(row) {
      this.editIndex = this.courseList.indexOf(row);
      this.editForm = Object.assign({}, row);
      this.editDialogVisible = true;
    },
    saveEdit() {
      this.courseList.splice(this.editIndex, 1, this.editForm);
      this.editDialogVisible = false;
    },
  },
};
</script>

<style>
.title{
  margin-left: 10px;
}
.classschedule{
  margin-left: 10px;
}
</style>
