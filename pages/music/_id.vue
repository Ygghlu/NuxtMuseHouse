<template>
  <div>
    <b-container>
      <b-card>
        <b-card
          no-body
          class="overflow-hidden"
        >
          <b-row no-gutters>
            <b-col md="6">
              <a :href="data.trackViewUrl" target="_blank">
                <b-img center thumbnail width="500%" class="rounded-0" :src="data.artworkUrl100" />
              </a>
            </b-col>
            <b-col md="6">
              <b-card-body :title="data.trackName">
                <b-card-text>
                  <p>Artist: <a :href="data.artistViewUrl" target="_blank">{{ data.artistName }}</a> </p>
                  <p>Album : <a :href="data.collectionViewUrl">{{ data.collectionName }}</a> </p>
                  <p>Track Number: {{ data.trackNumber }}/{{ data.trackCount }}</p>
                  <p>Explicit : {{ data.trackExplicitness }}</p>
                  <p>Genre : {{ data.primaryGenreName }}</p>
                  <p>Release Date : {{ data.releaseDate }}</p>
                  <p>Track Price : {{ data.trackPrice }} {{ data.currency }}</p>
                  <p>Full Album price : {{ data.collectionPrice }} {{ data.currency }}</p>
                  <b-button pill variant="primary" :href="data.trackViewUrl" target="_blank">
                    View on Apple Music
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
      </b-card>
    </b-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      data: this.$route.params.id
    }
  }
}
</script>

<style>

</style>
