<template>
  <div id="contentMain" class="row">
    <div id="historySidebar" v-show="showHistory" class="column">
      <div id="historyTitle">Search History</div>
      <div id="historyList" class="column">
        <div
          class="historyItem"
          v-for="record in history"
          @click="searchAgain(record.title)"
        >
          {{ record.title }} ({{ record.results }})
        </div>
      </div>
    </div>
    <div id="showHideHistory" @click="toggleHistory" v-if="showHistory">
      Hide History
    </div>
    <div id="showHideHistory" @click="toggleHistory" v-else>Show History</div>
    <div id="searchPanel" class="column">
      <div id="searchControls" class="row">
        <input
          id="searchBar"
          type="text"
          v-model="searchTerm"
          @keyup.enter="searchMovies"
        />
        <button id="searchButton" @click="searchMovies">Search</button>
      </div>
      <div id="searchResults" class="wrappedRow" style="gap: 20px;">
        <div class="media" v-for="movie in movies" :key="movie.imdbID">
          <img class="mediaPoster" :src="movie.Poster" style="width: 100%;" />
          <div :class="movie.Type">{{ movie.Type }}</div>
          <div class="mediaTitle">{{ movie.Title }} ({{ movie.Year }})</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      searchTerm: '',
      movies: [],
      resultCount: 0,
      showHistory: true,
      history: [],
    }
  },
  methods: {
    searchMovies() {
      const apiKey = process.env.VUE_APP_OMDB_KEY
      const apiUrl = `https://www.omdbapi.com/?s=${this.searchTerm}&apikey=${apiKey}`

      axios
        .get(apiUrl)
        .then((response) => {
          this.movies = response.data.Search || []
          this.resultCount = response.data.totalResults
          this.history.push({
            title: this.searchTerm,
            results: this.resultCount,
          })
        })
        .catch((error) => {
          console.error('Error fetching data from OMDB:', error)
        })
    },
    toggleHistory() {
      this.showHistory = !this.showHistory
    },
    searchAgain(previousSearch) {
      this.searchTerm = previousSearch
      this.searchMovies()
    },
  },
}
</script>
<style>
/* Begin: Boilerplate Rules */
body {
  margin: 0px;
}

.column {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  gap: 0px;
}

.column * {
  margin: 0px;
}

.row {
  display: flex;
  flex-direction: row;
}

.row * {
  margin: 0px;
}

.wrappedRow {
  display: flex;
  flex-wrap: wrap;
}

.wrappedRow * {
  margin: 0px;
}
/* End: Boilerplate Rules */

/* Begin: Static Page Components */

#contentMain {
  background-color: rgb(39, 39, 39);
  color: white;
  height: 100vh;
}

#searchPanel {
  margin-left: 20px;
  margin-bottom: 40px;
}

#searchControls {
  margin-top: 20px;
  margin-left: 40px;
}
#searchBar {
  height: 43px;
  border: 0px;
  padding: 0px;
  border-top-left-radius: 30px;
  border-bottom-left-radius: 30px;
  width: 45%;
  min-width: 300px;
  font-size: 16pt;
  text-indent: 20px;
  background-color: hsl(29, 20%, 70%);
  box-shadow: 4px 0 6px 0px rgb(15, 15, 15);
}
#searchButton {
  background-color: rgb(240, 142, 50);
  height: 45px;
  font-weight: bold;
  border-bottom-right-radius: 30px;
  border-top-right-radius: 30px;
  padding-right: 12px;
  box-shadow: 4px 0 6px 0px rgb(15, 15, 15);
}

#searchResults {
  margin-top: 20px;
  margin-bottom: 40px;
}
#historySidebar {
  width: 400px;
  max-width: 225px;
  margin-top: 10px;
}

#historyTitle {
  font-weight: bold;
  font-size: 23px;
  align-self: center;
  color: rgb(240, 142, 50);
  margin-bottom: 30px;
  text-shadow: 1px 3px 6px black;
}

#historyList {
  margin-left: 15px;
  gap: 10px;
}

#showHideHistory {
  display: flex;
  padding-top: 40px;
  padding-left: 10px;
  padding-right: 6px;
  background-color: rgb(240, 142, 50);
  color: black;
  font-weight: bold;
  box-shadow: 4px 0 6px 0px rgb(15, 15, 15);
  writing-mode: vertical-rl;
  text-orientation: sideways;
}

/* End: Static Page Components */

/* Begin: Dynamic Components */

.media {
  width: 200px;
  background-color: hsl(29.05deg 11.77% 30.43%);
  padding: 12px;
  border-radius: 10px;
  box-shadow: 8px 5px 9px 0px rgb(15, 15, 15);
}
.mediaPoster {
  border-top: 3px solid;
  border-bottom: 3px solid;

  border-color: rgb(240, 142, 50);
  box-shadow: 8px 5px 9px 0px rgb(15, 15, 15);
  margin-bottom: 10px;
}
.movie {
  background-color: red;
  border-radius: 8px;
  width: fit-content;
  align-content: center;
  padding-top: 3px;
  padding-bottom: 3px;
  padding-left: 4px;
  padding-right: 4px;
  margin: 0 auto;
  font-size: 10pt;
  font-weight: bold;
  margin-bottom: 5px;
  color: black;
}
.series {
  background-color: rgb(0, 153, 255);
  border-radius: 8px;
  width: fit-content;
  align-content: center;
  padding-top: 3px;
  padding-bottom: 3px;
  padding-left: 4px;
  padding-right: 4px;
  margin: 0 auto;
  font-size: 10pt;
  font-weight: bold;
  margin-bottom: 5px;
  color: black;
}
.game {
  background-color: rgb(0, 255, 64);
  border-radius: 8px;
  width: fit-content;
  align-content: center;
  padding-top: 3px;
  padding-bottom: 3px;
  padding-left: 4px;
  padding-right: 4px;
  margin: 0 auto;
  font-size: 10pt;
  font-weight: bold;
  margin-bottom: 5px;
  color: black;
}
.mediaTitle {
  margin: 0 auto;
  width: fit-content;
}
.historyItem {
  background-color: rgb(240, 163, 91);
  color: black;
  padding: 8px;
  width: fit-content;
  border-radius: 15px;
  box-shadow: 8px 5px 9px 0px rgb(15, 15, 15);
}
.historyItem:hover {
  background-color: rgb(226, 133, 45);
  color: black;
  padding: 8px;
  width: fit-content;
  border-radius: 15px;
  box-shadow: 8px 5px 9px 0px rgb(15, 15, 15);
}

/* End: Dynamic Components */
</style>
