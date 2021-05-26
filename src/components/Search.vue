<template>
  <div class="search">
    <!-- <h2>Type In Your Search Term</h2>
    <form v-on:submit.prevent="getResult(query)"> -->
      <h2>Browse a subreddit</h2>
    <form v-on:submit.prevent="getResult(query)">
      <input type="text" placeholder="Type in your search" v-model="query" />
    </form>
    <h2 class="errorMessage">{{ errormessage }}</h2>
    <div class="results" v-if="results">
      <br>
      <i>Browsing: {{ query }}</i>
      <div v-for="result in results">
          <h2 class="title"><a v-bind:href="result.data.url" />{{ result.data.title }}</h2>
          <p class="score">Score: {{result.data.score}}</p>
          <p class="updown">Upvotes: {{result.data.ups}} Downvotes: {{result.data.downs}}</p> 
            <div v-if="result.data.url_overridden_by_dest">
              <p class="subredditType">Subreddit Type: {{result.data.subreddit_type}}</p> 
              <img v-bind:src="result.data.thumbnail" v-bind:height="result.data.thumbnail_height" v-bind:width="result.data.thumbnail_width"/> 
            </div>
            <div v-else>
              <p class="subredditType">Subreddit Type: {{result.data.subreddit_type}}</p> 
              <img v-bind:src="result.data.thumbnail" v-bind:height="result.data.thumbnail_height" v-bind:width="result.data.thumbnail_width"/> 
            </div>
          <p class="selftext">{{result.data.selftext}}</p>
      </div>
    </div>
  </div>
</template>

<script>

          
//  <img v-bind:src="result.links[0].href" />

// axios.get('https://images-api.nasa.gov/search?q=' + query + '&media_type=image').then( response => {
//             console.log(response.data.collection.items);
//             this.results = response.data.collection.items;
//         });
import axios from 'axios';
export default {
  name: 'search',
  data () {
    return {
      msg: 'Enter a subreddit that you would like to browse',
      query: '',
      results: '',
      errormessage: ''
    }
  },
  methods: {
      getResult(query) {
        axios.get("https://www.reddit.com/r/" + query + ".json")
        .then( response => {
            console.log(response.data.data.children);
            // console.log(response.data.data.children.preview.images);
            var res = response.data.data.children;
            console.warn(res[0].data.score);
            this.results = res.sort((a,b) => a.data.score < b.data.score)
            this.errormessage = '';
            }).catch((error) => { 
              console.warn('Not good man :(');
              this.results = '';
              this.errormessage = "Sorry, I couldn't find that subreddit. Please try again"
            });
            /*
            Need to loop through all of the returns and display the info 
            this.results = response.data.data.children[0].data; ++
            */
        
      }
  }
  // methods: {
  //     getResult(query) {
  //       axios.get('https://images-api.nasa.gov/search?q=' + query + '&media_type=image').then( response => {
  //           console.log(response.data.collection.items);
  //           this.results = response.data.collection.items;
  //       });
  //     }
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.results img {
    margin: 10px;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
