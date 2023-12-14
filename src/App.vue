<template>
  <div class="container">
    <div class="row my-3">
      <div class="col-md-6">
        <h4>Post Entry From</h4>
        <div class="mb-2">
          <input type="text" v-model="form.title" class="form-control">
        </div>
        <div class="mb-2">
          <input type="text" v-model="form.body" class="form-control">
        </div>
        <div class="mb-2">
          <input type="text" v-model="form.userId" class="form-control">
        </div>
        <button v-if="isEdit" class="btn btn-info btn-sm" @click="updatePost">Update</button>
        <button v-else class="btn btn-info btn-sm" @click="storePosts">Save</button>
      </div>
      <div class="col-md-6">
        <!-- <button class="btn btn-primary" @click="loadPost">Load Posts</button> -->
        <ul class="p-0">
          <li v-for="post in posts" class="list-group-item my-2 border p-2">
            <h5>{{ post.id }} - {{ post.title }}</h5>
            <p>{{ post.body }}</p>
            <button class="btn btn-sm btn-danger" @click="deletePost(post.id)">Delete</button>
            <button class="btn btn-sm btn-info mx-1" @click="getPost(post.id)">Edit</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {onMounted, reactive, ref} from 'vue';

  const posts = ref([])
  const isEdit = ref(false)
  const editPostId = ref(null)

  //write
  const form = reactive({
    title: null,
    body: null,
    userId: null
  })

  //store posts
  const storePosts = () => {
    axios.post('https://jsonplaceholder.typicode.com/posts', form)
    .then((res) => {
      posts.value.unshift(res.data)
    })
    .catch((err) => console.log(err))
  }

  //get posts
  const getPosts = () => {
    axios.get('https://jsonplaceholder.typicode.com/posts')
    .then((res) => {
      posts.value = res.data
    })
    .catch((err) =>  console.log(err))
  }
  
  //getPosts နဲ့ ခေါ်လို့ရပေမယ့် onMounted ထဲမှာရေးတာပိုကောင်း browser ထဲတကယ်ရောက်လာမှအလုပ်လုပ်
  onMounted(() => {
    getPosts();
  })

  //Update post
  const getPost = (id) => {
    // axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)
    // .then((res) => {
    //   form.title = res.data.title
    //   form.body = res.data.body
    //   form.userId = res.data.userId
    // })
    // .catch((err) => console.log(err))

    const post = posts.value.find(post => {
      return post.id === id
    })
      form.title = post.title
      form.body = post.body
      form.userId = post.userId

      isEdit.value = true
      editPostId.value = post.id
  }

  const updatePost = () => {
    axios.put(`https://jsonplaceholder.typicode.com/posts/${editPostId.value}`, form)
    .then((res) => {
      posts.value.forEach(post => {
        if(post.id=== editPostId.value) {
          post.title = res.data.title
          post.body = res.data.body
          post.userId =res.data.userId
        }
      })
    })
    .catch((err) => console.log(err))
  }


  //delete a post
  const deletePost = (id) => {
    axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
    .then((res) => console.log(res))
    .catch((err) => console.log(err))

    //delete လုပ်ရင် browser မှာမြင်ချင်လို့ ခဏသုံးပြ
    posts.value = posts.value.filter((post) => {
      return post.id !== id 
    })

  }

  

</script>

<style scoped>

</style>