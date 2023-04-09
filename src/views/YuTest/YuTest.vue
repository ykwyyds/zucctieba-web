<template>
<el-row>
<el-col :span="6">
  <el-card>
    <!-- 第一个 el-card 的内容 -->
    haha
  </el-card>
</el-col>
<el-col :span="6">
  <el-card>
    <!-- 第二个 el-card 的内容 -->
    yeye
  </el-card>
</el-col>
</el-row>
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
        avatarUrl: 'https://picsum.photos/60',
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
