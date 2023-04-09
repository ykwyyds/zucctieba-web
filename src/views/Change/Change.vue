<template>
  <div class="profile">
    <el-row>
      <el-col :xs="{ span: 24 }" :sm="{ span: 12, offset: 6 }" :lg="{ span: 8, offset: 8 }">
        <el-card class="card">
          <div slot="header" class="card-header">
            <p>编辑个人资料</p>
          </div>
          <div class="avatar-wrapper">
            <el-upload
              class="avatar-uploader"
              action="https://jsonplaceholder.typicode.com/posts/"
              :show-file-list="true"
              :on-success="handleAvatarSuccess"
              :before-upload="beforeAvatarUpload">
              <img v-if="imageUrl" :src="imageUrl" class="avatar">
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </div>
          <el-form ref="form" :model="form" label-width="100px" class="form">
            <el-form-item label="用户名">
              <el-input v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item label="性别">
              <el-radio v-model="radio" label="1">男<i class="el-icon-male"></i></el-radio>
              <el-radio v-model="radio" label="2">女<i class="el-icon-female"></i></el-radio>
            </el-form-item>
            <el-form-item label="自我介绍">
              <el-input v-model="form.bio"></el-input>
            </el-form-item>
            <el-form-item label="地区">
              <el-input v-model="form.location"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm">保存</el-button>
              <el-button @click="resetForm">重置</el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </el-col>
    </el-row>
  </div>

</template>

<script>
import { mapState } from 'vuex';

export default {
  name: 'Change',
  data() {
    return {
      radio: '0',
      form: {
        username: '',
        bio: '',
        location: '',
        avatar: '',
      },
      imageUrl: '',
      uploadUrl: '/api/upload',
      headers: {
        Authorization: 'Bearer ' + localStorage.getItem('token'),
      },
    };
  },
  computed: {
    ...mapState(['user']),
  },
  mounted() {
    this.form.username = this.user.username;
    this.form.bio = this.user.bio;
    this.form.location = this.user.location;
    this.form.website = this.user.website;
    this.imageUrl = this.user.avatar;
  },
  methods: {
    resetForm(formName) {
      this.form.username = [];
      this.form.bio = [];
      this.form.location = [];
      this.form.website = [];
      this.radio = '0';
      this.$message.success('重置成功');
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === 'image/jpeg';
      const isPNG = file.type === 'image/png';
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG && !isPNG) {
        this.$message.error('只能上传 JPG/PNG 格式的图片');
        return false;
      }
      if (!isLt2M) {
        this.$message.error('上传图片大小不能超过 2MB');
        return false;
      }
      return true;
    },
    handleAvatarSuccess(response) {
      this.imageUrl = URL.createObjectURL(response.file);
      this.form.avatar = response.url;
    },
    submitForm() {
      this.$refs.form.validate(valid => {
        if (valid) {
          const {username, bio, location, website, avatar} = this.form;
          // 发送请求将表单数据保存到后端数据库中
          this.$http.post('/api/profile', {
            username,
            bio,
            location,
            website,
            avatar,
          }, {
            headers: this.headers,
          })
            .then(response => {
              // 更新本地 Vuex 状态
              this.$store.commit('setUser', response.data);
              this.$message.success('保存成功');
            })
            .catch(error => {
              this.$message.error('保存失败');
              console.log(error);
            });
        } else {
          this.$message.error('表单数据无效');
        }
      },)
    }
  }
}

</script>

<style scoped>
.profile {
  margin-top: 50px;
}
.card-header {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  padding: 10px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #e6e6e6;
}
.avatar-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 30px;
}
.avatar-uploader {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 1px dashed #ccc;
  overflow: hidden;
}
.avatar {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #999;
}
.form {
  margin-top: 30px;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409EFF;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>
