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
        <button class="btn btn-info btn-sm" @click="storePosts">Save</button>
      </div>
      <div class="col-md-6">
        <!-- <button class="btn btn-primary" @click="loadPost">Load Posts</button> -->
        <ul class="p-0">
          <li v-for="post in posts" class="list-group-item my-2 border p-2">
            <h5>{{ post.id }} - {{ post.title }}</h5>
            <p>{{ post.body }}</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {onMounted, reactive, ref} from 'vue';

  const posts = ref([])

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


</script>

<style scoped>

</style>