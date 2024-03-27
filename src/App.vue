<script setup>
import { ref } from "vue"
import axios from "axios"
let image = ref(null)

const onchange = (e) => {
  image.value = e.target.files[0]
}

const onDragover = (e) => {
  e.preventDefault()
  e.dataTransfer.dropEffect = "copy"
}
const onDragleave = (e) => {
  e.preventDefault()
}
const ondrop = (e) => {
  image.value = e.dataTransfer.files[0]
}

const submit = () => {
  let data = new FormData()
  data.append("user_id", 1)
  data.append("title", image.value.name)
  data.append("path", image.value)

  axios
    .post("http://127.0.0.1:8000/api/v1/movies", data)
    .then(function (response) {
      console.log(response.data, "response axios")
    })
    .catch(function (error) {
      console.log(error, "erros axios")
    })
}
</script>

<template>
  <div class="bg-gray-100 h-screen flex items-center justify-center p-3">
    <div class="w-full max-w-md p-9 bg-white rounded-lg shadow-lg">
      <h1 class="text-center text-2xl sm:text-2xl font-semibold mb-4 text-gray-800">
        File Drop and Upload
      </h1>
      <form @submit.prevent="submit" class="flex flex-col justify-center">
        <div
          class="bg-gray-100 p-8 text-center rounded-lg border-dashed border-2 border-gray-300 hover:border-blue-500 transition duration-300 ease-in-out transform hover:scale-105 hover:shadow-md"
          id="dropzone"
          @dragover.prevent="onDragover"
          @dragleave.prevent="onDragleave"
          @drop.prevent="ondrop"
        >
          <label for="fileInput" class="cursor-pointer flex flex-col items-center space-y-2">
            <svg
              class="w-16 h-16 text-gray-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6v6m0 0v6m0-6h6m-6 0H6"
              ></path>
            </svg>
            <span class="text-gray-600">Drag and drop your files here</span>
            <span class="text-gray-500 text-sm">(or click to select)</span>
          </label>
          <input type="file" @change="onchange" id="fileInput" class="hidden" />
        </div>
        <button class="py-3 px-7 mt-5 border border-solid border-red-500" type="submit">
          upload
        </button>
      </form>

      <div class="mt-6 text-center" id="fileList">{{ image }}</div>
    </div>
  </div>
</template>

<style scoped></style>
