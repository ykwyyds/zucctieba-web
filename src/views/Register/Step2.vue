<template>
  <div>
  <h1 style="margin-left: 10px;">我问你答</h1>
  <el-divider></el-divider>
  <el-card class="question-card">
    <div slot="header" class="question-header">{{ questionNumber }}. {{ currentQuestion.question }}</div>
    <el-radio-group v-model="selectedAnswer" class="options">
      <el-radio v-for="option in currentQuestion.options" :key="option" :label="option">{{ option }}</el-radio>
    </el-radio-group>
    <el-button v-if="showNextButton" type="primary" class="next-button" @click="nextQuestion">下一题</el-button>
  </el-card>
  </div>
</template>

<script>
export default {
  name: 'Step2',
  data() {
    return {
      allQuestions: [
        {
          question: '北校那个宿舍是男生宿舍？',
          options: ['问源楼', '思睿楼', '求真楼', '明学楼'],
          answer: '求真楼'
        },
        {
          question: '大学生背的最多的英语单词是？',
          options: ['nice', 'good', 'apple', 'abandon'],
          answer: 'abandon'
        },
        {
          question: '浙大城市学院成立于什么时候？',
          options: ['2000', '1999', '1897', '1898'],
          answer: '1999'
        },
        {
          question: '浙大城市学院的中外合资学院简称什么？',
          options: ['UW学院', 'UV学院', 'UAV学院', 'UAW学院'],
          answer: 'UW学院'
        },
        {
          question: '学校的图书馆又被称为？',
          options: ['妙脆角', '薯片', '藏书楼', '藏书阁'],
          answer: '妙脆角'
        }
      ],
      questions: [],
      currentQuestionIndex: 0,
      selectedAnswer: '',
      showNextButton: false
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    },
    questionNumber() {
      return this.currentQuestionIndex + 1;
    }
  },
  methods: {
    nextQuestion() {
      if (this.selectedAnswer === this.currentQuestion.answer) {
        if (this.currentQuestionIndex === this.questions.length - 1) {
          this.$router.push('/register/Step3');
          alert('你已完成所有题目！');
        } else {
          this.currentQuestionIndex++;
          this.selectedAnswer = '';
          this.showNextButton = false;
        }
      } else {
        this.$router.push('/register/Step1');
        alert('选择错误，请重新做题！');
      }
    },
    getRandomQuestions() {
      const shuffledQuestions = this.allQuestions.sort(() => 0.5 - Math.random());
      this.questions = shuffledQuestions.slice(0, 3);
    }
  },
  watch: {
    selectedAnswer() {
      this.showNextButton = true;
    }
  },
  mounted() {
    this.getRandomQuestions();
  }
};
</script>

<style scoped>
.question-card {
  max-width: 500px;
  margin: 0 auto;
  margin-top: 50px;
}

.question-header {
  font-size: 18px;
  font-weight: bold;
  text-align: center;
}

.options {
  margin-top: 20px;
}

.next-button {
  margin-top: 20px;
  width: 100%;
}
</style>
