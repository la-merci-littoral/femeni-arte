<script setup lang="ts">
import { ref } from 'vue';
import { RouterLink } from 'vue-router';

import Loader from '@/components/Loader.vue';
import BottomMention from '@/components/BottomMention.vue';

import useArtStore from '@/stores/artStore';
import type ArtPiece from '@/types/art';

const galleryLoaded = ref(false)
var artPieces: ArtPiece[] = []

useArtStore().fetchArtPieces().then((artStore) => {
  setTimeout(() => {
    artPieces = artStore!.artPieces
    galleryLoaded.value = true;
  }, 1500);
});

</script>

<template>
  <div id="landing-wrapper" :class="{ 'restrictHeight': !galleryLoaded }">
    <div id="title-section">
      <h1>Femenin'arte</h1>
      <h1 id="subtitle">El Poder del Arte hecho por Mujeres</h1>
      <h2 id="lml">La Merci Littoral</h2>
    </div>
    <div id="gallery" :class="{ 'hidden': !galleryLoaded }">
      <RouterLink :to="{name: 'details', params: {id: item.id}}" v-for="(item, index) in artPieces">
        <img :src="item.lace_imagen" :alt="item.nombre_cuadro" />
      </RouterLink>
    </div>
    <div id="loader-wrapper" v-if="!galleryLoaded">
      <Loader />
    </div>
    <BottomMention />
  </div>
</template>

<style scoped>

#landing-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  padding: 5vh;
  box-sizing: border-box;
  gap: 5vh;
}

#landing-wrapper.restrictHeight {
  height: calc(100svh - 5svh);
}

#title-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  text-align: center;
}

h1 {
  margin-top: 0;
  font-size: 4rem;
  margin-bottom: 0;
}

h1#subtitle {
  margin-top: 0;
  font-size: 2.2rem;
}

h2#lml {
  font-size: 2rem;
  background: rgb(128, 198, 214);
  background: linear-gradient(95deg, rgba(128, 198, 214, 1) 0%, rgba(16, 152, 218, 1) 100%);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

#gallery.hidden {
  opacity: 0;
}

#gallery {
  width: 100%;
  height: fit-content;
  opacity: 1;
  transition: opacity 0.5s ease-in-out;
}

#gallery {
  gap: 30px;
  columns: 3;
}

@media screen and (max-width: 630px) {
  #gallery {
    columns: 2;
  }
}

@media screen and (max-width: 430px) {
  #gallery {
    columns: 1;
  }
}

#gallery a {
  padding-bottom: 30px;
  display: block;
  height: fit-content;
}

#gallery img {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 7px;
}


#loader-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  flex-grow: 1;
}



</style>
