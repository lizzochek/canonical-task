<template>
  <h1>Canonical cards</h1>

  <div class="container" v-if="!error">
    <div class="row u-equal-height u-clearfix">
      <div v-for="card in cardsData" :key="card.id" class="col-4 p-card--highlighted card">
        <div class="header">{{ card.title.rendered }}</div>
        <div class="content">
          <div class="media">
            <a :href="card.link">
              <img :src="card['featured_media']" alt="Blog post image" />
            </a>
          </div>
          <h3 class="p-heading--4 title">
            <a :href="card.link">{{ card.title.rendered }}</a>
          </h3>
          <p class="post-date">
            By <a :href="card._embedded.author[0].link">{{ card._embedded.author[0].name }}</a>
            on {{ card._start_day + ' ' + new Date(card.date).toLocaleString('default', { month: 'long' })
              +
              ' ' + card._start_year }}
          </p>
        </div>
        <p class="footer">{{ card.type }}</p>
      </div>
    </div>
  </div>
  <div id="error" v-else>Sorry, something went wrong. Please try again later</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      error: null,
      cardsData: [],
    }
  },
  created() {
    this.error = null;
    fetch('https://people.canonical.com/~anthonydillon/wp-json/wp/v2/posts.json')
      .then(async res => {
        this.cardsData = await res.json();
      })
      .catch(err => this.error = err);
  }
}
</script>

<style>
@import "./App.scss";
</style>