<template>
  <div class="movie_body">
    <Scroller
      @handleToScroll="handleToScroll"
      @handleToTouchEnd="handleToTouchEnd"
    >
      <div class="movie_wrapper">
        <card
          v-for="item in nowPlayingList"
          :key="item.id"
          :info="item"
          :showBtn="showBuyBtn"
          @btnAction="handleBuyTicket"
          @handleToDetail="handleToDetail"
        />
      </div>
    </Scroller>
  </div>
</template>

<script>
import { mapState } from "vuex";
import Card from "@/components/Card/index.vue";
export default {
  name: "nowPlaying",
  components: {
    Card
  },
  computed: {
    ...mapState({
      nm: state => state.nm,
      id: state => state.id
    })
  },
  data() {
    return {
      nowPlayingList: [],
      showBuyBtn: true
    };
  },
  mounted() {
    this.queryData();
  },
  watch: {
    id(newVal) {
      if (newVal) {
        this.queryData();
      }
    }
  },
  methods: {
    queryData() {
      this.$http.get("/api/movieOnInfoList?cityId=" + this.id).then(res => {
        if (res.data.msg === "ok") {
          this.nowPlayingList = res.data.data.movieList;
        }
      });
    },
    handleToScroll(pos) {
      console.log("handleToScroll", pos);
    },
    handleToTouchEnd(pos) {
      if (pos.y > 50) {
        this.queryData();
      }
    },
    handleBuyTicket(params) {
      console.log(params);
    },
    handleToDetail(id) {
      this.$router.push("/movie/detail/n/" + id);
    }
  }
};
</script>

<style scoped>
.movie_body {
  flex: 1;
  overflow: auto;
  position: relative;
}
.movie_wrapper {
  position: relative;
  margin: 0 12px;
}
</style>
