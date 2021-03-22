<template>
  <div>Halamana Detail</div>
  <div class="conts-main">
    <div class="container">
      <div class="cardbx-movis">
        <div class="cardbx-movis__hedr justify-content-between">
          <h5 class="hedr-title">Trending</h5>
          <div class="d-flex align-items-end" aria-label="breadcrumb-trending">
            <ul class="breadcrumb mb-0">
              <li class="breadcrumb-item active hedr-tags">Today</li>
              <li class="breadcrumb-item hedr-tags">This Week</li>
            </ul>
          </div>
        </div>
        <div
          class="cardbx-movis__conts d-flex flex-wrap justify-content-between"
        >
          <div id="trendingMovis" class="glide">
            <div class="glide__track" data-glide-el="track">
              <ul class="glide__slides">
                <li
                  class="glide__slide"
                  v-for="getDataTrending in dataTrending_Movis"
                  v-bind:key="getDataTrending.id_movis"
                >
                  <div class="movisbx">
                    <div class="movisbx-imags">
                      <img
                        v-bind:src="getDataTrending.postr"
                        alt="Movies Poster"
                      />
                    </div>
                    <div class="movisbx-rating">
                      <span class="movisbx-rating__title">{{
                        configsDesc[0].title_platform
                      }}</span>
                      <span class="movisbx-rating__number">{{
                        getDataTrending.rating
                      }}</span>
                    </div>
                  </div>
                </li>
              </ul>
            </div>

            <div class="glide__arrows" data-glide-el="controls">
              <button
                class="glide__arrow glide__arrow--left"
                data-glide-dir="<"
              >
                prev
              </button>
              <button
                class="glide__arrow glide__arrow--right"
                data-glide-dir=">"
              >
                next
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Glide from "@glidejs/glide/dist/glide.esm";

let configsMovis =
  "https://api.themoviedb.org/3/configuration?api_key=32be9fb811dfc2b33294ba5ac30ec365";

let getTrendingMovis =
  "https://api.themoviedb.org/3/discover/movie?api_key=32be9fb811dfc2b33294ba5ac30ec365&language=en-US&primary_release_year=2021";

export default {
  name: "MoviesDetail",
  data() {
    return {
      configsDesc: [
        {
          title_platform: "IMDB",
          url_imags: "",
          size_poster: "",
        },
      ],
      dataTrending_Movis: [],
      dataUncoming_Movis: [],
    };
  },

  async created() {
    await axios.get(configsMovis).then((response) => {
      if (response.data != null) {
        this.configsDesc.url_imags = response.data.images.secure_base_url;
        this.configsDesc.size_poster = response.data.images.poster_sizes[3];
      }
    });
  },

  async mounted() {
    await axios.get(getTrendingMovis).then((response) => {
      if (response.data != null) {
        for (let i = 0; i < response.data.results.length; i++) {
          let imags_movis;

          imags_movis =
            this.configsDesc.url_imags +
            this.configsDesc.size_poster +
            response.data.results[i].poster_path;

          const setConfigs_Movis = {
            id_movis: response.data.results[i].id,
            postr: imags_movis,
            title: response.data.results[i].title,
            rating: response.data.results[i].vote_average,
          };

          this.dataTrending_Movis.push(setConfigs_Movis);
        }
      }
    });

    let glideConfgs = {
      type: "slider",
      startAt: 0,
      gap: 11,
      keyboard: true,
      rewind: false,
      touchRatio: 0.5,
      animationDuration: 500,
      perView: 6,
    };

    let queryTrendingMovis = document.querySelector("#trendingMovis.glide");

    new Glide(queryTrendingMovis, glideConfgs).mount();
  },
};
</script>