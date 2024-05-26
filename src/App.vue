<template>
  <div id="app">
    <div class="header">MAQE Forum</div>
    <div class="bodyhead">Your current time is: Asia/Bangkok</div>
    <div class="content">
      <Card v-for="card in cards" :key="card.id" :card="card" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Card from './components/Card.vue';

export default {
  name: 'App',
  components: {
    Card
  },
  data() {
    return {
      cards: [],
      loading: true,
      currentTime: new Date().toLocaleString('en-US', { timeZone: 'Asia/Bangkok', weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit', hour12: false })
    };
  },
  async created() {
    try {
      const usersResponse = await axios.get('https://maqe.github.io/json/authors.json');
      const postsResponse = await axios.get('https://maqe.github.io/json/posts.json');

      const users = usersResponse.data;
      const posts = postsResponse.data;

      const mergedData = posts.map(post => {
        const user = users.find(user => user.id === post.author_id);
        return { ...post, author: user ? user.name : 'Unknown', avatar_url: user.avatar_url };
      });

      this.cards = mergedData;
      this.loading = false;
    } catch (error) {
      console.error('Error fetching data', error);
      this.loading = false;
    }
  }
};
</script>

<style>
#app {
  /* text-align: center; */
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #eeeeee;
}

/* .header {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.bodyhead {
  margin-bottom: 20px;
}

.content {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
} */

.header {
  font-size:1.2em;
  font-weight: 700;
  margin:20px;
  width: 80%;
}

.bodyhead {
  width: 80%;
}

.content {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  overflow-y: auto;
  /* height: 100dvh; */
  /* padding: 20px; */
  width: 100%;
  box-sizing: border-box;
}
</style>
