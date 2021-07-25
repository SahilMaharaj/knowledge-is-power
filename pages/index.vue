<template>
  <div class="main-wrapper">
    <form @submit.prevent="fetchResults">
      <div class="search-bar">
        <input v-model="input" class="input" type="text" placeholder="Search Entities..." required />
        <button class="search-btn">Search</button>
      </div>

      <div class="selectors">
        <div class="limit">
          <select class="sl-limit" v-model="limit" name="limit" id="limit" required>
            <option value="" disabled selected>Limit</option>
            <option value="10">10</option>
            <option value="20">20</option>
            <option value="50">50</option>
            <option value="100">100</option>
          </select>
        </div>

        <div class="type">
          <select class="sl-type" v-model="type" name="type" id="type" required>
            <option value="" disabled selected>Type</option>
            <option value="any">Any Type</option>
            <option value="Person">Person</option>
            <option value="Place">Place</option>
            <option value="LocalBusiness">Local Business</option>
            <option value="Organization">Organization</option>
            <option value="WebSite">WebSite</option>
            <option value="Book">Book</option>
            <option value="BookSeries">Book Series</option>
            <option value="EducationalOrganization">Educational Organization</option>
            <option value="Event">Event</option>
            <option value="GovernmentOrganization">GovernmentOrganization</option>
            <option value="Movie">Movie</option>
            <option value="MovieSeries">Movie Series</option>
            <option value="MusicAlbum">Music Album</option>
            <option value="MusicGroup">Music Group</option>
            <option value="MusicRecording">Music Recording</option>
            <option value="Periodical">Periodical</option>
            <option value="SportsTeam">Sports Team</option>
            <option value="TVEpisode">TV Episode</option>
            <option value="TVSeries">TV Series</option>
            <option value="VideoGame">Video Game</option>
            <option value="VideoGameSeries">Video Game Series</option>
          </select>
        </div>
      </div>
    </form>

    <div class="results" v-for="entity in results.itemListElement" :key="entity.name">
      <div class="result-wrapper">
        <div class="entity-name">
          <h2>{{ entity.result.name }}</h2>
        </div>
        <div class="entity-desc">
          <h3>{{ entity.result.description }}</h3>
        </div>
        <div class="entity-types" v-if="entity.result['@type']">
          {{ entity.result["@type"].join(" | ") }}
        </div>
        <div v-for="desc in entity.result" :key="desc.articleBody">
          <div class="entity-longDesc">
            <p>{{ desc.articleBody }}</p>
            <p class="entity-desc-source" v-if="desc.url">Source: <a target="_blank" :href="desc.url">Wikipedia</a></p>
            </div>
        </div>

        <div class="entity-btns">
          <div class="entity-on-goog">
            <button><a :href="`https://www.google.com/search?q=${entity.result.name}+&kponly&kgmid=${entity.result['@id'].substring(3)}`" target="_blank"><font-awesome-icon :icon="['fab', 'google']"  /> View on Google</a></button>
          </div>
          <div v-if="entity.result.url" class="entity-website">
            <button>
              <a target="_blank" :href="entity.result.url"><font-awesome-icon :icon="['fas', 'globe-africa']"  /> Entity Website</a>
            </button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="loading"><Loader /></div>
    <div>{{ noResults }}</div> 

    <!--- ERROR --->
    <div class="error-message" v-if="error">
        Oops, an error occurred.
    </div>

  </div>
</template>

<script>
export default {
  layout: 'default',
  head: {
    title: "Knowledge Graph Search | Sahil Maharaj",
    meta: [
      {
        hid: 'description',
        name: 'description',
        content: "Search for knowledge graphs directly from Google's API. Built by Sahil Maharaj - Jellyfish."
      }
    ]
  },
  data() {
    return {
      input: '',
      error: '',
      results: '',
      limit: '',
      type: '',
      noResults: '',
      loading: false
    }
  },
  methods: {
    async fetchResults() {
      try {
        const userQuery = this.input
        const setLimit = this.limit
        const setType = this.type
        this.results = ''
        this.noResults = ''
        this.loading = true
        const results = await this.$axios.$get(`https://kgsearch.googleapis.com/v1/entities:search?limit=${setLimit}&query=${userQuery}&types=${setType}&key=AIzaSyA38VQCpP0Tk2ahl1xj9a428QCe8e2IhtM`)
        
        this.results = results
        this.error = ''
        this.noResults = ''
        
        if(results.itemListElement.length === 0) {
          this.noResults = "No Results."
        }
        
        this.loading = false
        return { results }
      } catch (error) {
          this.error = error
          return { error }
      } 
    },
    start() {
      this.loading = true
    },
    finish() {
      this.loading - false
    }
  }
}
</script>

<style scoped>
.main-wrapper {
  margin-bottom: 30px;
}

form {
  margin-bottom: 30px;
}

.search-bar {
  display: flex;
}

.input {
  flex: 12;
  font-family: 'Helvetica Neue Medium';
  font-weight: 100;
  padding: 15px;
  margin-right: 10px;
  font-size: 16px;
  color: #000;
  border: 1px solid grey;
  border-radius: 4px;
}

.input::placeholder {
  font-family: 'Helvetica Neue Thin';
}

.search-btn {
  flex: 1;
  font-family: 'Helvetica Neue Thin';
  font-size: 16px;
  cursor: pointer;
  color: #fff;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  background: #f09433;
  background: linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  background: -moz-linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); 
  background: -webkit-linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
}

.selectors {
  display: flex;
  margin-top: 15px;
}

.limit, .type {
    flex: 1;
}

.limit {
  margin-right: 10px;
}

.sl-limit, .sl-type {
  -webkit-appearance: none; 
  -moz-appearance: none;
  appearance: none;
  background: url('~/static/down-arrow.png') no-repeat;
  background-position: right center;
  width: 100%;
  font-family: 'Helvetica Neue Medium';
  font-size: 16px;
  padding: 15px !important;
  border: 1px solid grey;
  border-radius: 4px;
  cursor: pointer;
  color: #000;
}

option {
  cursor: pointer;
}

.results {
  list-style: none;
  padding: 0;
  margin: 0;
}

.result-wrapper {
  border: 1px solid grey;
  border-radius: 4px;
  box-shadow: 3px 3px 3px grey;
  margin-top: 15px;
  padding: 15px;
}

.entity-desc h3 {
  font-size: 16px;
  margin-bottom: 10px;
}

.entity-desc-source {
  margin-top: 10px;
  font-style: italic;
}

.entity-types {
  margin-bottom: 10px;
}

.entity-btns {
  display: flex;
}

.entity-website button {
  padding: 0;
  border: none;
  background: none;
}

.entity-website a {
  display: block;
  background: #f09433;
  background: linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  background: -moz-linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); 
  background: -webkit-linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  width: 150px;
  font-size: 16px;
  color: #fff;
  text-decoration: none;
  text-align: center;
  padding: 10px;
  border-radius: 4px;
  margin-top: 15px;
}

.entity-on-goog {
  margin-right: 10px;
}

.entity-on-goog button {
  border: none;
  background: none;
  padding: 0;
}

.entity-on-goog a {
  display: block;
  background: #f09433;
  background: linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  background: -moz-linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); 
  background: -webkit-linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  width: 160px;
  font-size: 16px;
  color: #fff;
  text-decoration: none;
  text-align: center;
  padding: 10px;
  border-radius: 4px;
  margin-top: 15px;
}

@media all and (max-width: 400px) {
  .entity-btns {
    flex-direction: column;
  }

  .entity-on-goog {
    margin: 0;
  }

  .entity-btns button {
    width: 100%;
  }

  .entity-on-goog a, .entity-website a {
    width: 100%;
  }
}
</style>
