<template>
  <div class="profile-container">
    <!-- 用户资料 -->
    <div class="profile-header">
      <!--      <el-avatar :src="userInfo.avatarUrl"></el-avatar>-->
      <el-avatar shape="square" :size="80" :src="userInfo.avatarUrl"></el-avatar>
      <div class="profile-info">
        <h2>{{userInfo.nickname}}</h2>
        <p class="username">@{{ userInfo.username }}</p>
        <p>{{userInfo.bio}}</p>
      </div>
    </div>

    <!-- 微博列表 -->
    <el-card class="weibo-list">
      <div slot="header">
        <h3>微博列表</h3>
      </div>
      <div v-for="(weibo, index) in weiboList" :key="index">
        <div class="weibo-content">
          <p>{{weibo.content}}</p>
          <div v-if="weibo.images.length > 0" class="weibo-images">
            <el-image v-for="(image, index) in weibo.images" :key="index" :src="image"></el-image>
          </div>
        </div>
        <div class="weibo-footer">
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
                <el-form-item label="Comment">
                  <el-input
                    type="textarea"
                    :rows="2"
                    placeholder="说点什么吧"
                    v-model="newComment">
                  </el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="submitComment">发 布</el-button>
              </div>
            </el-dialog>
          </div>
          <div class="comments">
            <!-- 评论列表 -->
            <div class="comment-list">
              <CommentItem v-for="comment in comments" :key="comment.id" :comment="comment" />
            </div>
          </div>
        </div>
      </div>
    </el-card>

    <!-- 关注列表 -->
    <el-card class="follow-list">
      <div slot="header">
        <h3>关注列表</h3>
      </div>
      <div v-for="(user, index) in followList" :key="index" class="guanzhu">
        <el-avatar shape="square" :size="50" :src="user.avatarUrl"></el-avatar>
        <span>{{user.nickname}}</span>
      </div>
    </el-card>
  </div>
</template>

<script>
import CommentItem from './CommentItem.vue'
export default {
  name: 'SelfCenter',
  props: {},
  components: { CommentItem },
  data() {
    return {
      likes: '1',
      liked: false, // 初始状态为未点赞
      stared: false, //初始状态未收藏
      dzcount: 50, // 点赞计数器
      sccount: 20, // 收藏计数器
      dialogFormVisible: false,
      newComment: '',
      userInfo: {
        nickname: '张三',
        username: 'Zhang',
        avatarUrl: 'http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg',
        bio: '一个热爱编程的程序员'
      },
      comments: [
        {
          id: 1,
          author: '张三',
          content: '小姐姐太美了！',
          timestamp: '2023/04/01 12:34:56'
        },
      ],
      weiboList: [
        {
          content: '这是一条微博内容',
          images: ['http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg'],
          reposts: 10,
          comments: 20,
          likes: 30
        },
      ],
      followList: [
        {
          nickname: '李四',
          avatarUrl: 'https://example.com/avatar2.png'
        },
        {
          nickname: '王五',
          avatarUrl: 'https://example.com/avatar3.png'
        }
      ]
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
    },
    submitComment() {
      if (this.newComment.trim() === '') {
        return
      }

      const newId = this.comments.length + 1
      const newComment = {
        id: newId,
        author: '匿名用户',
        content: this.newComment,
        timestamp: new Date().toLocaleString()
      }
      this.comments.push(newComment)
      this.newComment = ''
    }
  },
}
</script>

<style scoped>
.profile-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.profile-header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.profile-info {
  margin-left: 20px;
}

.weibo-list {
  width: 600px;
  margin-bottom: 20px;
}

.weibo-content {
  margin-bottom: 10px;
}
.likes, .comments, .date {
  display: flex;
  align-items: center;
}

.likes i, .comments i {
  margin-right: 5px;
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
.guanzhu{
  line-height: 65px;
}
</style>
