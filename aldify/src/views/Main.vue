<template lang="pug">
.m-5
  .flex.justify-center(v-if="!search")
      button.text-gray-700.rounded-lg.py-1.px-3.mx-2.hover-bg-gray-100(:class="selectedIndex == 0 ? 'bg-gray-200' : ''" @click="select(0)") Songs
      button.text-gray-700.rounded-lg.py-1.px-3.mx-2.hover-bg-gray-100(:class="selectedIndex == 1 ? 'bg-gray-200' : ''" @click="select(1)") Albums
      button.text-gray-700.rounded-lg.py-1.px-3.mx-2.hover-bg-gray-100(:class="selectedIndex == 2 ? 'bg-gray-200' : ''" @click="select(2)") Artists
  .mx-auto.flex.flex-wrap
      c-song-card.mx-2.my-3(v-for="song of songs" :song="song" @click="play(song)")
</template>

<script lang="ts">
import { defineComponent } from "vue";
import BadgeButton from "@/components/BadgeButton.vue";
import Card from "@/components/Card.vue";
import { fetchAllSongs, fetchSongs } from "@/requests";
import { Song } from "@/models/song";
import { player } from "@/player";

export default defineComponent({
  name: "Main",
  components: {
    "c-badge-button": BadgeButton,
    "c-song-card": Card,
  },
  props: ["search"],
  data(): { selectedIndex: number; songs: Song[] } {
    return {
      selectedIndex: 0,
      songs: [],
    };
  },
  async created() {
    this.loadSongs(this.search);
  },
  async beforeRouteUpdate(to, from) {
    this.loadSongs(to.query.search as string);
  },
  methods: {
    select(index: number) {
      this.selectedIndex = index;
    },
    async loadSongs(search: string) {
      this.songs = await fetchSongs(search ? search : "*");
    },
    play(song: Song) {
      player.load(song);
    },
  },
});
</script>
