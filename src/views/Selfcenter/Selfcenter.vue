<template>
  <div class="profile-container">
    <!-- 用户资料 -->
    <el-card>
      <div slot="header" class="profile-info">
        <el-avatar shape="square" :size="80" :src="userInfo.avatar" />
        <h2 style="margin-left: 10px;">{{ userInfo.name }}@{{ userInfo.username }}</h2>
        <el-button type="primary" :class="{subd}" round @click="sub">
          <i v-if="subd">已关注</i>
          <i v-else>关注</i>
        </el-button>
      </div>
      <el-collapse accordion>
        <el-collapse-item>
          <template slot="title">
            更多信息<i class="header-icon el-icon-info" />
          </template>
          <div class="self-info">
            <p>性别：{{ userInfo.gender }}</p>
            <p>个性签名：{{ userInfo.bio }}</p>
            <p>IP所属地：{{ userInfo.location }}</p>
          </div>
        </el-collapse-item>
      </el-collapse>
    </el-card>
    <el-row>
      <el-col span="20">
        <!-- 帖子列表 -->
        <el-card class="self-list">
          <div slot="header" class="Tietitle">
            <h3>帖子列表</h3>
            <el-button type="danger" @click="dele">编辑帖子</el-button>
          </div>
          <div v-for="(weibo, index) in weiboList" :key="index">
            <div class="weibo-content">
              <p>{{ weibo.content }}</p>
              <div v-if="weibo.images.length > 0" class="weibo-images">
                <el-image v-for="(image, index) in weibo.images" :key="index" :src="image" />
              </div>
              <div class="weibo-footer">
                <div class="likes">
                  <!--        生成点赞、收藏按钮        -->
                  <button :class="{ liked: liked }" @click="toggleLike">
                    <img v-if="liked" src="@/assets/images/unlike.png">
                    <img v-else src="@/assets/images/like.png">
                    <span v-if="dzcount" class="count">{{ dzcount }}</span>
                  </button>
                  <button :class="{ liked: stared }" @click="togglestar">
                    <i v-if="stared" class="el-icon-star-on" :style="{ width: '25px', height: '25px' }" />
                    <i v-else class="el-icon-star-off" :style="{ width: '25px', height: '25px' }" />
                    <span v-if="sccount" class="count">{{ sccount }}</span>
                  </button>
                  <!--        评论、删除按钮         -->
                  <el-button @click="dialogFormVisible = true">评论</el-button>
                  <!--        评论按钮窗口         -->
                  <el-dialog title="发表评论" :visible.sync="dialogFormVisible">
                    <el-form ref="commentForm" :model="comment" label-width="80px">
                      <el-form-item label="Comment">
                        <el-input
                          v-model="newComment"
                          type="textarea"
                          :rows="2"
                          placeholder="说点什么吧"
                        />
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
          </div>
        </el-card>
      </el-col>
      <el-col span="4">
        <!-- 关注列表 -->
        <el-card class="follow-list">
          <div slot="header">
            <h3>关注列表</h3>
          </div>
          <div v-for="(user, index) in followList" :key="index" class="guanzhu">
            <el-avatar shape="square" :size="50" :src="user.avatar" />
            <span>{{ user.name }}@{{ user.username }}</span>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import CommentItem from './CommentItem.vue'
export default {
  name: 'SelfCenter',
  components: { CommentItem },
  props: {},
  data() {
    return {
      likes: '1',
      liked: false, // 初始状态为未点赞
      stared: false, // 初始状态未收藏
      subd: false,
      dzcount: 50, // 点赞计数器
      sccount: 20, // 收藏计数器
      dialogFormVisible: false,
      newComment: '',
      userInfo: {
        name: '张三',
        username: 'Zhang',
        avatar: 'https://picsum.photos/60',
        gender: '男',
        bio: '一个热爱编程的程序员',
        location: '浙江'
      },
      comments: [
        {
          id: 1,
          author: '张三',
          content: '小姐姐太美了！',
          timestamp: '2023/04/01 12:34:56'
        }
      ],
      weiboList: [
        {
          id: 1,
          content: '真的不会写啊QUQ',
          images: ['http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg'],
          reposts: 10,
          comments: 20,
          likes: 30
        },
        {
          id: 2,
          content: '我也不会啊',
          images: [],
          reposts: 10,
          comments: 20,
          likes: 50
        }
      ],
      followList: [
        {
          name: '李四',
          username: 'Li',
          avatar: 'https://picsum.photos/50'
        },
        {
          name: '王五',
          username: 'Wang',
          avatar: 'https://picsum.photos/40'
        }
      ]
    }
  },
  methods: {
    toggleLike() {
      // 点赞操作
      this.liked = !this.liked // 点赞状态取反
      if (this.liked) {
        this.dzcount++ // 点赞计数器加1
      } else {
        this.dzcount-- // 取消点赞计数器减1
      }
    },
    togglestar() {
      this.stared = !this.stared // 点赞状态取反
      if (this.stared) {
        this.sccount++ // 点赞计数器加1
      } else {
        this.sccount-- // 取消点赞计数器减1
      }
    },
    addComment() {
      // validate comment form
      this.$refs.commentForm.validate((valid) => {
        this.dialogFormVisible = false
        this.comment.time = new Date().toLocaleString()
        if (valid) {
          // add comment to comments array
          this.comments.push({ ...this.comment })
          // clear comment form
          this.comment.name = ''
          this.comment.content = ''
        }
      })
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
      this.dialogFormVisible = false
    },
    sub() {
      this.subd = !this.subd
      if (this.subd) {
        this.$message({
          type: 'success',
          message: '关注成功！'
        })
      } else {
        this.$confirm('此操作将取消关注, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'info',
            message: '取关成功'
          })
        }).catch(() => {
          this.$message({
            type: 'success',
            message: '感谢你的继续关注'
          })
          this.subd = !this.subd
        })
      }
    },
    dele() {
      this.$confirm('是否要去管理你的帖子?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '已成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消！'
        })
      })
    }
  }
}
</script>

<style scoped>
.profile-container {
  /*display: flex;*/
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
  align-items: center;
  width: 728px;
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.self-info{
  margin-left: 20px;
  align-items: center;
  width: 600px;
  margin-bottom: 20px;
  margin-top: 20px;
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
.Tietitle{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
