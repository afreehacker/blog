<template>
    <div id="single-blog" v-animate-css="'slideInLeft'">
        <h1>{{blog.title}}</h1>
        <article>{{blog.content}}</article>
        <p>作者：{{blog.author}}</p>
        <p>分类：</p>
        <ul>
            <li v-for="category in blog.categories" :key="category">
                {{category}}
            </li>
        </ul>
        <el-button type="danger" @click="deleteSingleBlog()">删除</el-button>
        <!-- <button @click="deleteSingleBlog()">删除</button> -->
        <router-link :to="'/edit/'+id">编辑</router-link>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:"single-blog",
    data() {
        return {
            id:this.$route.params.id,
            // id:1,
            blog:{}
        }
    },
    created(){
        axios.get("https://my-blog-3ffc5-default-rtdb.firebaseio.com/posts/"+this.id+".json")
        .then((data)=>{
            this.blog=data.data;
        })
    },
    methods:{
        deleteSingleBlog(){
            axios.delete("https://my-blog-3ffc5-default-rtdb.firebaseio.com/posts/"+this.id+".json")
            .then(Response=>{
                this.$router.push({path:'/'})
            })
        }
    }
}
</script>

<style scoped>
    #single-blog{
        max-width: 960px;
        margin:0 auto;
        padding: 20px;
        background:#eee;
        border: 1px dotted #aaa;
    }
    /* .router-link-active {
        background: rgba(255, 255, 255, 0.8);
        color: #444;
    } */
    
</style>