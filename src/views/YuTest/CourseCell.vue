<template>
  <div class="course-cell" :style="style">
    <div class="course-info">
      <div class="course-name">{{ course.name }}</div>
      <div class="course-teacher">{{ course.teacher }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CourseCell',
  props: {
    course: Object,
    gridWidth: Number,
    gridHeight: Number,
  },
  computed: {
    style() {
      return {
        gridColumn: `span ${this.course.duration}`,
        gridRow: `span ${this.course.weekDay}`,
        width: `${this.gridWidth}px`,
        height: `${this.gridHeight}px`,
      };
    },
  },
};
</script>

<style scoped>
.course-cell {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #ddd;
  box-sizing: border-box;
  overflow: hidden;
}

.course-info {
  font-size: 14px;
  line-height: 20px;
  text-align: center;
}

.course-name {
  font-weight: bold;
  margin-bottom: 6px;
}

.course-teacher {
  color: #999;
}
</style>
