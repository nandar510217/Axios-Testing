<template>
  <div class="container">
    <div class="row my-3">
      <div class="col-md-6">
        <h4>Axios</h4>
        <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Adipisci nulla animi nam magnam nisi fuga, consequatur qui dolores assumenda distinctio ducimus, iusto, tempore pariatur non atque aperiam. Ratione, at quisquam!</p>
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
  import {onMounted, ref} from 'vue';

  const posts = ref([])

  //get posts
  const getPosts = () => {
    axios.get('https://jsonplaceholder.typicode.com/posts')
  .then((res) => {
    posts.value = res.data
  })
  .catch((err) =>  console.log(err))
  }
  
  //getPosts နဲ့ ခေါ်လို့ရပေမယ့် onMounted ထဲမှာပိုကောင်း browser ထဲတကယ်ရောက်လာမှအလုပ်လုပ်
  onMounted(() => {
    getPosts();
  })

  
</script>

<style scoped>

</style>