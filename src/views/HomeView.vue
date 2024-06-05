<script setup>
import TheWelcome from '../components/TheWelcome.vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import { createApp } from "vue";
</script>

<style>
@import '~bootstrap/scss/bootstrap.scss'
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>


<style scoped>
    /* Set height of the grid so .sidenav can be 100% (adjust if needed) */
    .row.content {
      height: 1500px
    }

    /* Remove the navbar's default margin-bottom and rounded borders */
    .navbar {
      margin-bottom: 0;
      border-radius: 0;
    }

    /* Set gray background color and 100% height */
    .sidenav {
      background-color: #f1f1f1;
      height: 100%;
    }

    /* Set black background color, white text and some padding */
    footer {
      background-color: #555;
      color: white;
      padding: 15px;
    }

    /* On small screens, set height to 'auto' for sidenav and grid */
    @media screen and (max-width: 767px) {
      .sidenav {
        height: auto;
        padding: 15px;
      }

      .row.content {
        height: auto;
      }
    }
</style>


<script>
// Give each commet a unique id...
let id = 0;
let currentPost = [];

export default {
  data() {
    return {
      text: '',
      addCommentBool: false,
      newComment: '',
      comments: [
        { id: id++, text: 'Learn HTML', user: "Guest" },
        { id: id++, text: 'Learn JavaScript', user: "Guest" },
        { id: id++, text: 'Learn Vue', user: "Guest" }
      ],
      newPost: '',
      newPostTitle: '',
      posts: [
        { id: id++, text: 'A', title: "Test", comments: [] },
      ]
    }
  },
  methods: {
    toggle() {
      this.addCommentBool = !this.addCommentBool
    },
    showPost(event) {
          for (let i = 0; i < this.posts.length; i++) {
            console.log(event.target.innerHTML);
            if(this.posts[i].title === event.target.innerHTML) {
                document.getElementById("blog-heading").innerHTML = this.posts[i].title;
                document.getElementById("blog-text").innerHTML = this.posts[i].text;
                currentPost = this.posts[i];
                //currentPost.comments = [];
                this.comments = currentPost.comments;
            }
          }
          document.getElementById("leave-comment").style.display = "block";
          this.$forceUpdate();
    },
    addComment() {
      currentPost.comments.push({ id: id++, text: this.newComment, user: "Guest" })
      this.newComment = ''
      this.comments = currentPost.comments;
      this.$forceUpdate();
    },
    removeComment(comment) {
      this.comments = this.comments.filter((t) => t !== comment)
    },
    addPost() {
      this.posts.push({ id: id++, text: this.newPost, title: this.newPostTitle, comments: [] })
      this.newPost = ''
    },
    removePost(post) {
      this.posts = this.posts.filter((t) => t !== comment)
    }
  }
}
</script>

<template>
  <main>

  <div>
    <div class="container-fluid">
      <div id="myNavbar" style="display: inline-flex; list-style-type: none; background-color: black; width: 100%; color: white; text-decoration: none;">
      <a  style="display: inline-flex; list-style-type: none; color: white; text-decoration: none;">My Blog</a>
        <ul style="display: inline-flex; list-style-type: none; text-decoration: none;">
          <div v-if="addCommentBool" @click="toggle">
          <li style=" text-decoration: none; color: white;" id="new-post-button"><a style=" text-decoration: none; color: white;">- HIDE NEW POST WINDOW</a></li>
          </div>
          <div v-else @click="toggle">
          <li style=" text-decoration: none; color: white;" id="new-post-button"><a style=" text-decoration: none; color: white;">+ NEW POST</a></li>
          </div>
        </ul>
      </div>
    </div>
  </div>







<div v-if="addCommentBool" id="add-new-post">
            <h2>Create New Post</h2>
            <div class="form-group">
              <label for="email">Title:</label>
              <div class="input-group">
                <span class="input-group-addon"><i class="glyphicon glyphicon-font"></i></span>
              </div>
              <br>
                     <form @submit.prevent="addPost">

            <input class="form-control" v-model="newPostTitle" id="post-title">
    <textarea id="post-form" class="form-control" rows="3" v-model="newPost" required></textarea>
    <button id="submit-post" class="btn btn-success">Submit Post</button>  
  </form>
              <br><br>
            </div>
          </div>

  <div v-else id="post-core">
    <div class="container-fluid">
      <div class="row content">
        <div class="col-sm-3 sidenav">
          <h4>My Blog</h4>
          <li id="post-selector-prototype" style="display: none;"><a>Post 1</a></li>
          <ul id="post-selector" style="display: row; list-style-type: none;">
                <li v-for="post in posts" :key="post.id" @click="showPost">{{ post.title }}</li>
          </ul>
        </div>
<div class="col-sm-9">
          <h4><small>MY BLOG</small></h4>
          <hr>





                    <div id="post-and-comments-body">
            <h2 id="blog-heading">Hello World!</h2>
            <p id="blog-text">Welcome to the blog. Click on a post on the left bar to see it or click the + NEW POST button to create your own post.
            You can also leave a comment.</p>
            <br><br>
<div id="leave-comment" style="display: none;">
            <h4>Leave a Comment:</h4>
    <form @submit.prevent="addComment">   
                  <div class="form-group">
                <textarea id="comment-form" v-model="newComment" class="form-control" rows="3" required></textarea>
              </div>
              <button id="submit-comment" class="btn btn-success">Submit</button>
  </form>

            <br><br>

            <p><span id="comment-count" class="badge">0</span> Comments:</p><br>


            <div id="comment-photo-prototype" style="display: none;" class="col-sm-2 text-center">
              <img src="Pictures/Logo.png" class="img-circle" height="65" width="65" alt="Avatar">
            </div>
            <div id="comment-prototype" style="display: none;" class="col-sm-10">
              <h4>
                <p>Kolya Vasylenko</p> <small>Sep 29, 2015, 9:12 PM</small>
              </h4>
              <p id="comment-text"></p>
              <hr>
            </div>
            <div id="comment-container" class="row">

            <div id="comment-prototype" v-for="comment in comments" :key="comment.id" class="col-sm-10">
              <h4>
                <p>{{ comment.user }}</p>
              </h4>
              <p id="comment-text">{{ comment.text }}</p>
              <hr>
            </div>
  </div>
            </div>
            <br>
          </div>
        </div>
      </div>
    </div>

  </div> <!---Post Core-->


    <footer class="container-fluid">
    <p>My Blog ©. All rights reserved.</p>
  </footer>
  
  
  
  <p>{{ text }}</p>


  </main>
</template>
