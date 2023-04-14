<template>
  <div class="comment-item">
    <div class="comment-header">
      <el-avatar shape="square" :size="30" :src="comment.avatar" />
      <span class="comment-author">{{ comment.author }}</span>
      <span class="comment-timestamp">{{ comment.timestamp }}</span>
    </div>
    <div class="comment-content">
      {{ comment.content }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'CommentItem',
  props: {
    comment: {
      type: Object,
      required: true
    }
  }
}
</script>

<style>
.comment-item {
  margin-bottom: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 16px;
}

.comment-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.comment-author {
  font-weight: bold;
  margin-right: 8px;
}

.comment-timestamp {
  color: #999;
}

.comment-content {
  font-size: 14px;
}
</style>
