<script setup>
import { ref, watch } from 'vue';

const apodDateMilliseconds = ref(Date.now());
const apodData = ref(null);
const loading = ref(true);

const fetchApod = async () => {
  loading.value = true;

  const apodDate =
    apodDateMilliseconds.value === 0
      ? new Date()
      : new Date(apodDateMilliseconds.value);

  const response = await fetch(
    `http://localhost:14000/api/apods/?date=${
      apodDate.toISOString().split('T')[0] || ''
    }`
  );

  apodData.value = await response.json();
  loading.value = false;
};

const handlePrevious = () => {
  apodDateMilliseconds.value = apodDateMilliseconds.value - 86400000;
};

const handleNext = () => {
  apodDateMilliseconds.value = apodDateMilliseconds.value + 86400000;
};

watch(apodDateMilliseconds, () => {
  fetchApod();
});

fetchApod();
</script>

<template>
  <div class="notifications">
    <div v-if="loading">Loading...</div>
  </div>

  <div v-if="apodData" class="apod-demo">
    <div class="apod-title">
      <h1 class="green">NASA APOD for {{ apodData.date }}</h1>
    </div>
    <div class="apod-content">
      <div class="apod-media">
        <div class="navigation-buttons">
          <button @click="handlePrevious" class="button">
            <i class="fa fa-arrow-left fa-2x"></i>
          </button>
          <div v-if="apodData.date != new Date().toISOString().split('T')[0]">
            <button @click="handleNext" class="button">
              <i class="fa fa-arrow-right fa-2x"></i>
            </button>
          </div>
        </div>
        <div v-if="apodData.media_type == 'image'">
          <img v-bind:src="apodData.url" width="420" height="315" />
        </div>
        <div v-if="apodData.media_type == 'video'">
          <iframe
            width="420"
            height="315"
            v-bind:src="apodData.url + '&autoplay=1&mute=1'"
          ></iframe>
        </div>
      </div>
      <div class="apod-explanation">{{ apodData.explanation }}</div>
    </div>
  </div>
</template>

<style scoped>
.notifications {
  min-height: 5vh;
  text-align: center;
}
.apod-content {
  min-height: 50%;
  display: flex;
}
.apod-title {
  width: 100%;
  text-align: center;
  margin: 0px 0px 20px 0px;
}
.apod-media {
  display: block;
  margin: 0px 20px;
}
.navigation-buttons {
  display: flex;
  justify-content: space-around;
  margin: 10px 0px;
}
.button {
  margin: 0px 20px;
}
.apod-explanation {
  margin: 20px 0px;
}
</style>
