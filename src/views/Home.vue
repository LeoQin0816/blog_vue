<template>
<div>
<nav-header :key="key"></nav-header>
<!-- content start -->
<div class="am-g am-g-fixed blog-fixed">
  <div class="am-u-md-8 am-u-sm-12">
    <article class="am-g blog-entry-article" v-for="text in texts">
        <div class="am-u-lg-4 am-u-md-12 am-u-sm-12 blog-entry-img">
            <img v-lazy="'/storage/'+text.image" alt="" class="am-u-sm-12">
        </div>
        <div class="am-u-lg-8 am-u-md-12 am-u-sm-12 blog-entry-text">
            <span style="color:grey" class="am-icon-user">&nbsp;<span style="color:black" class="am-kai">Leo Qin</span></span>&nbsp;-&nbsp;
            <span style="color:grey" class="am-icon-eye">&nbsp;<span style="color:black" class="am-kai">{{ text.is_read }}</span></span>&nbsp;-&nbsp;
            <span style="color:grey" class="am-icon-calendar-minus-o">&nbsp;<span style="color:black" class="am-kai">{{ text.updated_at }}</span></span>
            <h1><router-link :to="'/article/'+text.id">{{ text.title }}</router-link></h1>
            <p>{{ text.summary }}</p>
            <!-- <p><a href="" class="blog-continue">continue reading</a></p> -->
        </div>
    </article>
    <ul class="am-pagination am-pagination-centered">
      <li v-bind:class="{'am-disabled':pageMark==1}"><a href="javascript:void(0);" v-on:click="prev">&laquo;</a></li>
      <li v-for="n in totalPage" v-bind:class="{'am-active': n==pageMark}"><a href="javascript:void(0);" @click="jump(n)">{{ n }}</a></li>
      <li v-bind:class="{'am-disabled':pageMark==totalPage}"><a href="javascript:void(0);" v-on:click="next">&raquo;</a></li>
    </ul>
  </div>
  <!-- 右侧关于我界面 -->
  <about-me></about-me>
</div>
<!-- content end -->
  <nav-footer></nav-footer>
</div>
</template>
<script>
import AboutMe from '@/components/AboutMe'
import NavFooter from '@/components/Footer'
import NavHeader from '@/components/Header'
import axios from 'axios'
export default {
  data(){
    return{
      // 页数
      page:1,
      // 前一页后一页标识
      pageMark:1,
      // 每页条数
      paginate:6,
      // 总页数
      totalPage:'',
      // 文章
      texts:[],
      // 分类id
      category:'',
      // 标签id
      tag:'',
      // 查询数据
      searchData:'',

    }
  },
  mounted(){
    this.init();
  },
  components:{
    NavHeader,
    AboutMe,
    NavFooter,
  },
  // 排除header组件复用
  computed: {
    key() {
      return this.$route.name !== undefined? this.$route.name +new Date(): this.$route +new Date()
    }
  },
  // 监控category路由变化
  watch:{
    '$route':function(){
      this.init();
    }
  },
  methods:{
    // 初始化
    init(){
      this.category = this.$route.query.category =='undefined' ? '' : this.$route.query.category;
      this.tag = this.$route.query.tag =='undefined' ? '' : this.$route.query.tag;
      this.searchData = this.$route.query.search =='undefined' ? '' : this.$route.query.search;
      axios.get("/api/home",{
        params:{
          paginate:this.paginate,
          page:this.page,
          category_id:this.category,
          searchData:this.searchData,
          tag_id:this.tag
        }
      }).then((response)=>{
        let res = response.data;
        this.texts = res.data;
        this.totalPage = res.last_page;
        this.pageMark=this.page;
        this.windowScroll();
      })
    },
    // 前一页
    prev(){
      this.init(--this.page);
    },
    // 后一页
    next(){
      this.init(++this.page);
    },
    // 窗口滚回最顶部
    windowScroll(){
      window.scroll(0, 0);
    },
    // 跳转页面
    jump(n){
      this.page = n;
      this.init();
    },
  }
}
</script>


