<template>
  <div>
    <div class="search-bar">
      <input type="text" v-model="searchTerm" placeholder="搜索...">
      <button @click="search">搜索</button>
    </div>
    <div class="container">
      <div class="hotspot" :style="divStyle">
        <h2>热点1</h2>
        <p>这里是热点1的内容</p>
      </div>
      <div :style="divStyle" class="hotspot">
        <h2>热点2</h2>
        <p>这里是热点2的内容</p>
      </div>
      <div :style="divStyle" class="hotspot">
        <h2>热点3</h2>
        <p>这里是热点3的内容</p>
      </div>
      <div :style="divStyle" class="hotspot">
        <h2>热点4</h2>
        <p>这里是热点4的内容</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'HomePage',
  data() {
    return {
      searchTerm: '',  //可以diy搜索框内的内容，双向绑定
      divStyle:{
    height: '500px'  //四个区域的高度
      }
    };
  },
  methods: {
      mounted(){

      },
    search() {
      console.log(this.searchTerm)
      // 这里编写搜索功能的代码
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
  background-color: #ccc;
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


</style>
