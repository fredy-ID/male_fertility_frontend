<template>
  <main class="flex justify-center items-center flex-col">
    <div class="alert alert-success w-96 my-3" v-if="responseData">
      <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
      <span>Résultat : "{{ responseData.predicted_class }}"</span>
    </div>
    <div class="card w-96 bg-base-100 shadow-xl">
      <div class="h-44 bg-gray-200 flex justify-center items-center">
        <img v-if="selectedImage" :src="selectedImage" alt="Selected" class="w-full h-full object-cover">
      </div>
      <div class="card-body">
        <h2 class="card-title">Ajouter une image</h2>
        <input type="file" class="file-input file-input-bordered file-input-md w-full max-w-xs" @change="handleImageSelect" />
        <div class="card-actions justify-end">
          <button class="btn btn-primary" @click="analyzeImage">Analyser</button>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Prediction {
  predicted_class: string;
}

const selectedImage = ref<File | null>(null);
const responseData = ref<Prediction>();

const handleImageSelect = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files && target.files.length > 0) {
    selectedImage.value = target.files[0];
  }
};
  
const analyzeImage = async () => {
  if (selectedImage.value) {
    const formData = new FormData();
    formData.append('image', selectedImage.value);
    console.log(selectedImage.value)

    try {
      const response = await fetch('http://simplon.fredy-mc.fr/backend-male-fertility/api/scan/', {
        method: 'POST',
        body: formData,
      });

      responseData.value = await response.json();
      console.log('Response Data:', responseData);
    } catch (error) {
      console.error('An error occurred:', error);
    }
  }
};





// const analyzeImage = async () => {
//   if (selectedImage.value) {
//     const formData = new FormData();
//     formData.append('image', selectedImage.value);

//     try {
//       // const response = await fetch('http://simplon.fredy-mc.fr/male-fertility/api/scan/', {
//       const response = await fetch('http://localhost:8000/api/scan/', {
//         method: 'POST',
//         body: formData,
//       });

//       // Handle the response here
//       console.log('Response:', response);
//     } catch (error) {
//       console.error('An error occurred:', error);
//     }
//   }
// };
</script>
