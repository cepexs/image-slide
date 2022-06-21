<script lang="ts" setup>
import { ref } from "vue";

interface footerImage {
  href: String;
  caption: String;
}

const previewImage = ref(<footerImage[]>[]);
const step = ref(0);
const footerImage = ref([
  {
    href: "https://picsum.photos/200/300?random=1",
    caption: "gambar 1",
  },
  {
    href: "https://picsum.photos/200/300?random=2",
    caption: "gambar 2",
  },
  {
    href: "https://picsum.photos/200/300?random=3",
    caption: "gambar 3",
  },
]);

const slideClass = ref("slide-fade");

previewImage.value = [];

previewImage.value.push(footerImage.value[step.value]);

const next = () => {
  slideClass.value = "slide-next";
  if (step.value <= footerImage.value.length - 2) {
    step.value++;
    previewImage.value.push(footerImage.value[step.value]);
    previewImage.value.shift();
  } else {
    previewImage.value = [];
    step.value = 0;
    previewImage.value.push(footerImage.value[step.value]);
  }
};

const prev = () => {
  slideClass.value = "slide-back";
  if (step.value >= 1) {
    step.value--;
    previewImage.value.push(footerImage.value[step.value]);
    previewImage.value.shift();
  } else {
    previewImage.value = [];
    step.value = footerImage.value.length - 1;
    previewImage.value.push(footerImage.value[step.value]);
  }
};

const changePreview = (i) => {
  slideClass.value = "slide-zoom";
  step.value = i;
  previewImage.value.push(footerImage.value[step.value]);
  previewImage.value.shift();
};
</script>

<template>
  <div class="relative w-full min-h-screen flex flex-col">
    <div
      class="absolute h-2/3 w-full flex flex-col justify-center items-center"
    >
      <transition-group taga="div" :name="slideClass" mode="out-in">
        <div
          :id="`${item.caption}}`"
          class="absolute"
          v-for="(item, x) in previewImage"
          :key="`${item.caption}`"
        >
          <h1 class="text-center p-4">{{ item.caption }}</h1>
          <img class="border rounded-lg shadow-lg" :src="`${item.href}`" />
        </div>
      </transition-group>
    </div>

    <div class="absolute top-0 h-2/3 w-full flex flex-row justify-between">
      <button
        class="w-1/4 flex flex-col justify-center items-end group"
        @click="prev"
      >
        <svg
          class="w-6 h-6 transition duration-300 group-hover:-translate-x-4"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 19l-7-7 7-7"
          ></path>
        </svg>
      </button>
      <button
        class="w-1/4 flex flex-col justify-center items-start group"
        @click="next"
      >
        <svg
          class="w-6 h-6 transition duration-300 group-hover:translate-x-4"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M9 5l7 7-7 7"
          ></path>
        </svg>
      </button>
    </div>

    <div
      class="absolute bottom-0 h-1/3 w-full flex flex-row justify-center items-center gap-4 py-8"
    >
      <img
        v-for="(item, i) in footerImage"
        :key="i"
        class="w-24 rounded-lg cursor-pointer border-2 border-opacity-0 transition duration-500 ease-in-out"
        :class="[
          i === step ? 'border-purple-800 border-opacity-100 scale-105' : '',
        ]"
        @click="changePreview(i)"
        :src="item.href"
      />
    </div>
  </div>
</template>

<style scoped>
/* vue transition */

/* zoom */
.slide-zoom-enter-active {
  transition: all 0.8s ease;
}
.slide-zoom-enter-from {
  transform: scale(0.5);
  opacity: 0;
}

.slide-zoom-leave-active {
  transition: all 0.7s cubic-bezier(1, 0.5, 0.3, 1);
}

.slide-zoom-leave-to {
  transform: scale(0.5);
  opacity: 0;
}

/* next */
.slide-next-enter-active {
  transition: all 1s ease;
}
.slide-next-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.3, 1);
}
.slide-next-enter-from {
  transform: translateX(-200px);
  opacity: 0;
}

.slide-next-leave-to {
  transform: translateX(200px);
  opacity: 0;
}

/* back/prev */
.slide-back-enter-active {
  transition: all 1s ease;
}
.slide-back-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.3, 1);
}
.slide-back-enter-from {
  transform: translateX(200px);
  opacity: 0;
}
.slide-back-leave-to {
  transform: translateX(-200px);
  opacity: 0;
}
</style>
