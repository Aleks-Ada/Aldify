<template lang="pug">
.flex.w-72
  img.w-12.h-12.mx-1(:src="playProgress.image")
  div.w-full
    .flex.justify-between
      div
        p {{ playProgress.title }}
        p.text-sm.text-gray-400 {{ playProgress.artist }}
      .flex.content-center.place-items-center
        font-awesome-icon.mx-1(:icon="['fas', 'backward-step']")
        font-awesome-icon.mx-1.rounded-full.text-gray-800.bg-gray-100.p-1.w-4.h-4(:icon="['fas', playProgress.playing ? 'pause' : 'play']" @click="toggle")
        font-awesome-icon.mx-1(:icon="['fas', 'forward-step']")
    .bg-black.h-2.rounded
      .bg-gray-200.h-full.rounded(:style="`width: ${playProgress.progress * 100}%`")
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { PlayProgress } from "@/models/play-progress";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { player } from "@/player";

export default defineComponent({
  name: "MusicBadge",
  props: ["playProgress"],
  methods: {
    toggle() {
      player.toggle();
    },
  },
});
</script>
