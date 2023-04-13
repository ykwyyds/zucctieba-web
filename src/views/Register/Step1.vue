<template>
  <div>
    <h1 style="margin-left: 10px;">注册</h1>
    <el-divider></el-divider>
    <el-steps :active="1" class="step">
      <el-step title="我问你答" description="为了保证用户的真实性，我们会问你几个问题"></el-step>
      <el-step title="基本信息" description="填写基本信息"></el-step>
    </el-steps>
    <el-button style="margin-left: 45%;" @click="next">
      开始答题
    </el-button>
  </div>
</template>

<script>
export default {
  name: 'Step1',
  data() {
    return {
    };
  },
  computed: {
  },
  methods: {
    next() {
      this.$message({
        type: 'success',
        message: '开始答题'
      })
      this.$router.push('/register/Step2')
    }
  }
};
</script>

<style>
.step{
  width: 80%;
  margin-top: 20px;
  margin-bottom: 20px;
  margin-left: 10%;
}
</style>
