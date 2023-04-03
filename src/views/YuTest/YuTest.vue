<template>
  <div>
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
      <el-button slot="append" icon="el-icon-check" @click="handleInputConfirm" />
  </div>
</template>

<script>
export default {
  name: 'YuTest',
  data() {
    return {
      options: [{
        value: '毕业季',
        label: '毕业季'
      }, {
        value: '电子游戏',
        label: '电子游戏'
      }, {
        value: '闲置出售',
        label: '闲置出售'
      }],
      value: [],
      tags: [
        { label: "标签一", type: "primary", closable: true },
        { label: "标签二", type: "success", closable: true },
        { label: "标签三", type: "warning", closable: true },
        { label: "标签四", type: "danger", closable: true },
      ],
      colorIndex: 0,
      colors: ["primary", "success", "warning", "danger", "info"],
    };
  },
  methods: {
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
  },
};
</script>
