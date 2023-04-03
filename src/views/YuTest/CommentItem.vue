<template>
  <div class="comment-item">
    <div class="comment-header">
      <span class="comment-author">{{ comment.author }}</span>
      <span class="comment-timestamp">{{ comment.timestamp }}</span>
    </div>
    <div class="comment-content">{{ comment.content }}</div>
    <div class="comment-actions">
      <span class="comment-action" @click="handleReplyClick">回复</span>
    </div>
    <div class="reply-list" v-if="showReply">
      <comment-item
        v-for="(reply, index) in comment.replies"
        :key="reply.id"
        :comment="reply"
      />
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
  },
  data() {
    return {
      showReply: false
    };
  },
  methods: {
    handleReplyClick() {
      this.showReply = !this.showReply;
      if (this.showReply) {
        this.$emit('toggle-reply');
      }
    }
  }
};
</script>

<style>
.comment-item {
  margin-bottom: 16px;
}

.comment-header {
  display: flex;
  align-items: center;
  font-weight: bold;
  margin-bottom: 4px;
}

.comment-author {
  margin-right: 8px;
}

.comment-timestamp {
  font-size: 12px;
  color: #666;
}

.comment-content {
  margin-bottom: 4px;
}

.comment-actions {
  display: flex;
}

.comment-action {
  margin-right: 8px;
  color: #666;
  cursor: pointer;
}

.reply-list {
  margin-top: 8px;
  margin-left: 16px;
}
</style>
