<template>
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
    <div class="cardbx-movis__conts d-flex flex-wrap justify-content-between">
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
                  <img v-bind:src="getDataTrending.postr" alt="Movies Poster" />
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
          <button class="glide__arrow glide__arrow--left" data-glide-dir="<">
            prev
          </button>
          <button class="glide__arrow glide__arrow--right" data-glide-dir=">">
            next
          </button>
        </div>
      </div>
    </div>
  </div>

  <div class="cardbx-movis">
    <div class="cardbx-movis__hedr justify-content-between">
      <h5 class="hedr-title">Uncoming</h5>
      <div class="d-flex align-items-end" aria-label="breadcrumb-trending">
        <ul class="breadcrumb mb-0">
          <li class="breadcrumb-item active hedr-tags">2021</li>
        </ul>
      </div>
    </div>
    <div class="cardbx-movis__conts d-flex flex-wrap justify-content-between">
      <div id="uncomingMovis" class="glide">
        <div class="glide__track" data-glide-el="track">
          <ul class="glide__slides">
            <li
              class="glide__slide"
              v-for="getDataUncoming in dataUncoming_Movis"
              v-bind:key="getDataUncoming.id_movis"
            >
              <router-link
                :to="{
                  name: 'MoviesDetail',
                  params: { id: getDataUncoming.id_movis },
                }"
              >
                <div class="movisbx">
                  <div class="movisbx-imags">
                    <img
                      v-bind:src="getDataUncoming.postr"
                      alt="Movies Poster"
                    />
                  </div>
                  <div class="movisbx-rating">
                    <span class="movisbx-rating__title">{{
                      configsDesc[0].title_platform
                    }}</span>
                    <span class="movisbx-rating__number">{{
                      getDataUncoming.rating
                    }}</span>
                  </div>
                </div>
              </router-link>
            </li>
          </ul>
        </div>

        <div class="glide__arrows" data-glide-el="controls">
          <button class="glide__arrow glide__arrow--left" data-glide-dir="<">
            prev
          </button>
          <button class="glide__arrow glide__arrow--right" data-glide-dir=">">
            next
          </button>
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
  "https://api.themoviedb.org/3/trending/movie/day?api_key=32be9fb811dfc2b33294ba5ac30ec365";
let getUpcomingMovis =
  "https://api.themoviedb.org/3/movie/upcoming?api_key=32be9fb811dfc2b33294ba5ac30ec365&language=en-US";

export default {
  name: "CardMovies",
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
    // await axios.get(getTrendingMovis).then((response) => {
    //   if (response.data != null) {
    //   }
    // });

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

    await axios.get(getUpcomingMovis).then((response) => {
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

          this.dataUncoming_Movis.push(setConfigs_Movis);
        }
      }
    });

    let glideConfgs = {
      type: "slider",
      startAt: 0,
      gap: 11,
      keyboard: false,
      rewind: false,
      bound: true,
      touchRatio: 0.5,
      animationDuration: 500,
      perView: 6,
    };

    let queryTrendingMovis = document.querySelector("#trendingMovis.glide");
    let glideTrendingMovis = new Glide(queryTrendingMovis, glideConfgs);

    glideTrendingMovis.on("build.before", () => {
      queryTrendingMovis.childNodes[1].childNodes[0].classList.add("d-none");
    });

    glideTrendingMovis.on(["mount.after", "run"], () => {
      if (glideTrendingMovis.index >= 1) {
        queryTrendingMovis.childNodes[1].childNodes[0].classList.remove(
          "d-none"
        );
      } else {
        queryTrendingMovis.childNodes[1].childNodes[0].classList.add("d-none");
      }

      if (
        glideTrendingMovis.index >=
        queryTrendingMovis.childNodes[0].childNodes[0].children.length -
          glideConfgs.perView
      ) {
        queryTrendingMovis.childNodes[1].childNodes[1].classList.add("d-none");
      } else {
        queryTrendingMovis.childNodes[1].childNodes[1].classList.remove(
          "d-none"
        );
      }
    });

    glideTrendingMovis.mount();

    let queryUncomingMovis = document.querySelector("#uncomingMovis.glide");
    let glideUncomingMovis = new Glide(queryUncomingMovis, glideConfgs);

    glideUncomingMovis.on("build.before", () => {
      queryUncomingMovis.childNodes[1].childNodes[0].classList.add("d-none");
    });

    glideUncomingMovis.on(["mount.after", "run"], () => {
      if (glideUncomingMovis.index >= 1) {
        queryUncomingMovis.childNodes[1].childNodes[0].classList.remove(
          "d-none"
        );
      } else {
        queryUncomingMovis.childNodes[1].childNodes[0].classList.add("d-none");
      }

      if (
        glideUncomingMovis.index >=
        queryUncomingMovis.childNodes[0].childNodes[0].children.length -
          glideConfgs.perView
      ) {
        queryUncomingMovis.childNodes[1].childNodes[1].classList.add("d-none");
      } else {
        queryUncomingMovis.childNodes[1].childNodes[1].classList.remove(
          "d-none"
        );
      }
    });

    glideUncomingMovis.mount();
  },
};
</script>