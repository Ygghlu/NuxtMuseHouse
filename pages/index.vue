<template>
  <div class="content">
    <b-container>
      <hr class="my-4">

      <b-row v-if="searchmet == 1" align-h="center">
        <b-col cols="5">
          <b-form-input v-model="keyword" placeholder="Enter keyword" @keyup.enter="searchdata()" />
        </b-col>
        <b-col cols="2" md="auto">
          <b-button pill @click="searchdata()">
            Search
          </b-button>
          <b-button size="sm" variant="outline" @click="advance()">
            Advance Search
          </b-button>
        </b-col>
      </b-row>
      <b-row v-if="searchmet == 2" align-h="center">
        <b-col cols="5">
          <b-form-input v-model="key" placeholder="Enter keyword" @keyup.enter="searchAdvance()" />
        </b-col>
        <b-col cols="5">
          <b-form-select v-model="selected" :options="options" />
        </b-col>
      </b-row>
      <b-row v-if="searchmet == 2" class="my-4" align-h="center">
        <b-col cols="4" md="auto">
          <b-button pill @click="searchAdvance()">
            Search music
          </b-button>
        </b-col>
      </b-row>
    </b-container>
    <b-container v-if="load == true">
      <b-row>
        <b-col md="6" class="my-1">
          <b-pagination
            v-model="currentPage"
            :total-rows="resultData.length"
            :per-page="perPage"
            first-text="First"
            prev-text="Prev"
            next-text="Next"
            last-text="Last"
          />
        </b-col>
      </b-row>
      <b-card-group
        v-if="selected === 'entity=album&attribute=albumTerm'"
        columns
      >
        <b-card
          v-for="data in resultData.slice(
            (currentPage - 1) * perPage,
            (currentPage - 1) * perPage + perPage
          )"
          :key="data.collectionId"
          no-body
          class="overflow-hidden"
          style="max-width: 540px"
          :per-page="perPage"
          :current-page="currentPage"
        >
          <b-row no-gutters>
            <b-col md="4">
              <a :href="data.collectionViewUrl" target="_blank">
                <b-card-img :src="data.artworkUrl100" class="rounded-0" />
              </a>
            </b-col>
            <b-col md="8">
              <b-card-body :title="data.collectionName">
                <b-card-text>
                  <p>Artist:{{ data.artistName }}</p>
                  <b-button :to="{ name: 'music-idAlbum', params: { id: data } }" pill variant="info">
                    More info
                  </b-button>
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </b-card-group>
      <b-card-group v-else-if="selected === 'entity=allArtist'" columns>
        <b-card
          v-for="data in resultData.slice(
            (currentPage - 1) * perPage,
            (currentPage - 1) * perPage + perPage
          )"
          :key="data.artistId"
          no-body
          class="overflow-hidden"
          style="max-width: 540px"
          :per-page="perPage"
          :current-page="currentPage"
        >
          <b-row no-gutters>
            <b-col>
              <b-card-body :title="data.artistName">
                <b-card-text>
                  <b-button :href="data.artistLinkUrl" target="_blank" pill variant="info">
                    More info
                  </b-button>
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </b-card-group>
      <b-card-group
        v-else-if="selected === 'entity=allTrack&attribute=allTrackTerm'"
        columns
      >
        <b-card
          v-for="data in resultData.slice(
            (currentPage - 1) * perPage,
            (currentPage - 1) * perPage + perPage
          )"
          :key="data.trackId"
          no-body
          class="overflow-hidden"
          style="max-width: 540px"
          :per-page="perPage"
          :current-page="currentPage"
        >
          <b-row no-gutters>
            <b-col md="4">
              <a :href="data.trackViewUrl" target="_blank">
                <b-card-img :src="data.artworkUrl100" class="rounded-0" />
              </a>
            </b-col>
            <b-col md="8">
              <b-card-body :title="data.trackName">
                <b-card-text>
                  <p>Artist:{{ data.artistName }}</p>
                  <p>Album : {{ data.collectionName }}</p>
                  <b-button :to="{ name: 'music-id', params: { id: data } }" pill variant="info">
                    More info
                  </b-button>
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
          <b-row align-h="center">
            <audio controls>
              <source :src="data.previewUrl">
            </audio>
          </b-row>
        </b-card>
      </b-card-group>

      <b-card-group v-else columns>
        <b-card
          v-for="data in resultData.slice(
            (currentPage - 1) * perPage,
            (currentPage - 1) * perPage + perPage
          )"
          :key="data.trackId"
          no-body
          class="overflow-hidden"
          style="max-width: 540px"
          :per-page="perPage"
          :current-page="currentPage"
        >
          <b-row no-gutters>
            <b-col md="4">
              <a :href="data.trackViewUrl" target="_blank">
                <b-card-img :src="data.artworkUrl100" class="rounded-0" />
              </a>
            </b-col>
            <b-col md="8">
              <b-card-body :title="data.trackName">
                <b-card-text>
                  <p>Artist:{{ data.artistName }}</p>
                  <p>Album : {{ data.collectionName }}</p>
                </b-card-text>
                <b-button :to="{ name: 'music-id', params: { id: data } }" pill variant="info">
                  More info
                </b-button>
              </b-card-body>
            </b-col>
          </b-row>
          <b-row align-h="center">
            <audio controls>
              <source :src="data.previewUrl">
            </audio>
          </b-row>
        </b-card>
      </b-card-group>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      resultData: '',
      keyword: '',
      load: false,
      perPage: 6,
      currentPage: 1,
      totalRows: 0,
      key: '',
      selected: null,
      searchmet: 1,
      options: [
        { value: null, text: 'Please select season' },
        { value: 'entity=allTrack&attribute=allTrackTerm', text: 'Track name' },
        { value: 'entity=album&attribute=albumTerm', text: 'Album name' },
        { value: 'entity=allArtist', text: 'Artist Name' }
      ]
    }
  },
  methods: {
    searchdata () {
      axios
        .get('https://itunes.apple.com/search?term=' + this.keyword + '&media=music')
        .then(
          response => (this.resultData = response.data.results),
          (this.load = true),
          (this.currentPage = 1)
        )
        // eslint-disable-next-line no-console
        .catch(error => console.log(error))
    },
    searchAdvance () {
      axios

        .get(
          'https://itunes.apple.com/search?term=' +
            this.key +
            '&' +
            this.selected
        )
        .then(
          response => (this.resultData = response.data.results),
          (this.load = true),
          (this.currentPage = 1)
        )
        // eslint-disable-next-line no-console
        .catch(error => console.log(error))
    },
    advance () {
      this.searchmet = 2
    }
  }
}
</script>

<style>
.container {
  text-align: center;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  display: flexbox;

}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
