<template>
  <div class="post">
    <div class="post-image">
      <!-- 发帖图片 -->
      <img :src="post.imageUrl" alt="Post Image" v-bind:style="{ width: '50px', height: '50px' }">
    </div>
    <div class="post-info">
      <!-- 发帖信息 -->
      <h2>{{ post.title }}</h2>
      <p>{{ post.description }}</p>
      <div class="post-actions">
        <!-- 点赞按钮 -->
        <button @click="likePost">
          {{ post.likes }} {{ post.likes === 1 ? 'like' : 'likes' }}
        </button>
        <!-- 收藏按钮 -->
        <button @click="favoritePost">
          {{ post.favorites }} {{ post.favorites === 1 ? 'favorite' : 'favorites' }}
        </button>
        <!-- 分享按钮 -->
        <button @click="sharePost">
          Share
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'TieBa',
  data() {
    return {
      post: {
        title: 'My Post',
        description: 'This is a description of my post.',
        imageUrl: 'http://h.hiphotos.baidu.com/image/pic/item/7c1ed21b0ef41bd5f2c2a9e953da81cb39db3d1d.jpg',
        likes: 0,
        favorites: 0
      }
    }
  },
  methods: {
    likePost() {
      this.post.likes++
    },
    favoritePost() {
      this.post.favorites++
    },
    sharePost() {
      // 实现分享功能
    }
  }
}
</script>

<style>
.post {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.post-image img {
  max-width: 100%;
}

.post-info {
  margin: 20px;
}

.post-actions button {
  margin-right: 10px;
}
</style>
