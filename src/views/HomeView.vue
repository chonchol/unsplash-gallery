<script>
export default {
  data() {
    return {
      image: "",
      topic: "",
      inputedTopic: "",
      searchPhoto: "",
      accessKey:
        "622cdc9d4ce03f872f74b65e6f5d3be09c6bfabde530761b5bc47f2c840c4ad7",
      photoUrl: "https://api.unsplash.com/photos",
      topicUrl: "https://api.unsplash.com/topics",
      searchUrl: "https://api.unsplash.com/search/photos",
    };
  },
  methods: {

    searchedPhoto(){
        fetch(this.searchUrl + `?page=1&query=${this.inputedTopic}&per_page=9` + `&client_id=${this.accessKey}`)
        .then((response) => response.json())
        .then((json) => {
          this.searchPhoto = json.results;
          // console.log()
        })
        .catch((err) => {
          console.log("error", err);
        })
    },

    fetchRandomPhoto() {
      fetch(this.photoUrl + `?per_page=9` + `&client_id=${this.accessKey}`)
        .then((response) => response.json())
        .then((json) => {
          // console.log(json);
          this.image = json;
          console.log(this.image)
        })
        .catch((err) => {
          console.log("error", err);
        });
    },
    fetchTopic() {
      fetch(this.topicUrl + `?client_id=${this.accessKey}`)
        .then((response) => response.json())
        .then((json) => {
          this.topic = json;
          // console.log(this.topic); 
        })
        .catch((err) => {
          console.log("error", err);
        });
    },
    selectedTopic(title){
      // console.log(id)
      this.inputedTopic = title;
    },
  },
  created() {
    this.fetchRandomPhoto();
    this.fetchTopic();
    // this.searchedPhoto();
  },
};
</script>

<template>
  <main>
    <h2 class="text-center p-8 text-4xl font-bold">Gallery Page</h2>
    <div>
      <div class="flex items-center justify-center">   
        <!-- <div class="w-full"> -->
          <input v-model="inputedTopic" type="text" class="bg-white border border-gray-300 text-violet-900 text-sm rounded-lg focus:ring-violet-500 focus:border-violet-500 block w-1/2 pl-10 p-2.5" placeholder="Search photos...." required>
        <!-- </div> -->
          <button @click="searchedPhoto" class="py-2.5 px-8 ml-2 text-sm font-medium text-white bg-violet-700 rounded-lg border border-violet-700 hover:bg-violet-800 focus:ring-4 focus:outline-none focus:ring-violet-300">
            Search
        </button>
      </div>


    </div>
    <div>
      <div class="flex items-center justify-center py-4 md:py-8 flex-wrap">
        <div>
          <button
          v-for="item in topic"
          :key="item.title"
          @click="selectedTopic(item.title)"
          type="button"
          class="text-gray-900 border border-white hover:border-gray-200 dark:border-gray-900 dark:bg-gray-900 dark:hover:border-gray-700 bg-white focus:ring-4 focus:outline-none focus:ring-gray-300 rounded-full text-base font-medium px-5 py-2.5 text-center mr-3 mb-3 dark:text-white dark:focus:ring-gray-800"
        >
          {{ item.title }}
        </button>
        </div>
      </div>
      
      <div v-if="!searchPhoto" class="grid grid-cols-2 md:grid-cols-3 gap-4 mr-3 ml-3 mb-10">
        <div class="relative" v-for="img in image">
          <div class="relative w-full h-0 pb-[60%]">
            <img
              class="rounded-lg absolute inset-0 w-full h-full object-cover"
              :src="img.urls.regular"
              :alt="img.alt_description"
              height="200"
            />
          </div>

          <div class="absolute top-1/2 right-0 text-white mr-3 bg-text rounded-lg">
            <p>Total Like: {{ img.likes }}</p>
            <p class="mb-6">Author Name: {{ img.user.name }}</p>
            <a class="border border-inherit p-2" :href="img.links.download" :download="img.links.download">Download</a>
          </div>
        </div>
      </div>

      <div v-else class="grid grid-cols-2 md:grid-cols-3 gap-4 mr-3 ml-3 mb-10">
        <div class="relative" v-for="photo in searchPhoto">
          <div class="relative w-full h-0 pb-[60%]">
            <img
              class="rounded-lg absolute inset-0 w-full h-full object-cover"
              :src="photo.urls.regular"
              :alt="photo.alt_description"
              height="200"
            />
          </div>

          <div class="absolute top-1/2 right-0 text-white mr-3 bg-text rounded-lg">
            <p>Total Like: {{ photo.likes }}</p>
            <p class="mb-6">Author Name: {{ photo.user.name }}</p>
            <a class="border border-inherit p-2" :href="photo.links.download" :download="photo.links.download">Download</a>
          </div>
        </div>
      </div>
      

    </div>
  </main>
</template>

<style>
.bg-text {
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0, 0.4);
    color: white;
    font-weight: bold;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    width: 100%;
    padding: 18%;
}
</style>