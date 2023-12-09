<script>
export default {
  data() {
    return {
      image: "",
      topic: "",
      accessKey:
        "622cdc9d4ce03f872f74b65e6f5d3be09c6bfabde530761b5bc47f2c840c4ad7",
      photoUrl: "https://api.unsplash.com/photos",
      topicUrl: "https://api.unsplash.com/topics",
    };
  },
  methods: {
    fetchRandomPhoto() {
      fetch(this.photoUrl + `?client_id=${this.accessKey}`)
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
  },
  created() {
    this.fetchRandomPhoto();
    this.fetchTopic();
  },
};
</script>

<template>
  <main>
    <h2 class="text-center p-8 text-4xl font-bold">Gallery Page</h2>
    <div>
      

      <form class="flex items-center justify-center">   
        <!-- <div class="w-full"> -->
            <input type="text" class="bg-white border border-gray-300 text-violet-900 text-sm rounded-lg focus:ring-violet-500 focus:border-violet-500 block w-1/2 pl-10 p-2.5" placeholder="Search photos...." required>
        <!-- </div> -->
        <button type="submit" class="py-2.5 px-8 ml-2 text-sm font-medium text-white bg-violet-700 rounded-lg border border-violet-700 hover:bg-violet-800 focus:ring-4 focus:outline-none focus:ring-violet-300">
            Search
        </button>
    </form>


    </div>
    <div>
      <div class="flex items-center justify-center py-4 md:py-8 flex-wrap">
        <div v-for="item in topic">
          <button
          type="button"
          class="text-gray-900 border border-white hover:border-gray-200 dark:border-gray-900 dark:bg-gray-900 dark:hover:border-gray-700 bg-white focus:ring-4 focus:outline-none focus:ring-gray-300 rounded-full text-base font-medium px-5 py-2.5 text-center mr-3 mb-3 dark:text-white dark:focus:ring-gray-800"
        >
          {{ item.title }}
        </button>
        </div>
      </div>
      
      <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
        <div v-for="img in image">
          <img
            class="h-auto max-w-full rounded-lg"
            :src="img.urls.regular"
            :alt="img.alt_description"
            height="200"
          />
          <div>
            <p>Total Like: {{ img.likes }}</p>
            <p>Author Name: {{ img.user.name }}</p>
            <a :href="img.links.download" :download="img.links.download">Download</a>
          </div>
        </div>
      </div>

    </div>
  </main>
</template>
