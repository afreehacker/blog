<template>
  <div v-theme="'wide'" id="show-blogs" >
    <h1 v-animate-css="'zoomIn'">博客主页</h1>
    <input type="text" v-model="search" placeholder="搜索">
    <div v-for="blog in filterBlogs" :key="blog.title" class="single-blog" v-animate-css="'zoomIn'">
        <!-- <router-link v-bind:to="'/blog/'+blog.id"><h2 v-rainbow>{{blog.title | to-uppercase}}</h2></router-link> -->
        <router-link v-bind:to="'/blog/'+blog.id"><h2>{{blog.title | to-uppercase}}</h2></router-link>
        <article>{{blog.content | snippet}}</article>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
//import  "animate.css"
export default {
  name: 'show-blogs',
  data(){
      return{
          blogs:[],
          search:""
      }
  },
  created(){
      axios.get("https://my-blog-3ffc5-default-rtdb.firebaseio.com/posts.json")
      .then(function(data){
          return data.data;
      }).then((data)=>{
        var blogsArray=[];
        for(let key in data){
          data[key].id=key;
          blogsArray.push(data[key]);
        }
        this.blogs=blogsArray;
      })
  },
  computed:{
    filterBlogs:function(){
      return this.blogs.filter((blog)=>{
          return blog.title.match(this.search);
      })
    }
  },
  filters:{
    "to-uppercase":function(value){
        return value.toUpperCase();
    },
    "snippet":function(value){
        return value.slice(0,100)+"...";
    }
  },
  directives:{
    // 'rainbow':{
    //     bind(el, binding, vnode) {
    //        el.style.color = "#" + Math.random().toString(16).slice(2, 8);
    //     }
    // },
    'theme':{
      bind(el, binding, vnode) {
          if (binding.value == 'wide') {
              el.style.maxWidth = "1260px";
          } else if (binding.value == 'narrow') {
              el.style.maxWidth = "560px";
            }
      }
    }
  }
}
</script>

<style>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
}
.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
  border: 1px dotted #aaa;
}

#show-blogs a{
  color: #444;
  text-decoration: none;
}

input[type="text"]{
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}


</style>