<template>
  <div class="post">
    <div class="post-header">
        <!--用户信息-->
      <div class="meme">
        <el-avatar src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"></el-avatar>
      </div>
      <div class="user-info">
        <h2>{{ user.name }}</h2>
        <p class="username">@{{ user.username }}</p>
      </div>
    </div>
    <div class="post-content">
      <!--信息本体-->
      <p>{{ content }}</p>
      <!--图片-->
      <img v-if="image" class="post-image" v-bind:src="image" alt="Post Image" v-bind:style="{ width: '50px', height: '50px' }" />
    </div>
    <div class="post-footer">
      <div class="likes">
        <!--        生成点赞、收藏按钮        -->
        <button @click="toggleLike" :class="{ liked: liked }">
          <img src="@/assets/images/unlike.png" v-if="liked" />
          <img src="@/assets/images/like.png" v-else />
          <span class="count" v-if="dzcount">{{ dzcount }}</span>
        </button>
        <button @click="togglestar" :class="{ liked: stared }">
          <i class="el-icon-star-on" v-if="stared" v-bind:style="{ width: '25px', height: '25px' }"></i>
          <i class="el-icon-star-off" v-else v-bind:style="{ width: '25px', height: '25px' }"></i>
          <span class="count" v-if="sccount">{{ sccount }}</span>
        </button>
        <el-button  @click="dialogFormVisible = true" >评论</el-button>
        <!--        评论按钮         -->
        <el-dialog title="发表评论" :visible.sync="dialogFormVisible">
          <el-form ref="commentForm" :model="comment" label-width="80px">
            <el-form-item label="Name">
              <el-input v-model="comment.name"></el-input>
            </el-form-item>
            <el-form-item label="Comment">
              <el-input
                type="textarea"
                :rows="2"
                placeholder="请输入内容"
                v-model="comment.content">
              </el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="addComment">发 布</el-button>
          </div>
        </el-dialog>

      </div>
    </div>
    <div class="comments">
      <!-- 评论列表 -->
      <el-card v-for="(comment, index) in comments" :key="index">
        <p>{{ comment.name }}:  {{ comment.content }}</p>
        <p>时间: {{ comment.time }}</p>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'YuTest',
  props: {},
  data() {
    return {
      // eslint-disable-next-line vue/no-dupe-keys
      user: {
        name: 'Yumou',
        username: 'litterfish'
      },
      content: 'xxxxxxxxxxxxx',
      image: 'http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg',
      likes: '1',
      liked: false, // 初始状态为未点赞
      stared: false, //初始状态未收藏
      dzcount: 1, // 点赞计数器
      sccount: 1, // 收藏计数器
      dialogFormVisible: false,
      comment: {
        name: 'Yu',
        content: ''
      },
      comments: []
    }
  },
  methods: {
    toggleLike() {
      // 点赞操作
      this.liked = !this.liked; // 点赞状态取反
      if (this.liked) {
        this.dzcount++; // 点赞计数器加1
      } else {
        this.dzcount--; // 取消点赞计数器减1
      }
    },
    togglestar() {
      this.stared = !this.stared; // 点赞状态取反
      if (this.stared) {
        this.sccount++; // 点赞计数器加1
      } else {
        this.sccount--; // 取消点赞计数器减1
      }
    },
    addComment() {
      // validate comment form
      this.$refs.commentForm.validate((valid) => {
        this.dialogFormVisible = false;
        this.comment.time = new Date().toLocaleString();
        if (valid) {
          // add comment to comments array
          this.comments.push({...this.comment});
          // clear comment form
          this.comment.name = '';
          this.comment.content = '';
        }
      });
    }
  },
}

</script>

<style>
.post {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin: 10px 0;
}

.post-header {
  display: flex;
  align-items: center;
}

.avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  margin-right: 10px;
}

.user-info h2 {
  font-size: 18px;
  margin: 0;
}

.username {
  color: #aaa;
  margin: 0;
}

.post-content {
  margin: 10px 0;
}

.post-content p {
  margin: 0;
}

.post-image {
  max-width: 100%;
  margin-top: 10px;
}

.post-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.likes, .comments, .date {
  display: flex;
  align-items: center;
}

.likes i, .comments i {
  margin-right: 5px;
}

.date span {
  color: #aaa;
}
.like-container {
  position: fixed;
  bottom: 20px;
  right: 20px;
  display: flex;
  align-items: center;
  background-color: white;
  border-radius: 5px;
  padding: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.like-container i {
  color: #3b5998;
  font-size: 20px;
  margin-right: 5px;
}

.like-container .like-count {
  font-size: 16px;
  font-weight: bold;
  color: #333;
}
button.liked {
  /*background-color: #ffdd00; !* 将点赞按钮的背景颜色改为黄色 *!*/
}
button img {
  width: 24px;
  height: 24px;
}
span.count {
  margin-left: 5px; /* 数字和图标之间添加一些间距 */
  font-size: 12px;
  color: #666;
}
</style>
