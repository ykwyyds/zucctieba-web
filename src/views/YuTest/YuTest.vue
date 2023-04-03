<template>
  <div class="comment-section">
    <div class="comment-list">
      <comment-item
        v-for="(comment, index) in comments"
        :key="comment.id"
        :comment="comment"
        @toggle-reply="toggleReply(index)"
      />
    </div>
  </div>
</template>

<script>
import CommentItem from './CommentItem.vue';

export default {
  name: 'YuTest',
  components: {
    CommentItem
  },
  data() {
    return {
      comments: [
        {
          id: 1,
          author: 'Alice',
          timestamp: '2022-04-05 10:30',
          content: '这是一条评论。',
          replies: [
            {
              id: 2,
              author: 'Bob',
              timestamp: '2022-04-05 10:35',
              content: '谢谢你的评论！'
            },
            {
              id: 3,
              author: 'Charlie',
              timestamp: '2022-04-05 10:40',
              content: '我也来凑个热闹。'
            }
          ]
        },
        {
          id: 4,
          author: 'David',
          timestamp: '2022-04-05 11:00',
          content: '这是另一条评论。',
          replies: []
        }
      ],
      showReply: []
    };
  },
  methods: {
    toggleReply(index) {
      this.$set(this.showReply, index, !this.showReply[index]);
    }
  }
};
</script>

<style>
.comment-section {
  margin-top: 16px;
}

.comment-list {
  margin-top: 16px;
}

.reply-list {
  margin-top: 8px;
  margin-left: 16px;
}
</style>
