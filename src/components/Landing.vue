<template>
  <div id="app">
    <div id="nav">
      <router-link to="">Home</router-link> |
      <router-link to="/bookmark">Bookmark</router-link> |
      <router-link to="/like">Liked</router-link>
    </div>
    <div class="container">
      <stack
              :column-min-width="300"
              :gutter-width="15"
              :gutter-height="15"
              monitor-images-loaded
      >
        <stack-item
                v-for="(content, i) in contentData"
                :key="i"
                style="transition: transform 300ms"
        >
          <img :src="content.imgUrl" :alt="content.description" />
          <p class="judul">{{content.productName}}</p>
          <p><b-badge variant="secondary">{{content.tags}}</b-badge></p>
          <p>          
            <b-button pill variant="primary" @click="like(content.contentId)">Like</b-button>
            <b-button pill variant="danger" @click="dislike(content.contentId)">Dislike</b-button>
            <b-button pill variant="outline-info" @click="bookmark(content.contentId)">Bookmark</b-button>
          </p>
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  name: "Landing",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    contentIds: [],
    contentData: [],
  }),
    mounted() {
      this.contentIds = localStorage.getItem('contentID').split(',');
      console.log(this.contentIds);
      for (var i=0; i<this.contentIds.length; i++){
        axios.post(`https://makan-master.herokuapp.com/api/content`,
            { 
              contentId: this.contentIds[i]
            }
          )
          .then(res => {
            // console.log(res.data)
            if(res.data != undefined){
              this.contentData.push(res.data);
              // console.log(res.data.contentId)
            }  
            console.log(this.contentData[this.contentData.length - 1].contentId);
          })
      }
     },
     methods : {
       bookmark(id) {
         console.log(id);
         axios.defaults.headers.common = {'Authorization': "Bearer " + localStorage.getItem('token')}
         axios.post('https://makan-master.herokuapp.com/api/interaction',
         {
            // headers: {
            //     'Authorization': "Bearer " + localStorage.getItem('token')
            // },
             interaction: '2',
             contentId: id.toString()
         })
         .then(res => {
             console.log(res);
         })
       },
       like(id) {
         console.log(id);
         axios.defaults.headers.common = {'Authorization': "Bearer " + localStorage.getItem('token')}
         axios.post('https://makan-master.herokuapp.com/api/interaction',
         {
            // headers: {
            //     'Authorization': "Bearer " + localStorage.getItem('token')
            // },
             interaction: '0',
             contentId: id.toString()
         })
         .then(res => {
             console.log(res);
         })
       },
        dislike(id) {
         console.log(id);
         axios.defaults.headers.common = {'Authorization': "Bearer " + localStorage.getItem('token')}
         axios.post('https://makan-master.herokuapp.com/api/interaction',
         {
            // headers: {
            //     'Authorization': "Bearer " + localStorage.getItem('token')
            // },
             interaction: '1',
             contentId: id.toString()
         })
         .then(res => {
             console.log(res);
         })
       },
     }
   };
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
.judul {
  margin-top: 12px;
  margin-bottom: 5px;  
  font-weight: bold;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.btn {
  font-size: 18px;
  background-color: #42b983;
  color: white;
  padding: 10px 20px;
}
.btn:not(:last-child) {
  margin-right: 10px;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
</style>
