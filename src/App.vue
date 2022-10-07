<template>
  <div id="app">
    <div class="viewPosts">
      <div v-for="(post, index) in posts" :key="'post' + index" class="post">
        <div class="header">
          <div class="avatar" :style="{ background: post.color }"></div>
          <div class="headerInformations">
            <div class="title">
              <h1 class="titlePost">{{ post.title }}</h1>
              <div class="actions">
                <button class="deleteButton" @click="deletePost(index)">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
                <button class="editButton" @click="editPost(index)">
                  <i class="fa-solid fa-pen-to-square"></i>
                </button>
              </div>
            </div>
            <div class="subtitle">
              <h2>{{ post.subtitle }}</h2>
              <span>{{ post.date }}</span>
            </div>
          </div>
        </div>
        <div class="contentPost">
          <div class="contentFont">
            {{ post.content }}
          </div>
          <div class="author">De: {{ post.author }}</div>
        </div>
      </div>
    </div>
    <div class="newPost">
      <input
        type="text"
        placeholder="Titulo"
        class="input-text"
        v-model="postTitle"
      />
      <input
        type="text"
        placeholder="Subtitulo"
        class="input-text"
        v-model="postSubtitle"
      />
      <textarea
        type="text-area"
        placeholder="Publicação"
        class="input-text area-text"
        v-model="postContent"
      ></textarea>
      <button
        type="submit"
        class="publishButton"
        @click="newPost"
        :disabled="isDisabledButton"
      >
        PUBLICAR / ATUALIZAR
      </button>

      <div class="error" v-show="messageError">{{ messageError }}</div>
    </div>
  </div>
</template>

<script>
const moment = require("moment");

export default {
  name: "App",

  data() {
    return {
      postTitle: "",
      postSubtitle: "",
      postContent: "",
      isEditingPost: false,
      idEditingPost: 0,
      messageError: "",
      posts: [
        {
          color: this.getColor(),
          title: "Lorem Ipsum 01",
          subtitle: "Dolor sit amet",
          date: this.getDate(),
          content:
            "Dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
          author: "Carolina Souza Floriano",
        },
        {
          color: this.getColor(),
          title: "Lorem Ipsum 02",
          subtitle: "Dolor sit amet",
          date: this.getDate(),
          content:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Risus commodo viverra maecenas accumsan. Pellentesque eu tincidunt tortor aliquam. Semper feugiat nibh sed pulvinar. Nisl nunc mi ipsum faucibus vitae. Nisi vitae suscipit tellus mauris a diam. Lacus luctus accumsan tortor posuere ac ut consequat. Ac turpis egestas integer eget. Ut aliquam purus sit amet luctus venenatis lectus magna. Diam vel quam elementum pulvinar etiam non. Vitae purus faucibus ornare suspendisse sed nisi. Enim nulla aliquet porttitor lacus luctus accumsan tortor posuere ac. In ante metus dictum at tempor commodo ullamcorper a lacus.",
          author: "Carolina Souza Floriano",
        },
      ],
    };
  },

  methods: {
    getDate() {
      return moment().format("DD/MM/YYYY");
    },
    newPost() {
      if (
        this.postTitle == "" ||
        this.postSubtitle == "" ||
        this.postContent == ""
      ) {
        this.messageError = "Preencha os campos obrigatórios!";
        setTimeout(() => {
          this.messageError = "";
        }, 3000);
      } else {
        if (this.isEditingPost) {
          this.posts[this.idEditingPost].title = this.postTitle;
          this.posts[this.idEditingPost].subtitle = this.postSubtitle;
          this.posts[this.idEditingPost].content = this.postContent;
        } else {
          this.posts.push({
            color: this.getColor(),
            title: this.postTitle,
            subtitle: this.postSubtitle,
            date: this.getDate(),
            content: this.postContent,
            author: "Carolina Souza Floriano",
          });
        }
        this.postTitle = "";
        this.postSubtitle = "";
        this.postContent = "";
        this.isEditingPost = false;
      }
    },
    deletePost(id) {
      this.posts.splice(id, 1);
    },
    editPost(id) {
      this.postTitle = this.posts[id].title;
      this.postSubtitle = this.posts[id].subtitle;
      this.postContent = this.posts[id].content;
      this.isEditingPost = true;
      this.idEditingPost = id;
    },

    getColor() {
      let r = parseInt(Math.random() * 255);
      let g = parseInt(Math.random() * 255);
      let b = parseInt(Math.random() * 255);

      return "rgb(" + r + "," + g + "," + b + ")";
    },
  },

  computed: {},
};
</script>

<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css";

body {
  background-color: #f3f3f3;
  margin: 0;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  column-gap: 10px;
}

.viewPosts {
  background-color: #e7e7e7;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  row-gap: 15px;
  width: 60%;
  height: 80%;
  overflow-y: auto;
  padding: 2%;
  box-sizing: border-box;
}

.post {
  background-color: #f3f3f3;
  border-radius: 7px;
  box-sizing: border-box;
  padding: 30px;
  box-shadow: 2px 5px 10px #8b8a8a;
}

.header {
  display: flex;
  flex-direction: row;
  column-gap: 10px;
  border-bottom: 2px solid #8b8a8a;
}

.avatar {
  border-radius: 50%;
  width: 90px;
  height: 90px;
}

.headerInformations {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.title {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.titlePost {
  margin: 0;
}

.actions {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 2px;
}

.subtitle {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.contentPost {
  padding-top: 10px;
  text-align: justify;
  line-height: 1.5;
}

.author {
  display: flex;
  justify-content: right;
  margin-top: 15px;
}

.deleteButton {
  background-color: rgb(214, 4, 4);
  color: #ffffff;
  border-radius: 5px;
  border: 0;
  width: 25px;
  aspect-ratio: 1;
}

.editButton {
  background-color: rgb(25, 25, 254);
  color: #ffffff;
  border-radius: 5px;
  border: 0;
  width: 25px;
  aspect-ratio: 1;
}

.newPost {
  background-color: #e7e7e7;
  border-radius: 10px;
  width: 30%;
  height: 80%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  box-sizing: border-box;
  padding: 2%;
  row-gap: 30px;
}

.publishButton {
  box-sizing: border-box;
  background-color: #32cd32;
  color: #ffffff;
  font-weight: bold;
  font-size: 1em;
  border: 0;
  border-radius: 7px;
  padding: 10px;
}

.input-text {
  box-sizing: border-box;
  box-shadow: 0px 2px 5px rgb(132, 131, 131);
  border-radius: 5px;
  border: 1px solid #ffffff;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  font-size: 1em;
  line-height: 2em;
  color: #413f3f;
  outline: none;
  padding: 5px;
}

.input-text:focus {
  border: 2px solid #32cd32;
}

.input-text::placeholder {
  color: #908f8f;
  padding: 1.5px;
}

.area-text {
  flex-grow: 1;
}

.error {
  box-sizing: border-box;
  position: absolute;
  background-color: #ffffff;
  width: 80%;
  height: 10%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  margin: 0 auto;
  border-radius: 7px;
  border: 2px solid red;
  color: red;
  text-align: center;
  display: grid;
  align-content: center;
  font-size: 12px;
}

@media (max-width: 600px) {
  #app {
    display: flex;
    flex-direction: column;
    font-size: 0.75em;
  }

  .viewPosts {
    width: 98%;
  }

  .newPost {
    width: 98%;
    height: 60%;
    position: relative;
    row-gap: 10px;
  }

  .post {
    padding: 15px;
  }

  .avatar {
    margin-top: 10px;
    border-radius: 50%;
    width: 50px;
    height: 50px;
  }
}
</style>
