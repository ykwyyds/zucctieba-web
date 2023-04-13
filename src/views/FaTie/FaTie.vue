<template>
  <div class="post-form">
    <h1 class="title">我要发帖</h1>
    <el-form :model="form" :rules="rules" ref="postForm" label-width="80px">
      <el-form-item label="标题" prop="title">
        <el-input v-model="form.title" placeholder="请输入标题"></el-input>
      </el-form-item>
      <el-form-item label="内容" prop="content">
        <el-input v-model="form.content" type="textarea" :rows="5" placeholder="请输入内容"></el-input>
      </el-form-item>
      <el-form-item label="添加标签">
      <el-tag
        v-for="(tag, index) in tags"
        :key="index"
        :type="tag.type"
        :closable="tag.closable"
        @close="handleClose(index)"
      >
        {{ tag.label }}
      </el-tag>
      <!--设置一个有预设，也可以自定义的标签-->
      <el-select
        v-model="value"
        multiple
        filterable
        allow-create
        default-first-option
        placeholder="请输入标签名称"
        @keyup.enter.native="handleInputConfirm">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
        <div><el-button type="primary" round slot="append" icon="el-icon-check" @click="handleInputConfirm">
          确认标签
        </el-button>
        </div>
      </el-form-item>
      <el-form-item label="谁可以看">
        <el-radio v-model="radio" label="1">所有人</el-radio>
        <el-radio v-model="radio" label="2">仅自己</el-radio>
      </el-form-item>
      <el-form-item label="上传图片">
        <el-upload
          class="upload-demo"
          :action="uploadUrl"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          :headers="uploadHeaders"
          :data="{ type: 'image' }"
          :on-success="handleUploadSuccess"
          :on-error="handleUploadError"
          :before-upload="beforeUpload"
          :file-list="fileList"
          :limit="9"
          :on-exceed="handleExceedPi"
          :show-file-list="true"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="">
        </el-dialog>
      </el-form-item>
      <el-form-item label="上传文件">
        <el-upload
          class="upload-demo"
          drag
          :action="uploadUrl"
          :headers="uploadHeaders"
          :data="{ type: 'file' }"
          :on-success="handleUploadSuccess"
          :on-error="handleUploadError"
          :before-upload="beforeUpload"
          :file-list="fileList"
          :limit="3"
          :on-exceed="handleExceed"
          :show-file-list="true"
        >
          <i class="el-icon-upload"></i>
          <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
          <div class="el-upload__tip" slot="tip">Tips:图片只能上传jpg/png格式，不能超过500kb，文件只能上传zip/rar文件，且不超过2mb哦ovo</div>
        </el-upload>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('postForm')">发布</el-button>
        <el-button @click="resetForm('postForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'FaTie',
  data() {
    return {
      radio: '1',
      options: [{
        value: '毕业季',
        label: '#毕业季'
      }, {
        value: '电子游戏',
        label: '#电子游戏'
      }, {
        value: '匿名天地',
        label: '#匿名天地'
      }],
      value: [],
      tags: [
        { label: "我爱城院", type: "primary", closable: true },
      ],
      colorIndex: 0,
      colors: ["primary", "success", "warning", "danger", "info"],
      form: {
        dialogImageUrl: '',
        dialogVisible: false,
        title: '',
        content: '',
        files: []
      },
      rules: {
        title: [{ required: true, message: '请输入标题', trigger: 'blur' }],
        content: [{ required: true, message: '请输入内容', trigger: 'blur' }]
      },
      uploadUrl: 'https://jsonplaceholder.typicode.com/posts/',
      uploadHeaders: { Authorization: 'your_token' },
      fileList: []
    }
  },
  methods: {
    beforeUpload(file) {
      const isJPG = file.type === 'image/jpeg' || file.type === 'image/png';
      const isLt500K = file.size / 1024 < 500;
      const isZip = file.type === 'application/x-zip-compressed' || file.type === 'application/zip';
      const isRar = file.type === 'application/octet-stream' || file.type === 'application/x-rar-compressed';
      const isLt2M = file.size / 1024 / 1024 < 2;
      if (this.form.files.length >= 3) {
        this.$message.warning('最多只能上传3个文件');
        return false;
      }
      if (file.data.type === 'image') {
        if (!isJPG || !isLt500K) {
          this.$message.error('上传图片只能是 JPG/PNG 格式，且不超过 500KB');
          return false;
        }
      } else if (file.data.type === 'file') {
        if (!isZip || !isRar || !isLt2M) {
          this.$message.error('上传文件只能是 ZIP/RAR 格式，且不超过 2MB');
          return false;
        }
      }
      return true;
    },
    handleUploadSuccess(response, file) {
      this.fileList.push(file);
      this.form.files.push({
        name: file.name,
        url: response.data.url,
        type: file.data.type
      });
    },
    handleUploadError(error, file) {
      this.$message.error(`${file.name} 上传失败: ${error.message}`);
    },
    handleExceed(files, fileList) {
      this.$message.warning('最多只能上传3个文件');
    },
    handleExceedPi(files, fileList) {
      this.$message.warning('最多只能上传9张图片');
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // 将表单数据及上传的文件提交到后端服务器
          // ...
          this.$message.success('发布成功');
          this.resetForm(formName);
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
      this.fileList = [];
      this.form.files = [];
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleClose(index) {
      this.tags.splice(index, 1);
    },
    handleInputConfirm() {
      if (this.value) {
        const newTag = {
          label: this.value,
          type: this.colors[this.colorIndex],
          closable: true,
        };
        this.tags.push(newTag);
        this.value = [];
        this.colorIndex = (this.colorIndex + 1) % this.colors.length;
      }
    },
  }
}
</script>

<style scoped>
.post-form {
  max-width: 600px;
  margin: 20px auto;
}
.title{
  text-align: center;
  color: black;
}
</style>
