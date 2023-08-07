<template>
  <main class="flex justify-center items-center">
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

const selectedImage = ref<File | null>(null);

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
      const response = await fetch('http://localhost:8000/api/scan/', {
        method: 'POST',
        body: formData,
      });

      const responseData = await response.json();
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
