<template>
  <div>
    <div class="search-bar">
      <input type="text" v-model="searchTerm" placeholder="搜索...">
      <button @click="search">搜索</button>
    </div>
    <div class="container">
      <div class="hotspot" :style="divStyle">
        <el-carousel indicator-position="outside">
          <el-carousel-item v-for="item in 4" :key="item">
            <h3>{{ item }}</h3>
          </el-carousel-item>
        </el-carousel>
      </div>
      <div :style="divStyle" class="hotspot">
        <h2>热点2</h2>
        <p>这里是热点2的内容</p>
      </div>
      <el-scrollbar :native="false" ref="scrollbar" @scroll="handleScroll">
        <div class="content">
          <div v-for="(item, index) in items" :key="index" class="item">
            {{ item }}
          </div>
          <div v-if="isLoading" class="loading">
            <el-spinner size="small" />
          </div>
        </div>
      </el-scrollbar>
      <div :style="divStyle" class="hotspot">
        <h2>热点4</h2>
        <p>这里是热点4的内容</p>
      </div>
    </div>
  </div>
</template>

<script>
import { Scrollbar, Spinner } from 'element-ui';

export default {
  name:'HomePage',
  components: {
    ElScrollbar: Scrollbar,
    ElSpinner: Spinner,
  },
  data() {
    return {
      searchTerm: '',  //可以diy搜索框内的内容，双向绑定
      divStyle:{
    height: '350px'  //四个区域的高度
      },
      items: [],
      isLoading: false,
      page: 1,
    };
  },
  mounted() {
    this.loadItems();
  },
  methods: {
      mounted(){

      },
    search() {
      console.log(this.searchTerm)
      // 这里编写搜索功能的代码
    },
    loadItems() {
      this.isLoading = true;
      // 模拟加载数据
      setTimeout(() => {
        const newItems = [];
        for (let i = 0; i < 10; i++) {
          newItems.push(`Item ${this.items.length + i + 1}`);
        }
        this.items = [...this.items, ...newItems];
        this.isLoading = false;
        this.page++;
      }, 1000);
    },
    handleScroll() {
      const scrollbar = this.$refs.scrollbar;
      const distance = scrollbar.$el.scrollHeight - scrollbar.$el.scrollTop - scrollbar.$el.clientHeight;
      if (distance < 50 && !this.isLoading) {
        this.loadItems();
      }
    },
  },
};
</script>

<style>
.container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-gap: 20px;

}

.hotspot {
  background-color: #ffffff;
  border: 1px solid #000;
  text-align: center;

}
.search-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.search-bar input {
  width: 300px;
  height: 30px;
  padding: 5px;
  margin-right: 10px;
}

.search-bar button {
  height: 40px;
  padding: 5px 10px;
  background-color: #42b983;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
.content {
  height: 300px;
  overflow-y: auto;
}

.item {
  height: 50px;
  line-height: 50px;
  border-bottom: 1px solid #ccc;
  text-align: center;
  border: 1px solid #000;
}

.loading {
  text-align: center;
  margin-top: 20px;
}

</style>
