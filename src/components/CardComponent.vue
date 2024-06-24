<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const currentData = ref(null);
    const error = ref(null);
    const id = ref(null);
    const apiUrl = 'https://api.adviceslip.com/advice'; // Replace with your actual URL

    const getRandomAdvice = async () => {
      try {
        const response = await axios.get(apiUrl);
        id.value = response.data.slip.id;
        currentData.value = response.data.slip.advice;
        error.value = null;

        //save to local storage
        localStorage.setItem('advice', JSON.stringify({id: id.value, advice: currentData.value}));

      } catch (err) {
        error.value = 'Error fetching data';
        console.error('Axios error: ', err);
      }
    };

    onMounted(() => {
      const storageAdvice = localStorage.getItem('advice');
      if (storageAdvice) {
        const storageAdviceData = JSON.parse(storageAdvice);
        id.value = storageAdviceData.id;
        currentData.value = storageAdviceData.advice;
      }
    });


    return {
      id,
      currentData,
      error,
      getRandomAdvice
    };
  }
};
</script>


<template>
<div class="bg-[#303a49] rounded-2xl flex flex-col gap-6 justify-center items-center p-10 relative">

    <div
        v-if="id"
        class="text-[#52fead] tracking-wide"

    >
      ADVICE #{{ id }}

    </div>

    <div
        v-if="currentData"
        class="text-white text-2xl text-center font-serif"
    >
      &#8220;{{ currentData }}&#8221;
    </div>

    <p v-if="error">{{ error }}</p>

    <img src="/src/assets/images/divider.svg" alt="divider_svg_img" class="m-2">
    <!--    button-->
    <button
        @click="getRandomAdvice"
        class="bg-[#52fead] rounded-[50%] w-[50px] h-[50px] flex items-center justify-center absolute bottom-[-25px]"

    >
      <img src="/src/assets/icons/dice-icon.png" alt="dice_icon" class="w-[32px] h-[32px]">
    </button>

</div>

</template>


<style scoped></style>