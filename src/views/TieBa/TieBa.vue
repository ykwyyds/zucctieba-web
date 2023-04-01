<template>
  <div class="post">
    <div class="post-header">
<!--      <img class="avatar" v-bind:src="user.avatar" alt="User Avatar" />-->
      <div class="user-info">
        <h2>{{ user.name }}</h2>
        <p class="username">@{{ user.username }}</p>
      </div>
    </div>
    <div class="post-content">
      <p>{{ content }}</p>//
      <img v-if="image" class="post-image" v-bind:src="image" alt="Post Image" v-bind:style="{ width: '50px', height: '50px' }" />
    </div>
    <div class="post-footer">
      <div class="likes">
<!--        <i class="fa fa-heart"></i>-->
<!--        <span>{{ likes }}</span>-->
        <button @click="toggleLike" :class="{ liked: liked }">
          <img src="@/assets/images/like.png" v-if="liked" />
          <img src="@/assets/images/unlike.png" v-else />
          </button>
      </div>
<!--      <div class="comments">-->
<!--        <i class="fa fa-comment"></i>-->
<!--        <span>{{ comments }}</span>-->
<!--      </div>-->
<!--      <div class="date">-->
<!--        <span>{{ date }}</span>-->
<!--      </div>-->
    </div>
  </div>
</template>

<script>
export default {
  name:'TieBa',
  props: {
    // user: {
    //   type: Object,
    //   required: true
    // },
    // content: {
    //   type: String,
    //   required: true
    // },
    // image: {
    //   type: String,
    //   default: null
    // },
    // likes: {
    //   type: Number,
    //   default: 0
    // },
    // comments: {
    //   type: Number,
    //   default: 0
    // },
    // date: {
    //   type: String,
    //   required: true
    // }
  },
  data(){
    return{
      // eslint-disable-next-line vue/no-dupe-keys
      user: {
        name:'s',
        username:'s'
      }
      ,content: 'xxxxxxxxxxxxx',
      image:'http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg',
      likes:'1',
      liked: false, // 初始状态为未点赞
    }
  },
  methods: {
    toggleLike() {
      // 点赞操作
      this.liked = !this.liked; // 点赞状态取反
    },
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
</style>
