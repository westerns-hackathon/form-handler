<script setup lang="ts">
import { reactive, toRaw } from 'vue';
import type { UnwrapRef } from 'vue';

let tg: any = window.Telegram.WebApp;

tg.expand();

tg.MainButton.text = "Отправить";
tg.MainButton.show()

// @ts-ignore
Telegram.WebApp.onEvent('mainButtonClicked', () => {
  const formData = JSON.stringify(toRaw(formState))
  tg.sendData(formData);
})

interface FormState {
  address: string;
  category: string;
  commentary: string;
}
const formState: UnwrapRef<FormState> = reactive({
  address: '',
  category: '',
  commentary: '',
});

const categories: string[] = [
    "Ремонт дороги",
    "Уличное освещение",
    "Дорожный знак",
    "Ремонт тротуара",
    "Светофор"
];

</script>

<template>

  <h2 class="text-4xl py-6 text-gray-900 font-extrabold dark:text-white">Обращение жалобой</h2>

  <form class="max-w-screen-md">

    <div class="mb-6">
      <label for="large-input" class="block mb-2 text-base font-medium text-gray-900 dark:text-white">Укажите адрес</label>
      <input
          required
          v-model="formState.address"
          type="text"
          id="large-input"
          class="block w-full p-4 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 text-base focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
    </div>

    <label for="large" class="block mb-2 text-base font-medium text-gray-900 dark:text-white">Выберите категорию</label>
    <select
        required
        v-model="formState.category"
        id="large"
        class="block w-full px-4 py-3 text-base text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
      <template v-for="category in categories" :key="category">
        <option :selected="category === 'Ремонт дороги'" :value="category">{{category}}</option>
      </template>
    </select>

    <label for="message" class="block mb-2 mt-7 text-base font-medium text-gray-900 dark:text-white">Комментарий</label>
    <textarea
        required
        v-model="formState.commentary"
        id="message"
        rows="4"
        class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Write your thoughts here..."></textarea>

<!--    <button @click="() => {console.log(toRaw(formState))}" type="button" class="text-gray-900 bg-gradient-to-r from-teal-200 to-lime-200 hover:bg-gradient-to-l hover:from-teal-200 hover:to-lime-200 focus:ring-4 focus:outline-none focus:ring-lime-200 dark:focus:ring-teal-700 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">Teal to Lime</button>-->

  </form>
</template>