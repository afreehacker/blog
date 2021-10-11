<template>
  <div id="add-blog">
    <h2 v-animate-css="'zoomIn'">添加博客</h2>
    <form v-if="!submmited">
      <label v-animate-css="'zoomIn'">博客标题</label>
      <input type="text" v-model="blog.title" required v-animate-css="'zoomIn'"/>

      <label v-animate-css="'zoomIn'">博客内容</label>
      <textarea v-model="blog.content" v-animate-css="'zoomIn'"></textarea>

      <div id="checkboxes" v-animate-css="'zoomIn'">
        <label>Vue.js</label>
        <input type="checkbox" value="Vue.js" v-model="blog.categories" />
        <label>Node.js</label>
        <input type="checkbox" value="Node.js" v-model="blog.categories" />
        <label>React.js</label>
        <input type="checkbox" value="React.js" v-model="blog.categories" />
        <label>Angular4</label>
        <input type="checkbox" value="Angular4" v-model="blog.categories" />
        <label>others</label>
        <input type="checkbox" value="others" v-model="blog.categories" />
      </div>

      <label v-animate-css="'zoomIn'">作者</label>
      <select v-model="blog.author" v-animate-css="'zoomIn'">
        <option v-for="author in authors" :key="author">
          {{ author }}
        </option>
      </select>

      <button v-on:click.prevent="post" v-animate-css="'zoomIn'">添加博客</button>
      
    </form>

    <div v-if="submmited">
      <h3>你的博客发布成功！</h3>
    </div>

    <div id="preview" v-animate-css="'zoomIn'">
      <h3>博客总览</h3>
      <p>博客标题：{{ blog.title }}</p>
      <p>博客内容：</p>
      <p>{{ blog.content }}</p>
      <p>博客分类：</p>
      <ul>
        <li v-for="category in blog.categories" :key="category">
          {{ category }}
        </li>
      </ul>
      <p>作者：{{ blog.author }}</p>
    </div>

  </div>

</template>

<script>
import axios from 'axios'
export default {
  name: "add-blog",
  data() {
    return {
      blog: {
        title: "",
        content: "",
        categories: [],
        anthor: ""
      },
      authors: ["Hemiah", "Henry", "Bucky","anonymity"],
      submmited: false,
    };
  },
  methods: {
    post: function () {
      axios.post("https://my-blog-3ffc5-default-rtdb.firebaseio.com/posts.json", this.blog)
        .then((data) => {
          //console.log(data);
          this.submmited = true;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#add-blog * {
  box-sizing: border-box;
}

#add-blog {
  margin: 20px auto;
  max-width: 600px;
  padding: 20px;
}

label {
  display: block;
  margin: 20px 0 10px;
}

input[type="text"],
textarea,
select {
  display: block;
  width: 100%;
  padding: 8px;
}

textarea {
  height: 200px;
}

#checkboxes label {
  display: inline-block;
  margin-top: 0;
}

#checkboxes input {
  display: inline-block;
  margin-right: 10px;
}

button {
  display: block;
  margin: 20px 0;
  background: crimson;
  color: #fff;
  border: 0;
  padding: 14px;
  border-radius: 4px;
  font-size: 18px;
  cursor: pointer;
}

#preview {
  padding: 10px 20px;
  border: 1px dotted #ccc;
  margin: 30px 0;
}

h3 {
  margin-top: 10px;
}
</style>
