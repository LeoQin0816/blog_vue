<template>
<div>
  <!-- nav start -->
  <nav class="am-g am-g-fixed blog-fixed blog-nav">
    <div class="am-collapse am-topbar-collapse" id="blog-collapse">
      <ul class="am-nav am-nav-pills am-topbar-nav">
        <li><router-link to="/"><span class="am-icon-home"></span> 首页</router-link></li>
        <li id="dropdown-justify-category">
          <li class="am-dropdown" id="dropdown-category">
            <a class="am-dropdown-toggle" href="javascript:void(0);"><span class="am-icon-tasks"></span> 分类 <span class="am-icon-caret-down"></span></a>
            <ul class="am-dropdown-content">
              <li v-for="item in titleCate"><router-link :to="{path:'/',query:{category:item.id}}">{{ item.name }}</router-link></li>
            </ul>
          </li>
        </li>
        <li id="dropdown-justify-media">
          <li class="am-dropdown" id="dropdown-media">
            <a class="am-dropdown-toggle" href="javascript:void(0);"><span class="am-icon-photo"></span> 相册 <span class="am-icon-caret-down"></span></a>
            <ul class="am-dropdown-content">
              <li><router-link to="/media">所有</router-link></li>
              <li v-for="item in imageCate"><router-link :to="{path:'/media',query:{category:item.id}}">{{ item.name }}</router-link></li>
            </ul>
          </li>
        </li>
        <li><router-link to="/timeline"><span class="am-icon-archive"></span> 归档</router-link></li>
      </ul>
      <div class="am-topbar-form am-topbar-right am-form-inline">
        <div class="am-form-group">
          <input v-model="searchData" type="text" class="am-form-field am-input-sm" placeholder="搜索" @keyup.enter="searchToParent">
        </div>
      </div>
    </div>
  </nav>
  <hr>
  <!-- nav end -->
</div>
</template>
<script>
import axios from 'axios'
export default {
  data(){
    return{
      // 文章标题分类
      titleCate:[],
      // 图片分类
      imageCate:[],
      // 搜索内容
      searchData:''
    }
  },
  mounted(){
    this.init();
  },
  methods:{
    init(){
      axios.get('/api/titlecate').then((response)=>{
        this.titleCate = response.data;
      });
      axios.get('/api/imagecate').then((response)=>{
        this.imageCate = response.data;
      });
      $('#dropdown-category').dropdown({justify: '#dropdown-justify-category'});
      $('#dropdown-media').dropdown({justify: '#dropdown-justify-media'});
    },
    searchToParent(){
      let searchData = this.searchData;
      this.$router.push({path:'/',query:{search:searchData}});
    }
  }
}
</script>
