<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, watchEffect } from 'vue'

const plants = ref([])

watchEffect(async () => {
  const query = `{ 
    plantCollection {
      items {
        sys {
          id
        }
        commonName
        scientificName
        image {
          url
          description
        }
        wateringSchedule
        lastWatered
        sunlight
        happiness
      }
    }
  }`;

  const fetchUrl = `https://graphql.contentful.com/content/v1/spaces/${import.meta.env.VITE_CONTENTFUL_SPACE_ID}`;

  const fetchOptions = {
        method: "POST",
        headers: {
          Authorization: `Bearer ${import.meta.env.VITE_CONTENTFUL_ACCESS_TOKEN}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ query })
      };

    try {
        const response = await fetch(fetchUrl, fetchOptions).then(response =>
          response.json()
        );
        plants.value = response.data.plantCollection.items;
      } catch (error) {
        throw new Error("Could not receive the data from Contentful!");
      }
})


</script>

<template>
  <header>
    <div class="header-content">
      <div>
        <h1>House Plant Tracker</h1>
        <p class="subtitle">Monitor the happiness of your house plants!</p>
      </div>
      <nav>
        <RouterLink class="nav-link" to="/">Home</RouterLink>
        <RouterLink class="nav-link" to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView :plants="plants"/>
  <footer>
    <div class="footer-content">
      <img alt="Contentful logo" class="logo" src="@/assets/logo.svg" width="50" height="125" />
      <p>Created with <a href="https://www.contentful.com/" target="_blank">Contentful</a> and <a href="https://vuejs.org/" target="_blank">Vue.js</a>.</p>
    </div>
    <div class="credit">Images from 
      <a href="https://unsplash.com/@feeypflanzen?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">feey</a>, <a href="https://unsplash.com/@parkerdesignsss?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Parker Sturdivant</a>, <a href="https://unsplash.com/@nataliekinnear?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Natalie Kinnear</a>, <a href="https://unsplash.com/@cortes?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Dennis Cortés</a>, <a href="https://unsplash.com/@marcblue?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Marc Blue</a>, and <a href="https://unsplash.com/@karaeads?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Kara Eads</a> on <a href="https://unsplash.com/photos/green-plant-on-brown-woven-basket-2LlRY-bMmig?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>.</div>
  </footer>
</template>

<style scoped>

header {
  background-color: white;
  padding: 2rem;
}

.header-content {
  max-width: 1280px;
  margin: auto;
}

.subtitle {
  font-size: 21px;
}

.nav-link {
  color: #2D4A2F;
  text-decoration: none;
  padding: .5rem 1rem;
  border: 1px solid #2D4A2F;
  margin: 1rem 1rem 1rem 0;
  display: inline-block;
  text-transform: uppercase;
}

.router-link-active {
  background-color:  #2D4A2F;
  color: white;
}

footer {
  color: white;
  font-size: 20px;
  font-family: sans-serif;
  padding: 20px;
  background-image: url(/plant-background.jpg);
  background-size: cover;
  background-position: right;
}

.footer-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 1280px;
  margin: auto;
}

.footer-content a {
  color: white;
}

.credit {
  text-align: center;
  font-size: 15px;
}

.credit a {
  color: lightgray;
}

@media screen and (min-width: 992px) {
  .header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .nav-link {
    margin: 0 .5rem;
  }
}

</style>
