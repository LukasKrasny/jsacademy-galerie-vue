<template>
  <div>
    <h1>Galerie kreseb | Hana Kroupová</h1>

    <div class="filters">
      <label for="year">Rok:</label>
      <button v-for="year in ['all', ...availableYears]" 
      :key="year" 
      :class="['year-btn', { active: selectedYear === year }]" 
      @click="selectedYear = year">

      {{ year === 'all' ? 'Vše' : year }}
      </button>
    </div>
    
    <div class="gallery">
      <div class="image-card" v-for="img in filteredImages" :key="img.src">
        <div class="image-wrapper">
          <img :src="img.src" :alt="img.title" @click="openLightbox(img)" />
        </div>
        <h3>{{ img.title }}</h3>
        <p class="year-badge">{{ img.year }}</p>
      </div>
    </div>

    <!-- Transition wrapper -->
    <transition name="fade">
      <div class="lightbox" v-if="selectedImage" @click.self="closeLightbox">
        <span class="close" @click="closeLightbox">&times;</span>
        <img :src="selectedImage.src" :alt="selectedImage.title" />
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { images as initialImages } from '../images.js';

const selectedImage = ref(null);

const openLightbox = (img) => {
  selectedImage.value = img;
};

const closeLightbox = () => {
  selectedImage.value = null;
};

// Reaktivní výběr roku
const selectedYear = ref('all');

//Vygeneruj seznam unikátních roků pro dropdown
const availableYears = [...new Set(initialImages.map(img => img.year))].sort((a, b) => b - a);

// Vrací jen obrázky pro vybraný rok nebo všechny
const filteredImages = computed(() => {
  if (selectedYear.value === 'all') return initialImages;
  return initialImages.filter(img => img.year === Number(selectedYear.value));
});
</script>

<style scoped>

h1 {
  text-align: center;
  font-size: 2.5rem;
  color: #e08702;
  font-family: 'Courier New', Courier, monospace;
  margin: 2rem 0 1rem;
}

.filters {
  margin: 1.5rem;
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.year-btn {
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  border: 1px solid #4b4b4b;
  background: #e08702;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s ease;
}

.year-btn.active {
  background-color: #4b4b4b;
  color: #e08702;
  border-color: #e08702;
}

.filters label {
  font-weight: bold;
  font-size: 1.5rem;
  color: #e08702;
  font-family: 'Courier New', Courier, monospace;
  letter-spacing: 0.5px;
  margin-right: 0.5rem;
}

.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1.5rem;
  padding: 1rem;
}

.image-card {
  background: #4b4b4b;
  border-radius: 12px;
  box-shadow: 0 5px 10px #e08702;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 320px; /* pevná výška všech karet */
  padding: 1rem;
  text-align: center;
  transition: transform 0.2s;
}

.image-card:hover {
  transform: scale(1.03);
}

.image-wrapper {
  width: 100%;
  height: 180px;
  overflow: hidden;
  border-radius: 8px;
}

.image-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* důležité: ořízne obrázek do boxu */
  cursor: pointer;
  display: block;
}

.image-card h3 {
  margin: 0.5rem 0 0.25rem;
  font-size: 1.1rem;
  font-family: 'Courier New', Courier, monospace;
  color: #e08702;
}

.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.lightbox img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 8px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

.close {
  position: absolute;
  top: 20px;
  right: 30px;
  font-size: 3rem;
  color: white;
  cursor: pointer;
}

.year-badge {
  background-color: #e08702;
  color: #4b4b4b;
  font-size: 1rem;
  font-weight: 600;
  padding: 0.3rem 0.6rem;
  border-radius: 999px;
  margin-bottom: 0.5rem;
  display: inline-block;
}
</style>
