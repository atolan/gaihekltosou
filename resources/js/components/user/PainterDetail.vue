<template>
  <Detail v-if="inner_painter_id != null" :painter_id="inner_painter_id">
    <template v-slot:btn_consul>
      <a href="#" class="btn-confirm" @click="clickConsul()">相談する</a>
    </template>

    <template v-slot:btn_favorite>
      <div class="favorite-box">
        <p class="favorite-label">お気に入り登録</p>
        <div id="fav-icon" class="favorite-icon" @click="clickFavorite()"></div>
      </div>
    </template>

    <template v-slot:btn_example>
      <a :href="exampleLink" class="show-btn">施工事例を見る</a>
    </template>

    <template v-slot:btn_column>
      <a :href="columnLink" class="show-btn">コラムを見る</a>
    </template>
  </Detail>
</template>

<script>
import { Web } from "js/route/user.js";
import { mapGetters } from 'vuex';
import Detail from "js/components/painter/Detail.vue";

export default {
  props: {
    painter_id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      inner_painter_id: null,
    }
  },
  computed: {
    ...mapGetters('Painter', [
      'findPainter',
    ]),
    exampleLink() {
      return Web.example.painter(this.painter_id);
    },
    columnLink() {
      return Web.column.painter(this.painter_id);
    },
  },
  created() {
    this.$store.dispatch('Painter/LoadPainter', this.painter_id).then(() => {
      this.inner_painter_id = this.findPainter(this.painter_id).id;

      this.$store.dispatch('Example/loadPainterExamples', this.painter_id);
      this.$store.dispatch('Column/loadPainterColumns', this.painter_id);
    });
  },
  methods: {
    clickConsul() {
    },
    clickFavorite() {
    },
  },
  components: {
    Detail,
  },
};
</script>