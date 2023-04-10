<template>
  <div class="tieguan">
    <h1 class="title">查询、删除帖子</h1>
    <el-divider></el-divider>
    <el-table :data="postList" style="width: 100%">
      <el-table-column prop="time" label="时间"></el-table-column>
      <el-table-column prop="username" label="用户名"></el-table-column>
      <el-table-column prop="content" label="内容"></el-table-column>
      <el-table-column label="操作">
        <template v-slot="scope">
          <el-button type="danger" size="mini" @click="deletePost(scope.$index)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-input class="search-bar" v-model="searchText" placeholder="请输入搜索内容"></el-input>
    <el-button type="primary" @click="searchPost">搜索</el-button>
  </div>
</template>

<script>
export default {
  name: 'TieGuan',
  data() {
    return {
      searchText: "",
      postList: [
        { time: "2023-04-01 09:30", username: "user1", content: "这是第一篇帖子" },
        { time: "2023-04-02 14:20", username: "user2", content: "这是第二篇帖子" },
        { time: "2023-04-03 21:10", username: "user3", content: "这是第三篇帖子" },
        { time: "2023-04-04 08:50", username: "user4", content: "这是第四篇帖子" },
      ],
    };
  },
  methods: {
    searchPost() {
      if (!this.searchText) {
        this.$message.error("搜索内容不能为空");
        return;
      }
      const result = this.postList.filter(
        (item) =>
          item.time.includes(this.searchText) ||
          item.username.includes(this.searchText) ||
          item.content.includes(this.searchText)
      );
      this.postList = result;
    },
    deletePost(index) {
      this.postList.splice(index, 1);
      this.$message.success("删除成功");
    },
  },
};
</script>

<style>
.search-bar {
  align-items: center;
  justify-content: center;
  margin-top: 10px;
  width: 75.5%;
}
.tieguan{
  margin-left: 10px;
}
</style>
