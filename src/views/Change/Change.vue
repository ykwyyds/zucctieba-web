<template>
  <div class="personal-center">
    <!--    左侧框框    -->
    <el-card class="personal-info">
      <p>个人信息</p>
      <el-divider></el-divider>
      <div class="avatar">
        <el-image :src="userInfo.avatar" fit="cover"></el-image>
        <input class="avatar-upload" type="file" accept="image/*" @change="handleAvatarUpload">
      </div>
      <div class="personal-details">
        <p>用户名：{{userInfo.name}}</p>
        <p>用户ID：{{userInfo.username}}</p>
        <p>个性签名：{{userInfo.bio}}</p>
        <p>IP所属地：{{userInfo.location}}</p>
      </div>
    </el-card>
    <!--    右侧框框    -->
    <el-card class="personal-action">
      <p>修改信息</p>
      <el-divider></el-divider>
      <el-form ref="userInfoForm" :model="userInfo" :rules="rules" label-width="80px" class="personal-form">
        <el-form-item label="昵称" prop="name">
          <el-input v-model="userInfo.name" placeholder="请输入昵称"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="gender">
          <el-radio-group v-model="userInfo.gender">
            <el-radio label="男"></el-radio>
            <el-radio label="女"></el-radio>
            <el-radio label="保密"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="个性签名" prop="signature">
          <el-input v-model="userInfo.bio" placeholder="来说些什么吧"></el-input>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="userInfo.phone" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item label="地区" prop="location">
          <el-input v-model="userInfo.location" placeholder="请输入地区"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">保存</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'Change',
  data() {
    return {
      userInfo: {
        name: '虞某',
        username : 'litterfish123',
        avatar: 'https://picsum.photos/200',
        gender: '男',
        phone: '13185858859',
        bio: '我真的不会啊！',
        location: '浙江'
      },
      rules: {
        name: [
          { required: true, message: '请输入昵称', trigger: 'blur' },
          { min: 2, max: 10, message: '昵称长度在 2 到 10 个字符之间', trigger: 'blur' }
        ],
        gender: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ],
        phone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { pattern: /^1[34578]\d{9}$/, message: '手机号格式不正确', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    handleAvatarClick() {
      document.querySelector('.avatar-upload').click()
    },
    handleAvatarUpload(e) {
      const file = e.target.files[0]
      if (!file) return
      if (!/\.(jpg|jpeg|png|gif)$/i.test(file.name)) {
        this.$message.error('上传头像图片格式不正确')
        return
      }
      if (file.size > 2 * 1024 * 1024) {
        this.$message.error('上传头像图片大小不能超过 2MB')
        return
      }
      const reader = new FileReader()
      reader.onload = () => {
        this.userInfo.avatar = reader.result
      }
      reader.readAsDataURL(file)
    },
    submitForm() {
      this.$refs.userInfoForm.validate(valid => {
        if (valid) {
          this.$message.success('保存成功')
        } else {
          this.$message.error('表单验证失败')
        }
      })
    }
  }
}
</script>

<style scoped>
.personal-center {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}
.personal-info {
  margin-left: 20px;
  width: 300px;
}
.personal-action {
  flex-grow: 1;
  margin-left: 20px;
  margin-right: 20px;
}
.per-inside{
  display: flex;
  text-align: center;
  margin-top: 3px;
  font-size: 16px;
}
.avatar {
  position: relative;
  /*height: 250px;*/
  /*overflow: hidden;*/
  text-align: center;
  margin-top: 20px;
  cursor: pointer;
}
.avatar-upload {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  cursor: pointer;
}
.avatar-upload-button {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}
.personal-details {
  text-align: center;
  margin-top: 3px;
  font-size: 16px;
}
.personal-details h2 {
  margin-bottom: 10px;
}
.personal-form {
  width: 400px;
  margin: 40px auto;
}
</style>
