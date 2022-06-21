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

previewImage.value = [];

previewImage.value.push(footerImage.value[step.value]);

const next = () => {
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
      <transition-group name="slide-next relative" mode="out-in">
        <div
          :id="`${x}`"
          class="absolute"
          v-for="(item, x) in previewImage"
          :key="x"
        >
          <h1 class="text-center p-4">{{ item.caption }}</h1>
          <img class="border rounded-lg shadow-lg" :src="`${item.href}`" />
        </div>
      </transition-group>
    </div>

    <div class="absolute top-0 h-2/3 w-full flex flex-row justify-between">
      <button
        class="w-1/4 flex flex-col justify-center items-end"
        @click="prev"
      >
        <svg
          class="w-6 h-6"
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
        class="w-1/4 flex flex-col justify-center items-start"
        @click="next"
      >
        <svg
          class="w-6 h-6"
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
        class="w-24 rounded-lg cursor-pointer border-2 border-opacity-0 active:border selection:border-4"
        :class="[i === step ? 'border-purple-800 border-opacity-100' : '']"
        @click="changePreview(i)"
        :src="item.href"
      />
    </div>
  </div>
</template>

<style scoped>
/* vue transition */
.slide-fade-enter-active {
  transition: all 0.8s ease;
}
.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.3, 1);
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(30px);
  opacity: 0;
}

.slide-next-enter-active {
  transition: all 1s ease;
}
.slide-next-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.3, 1);
}
.slide-next-enter {
  transform: translateX(100px);
  opacity: 0;
}
.slide-next-leave-to {
  transform: translateX(-100px);
  opacity: 0;
}
.slide-back-enter-active {
  transition: all 1s ease;
}
.slide-back-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.3, 1);
}
.slide-back-enter {
  transform: translateX(-100px);
  opacity: 0;
}
.slide-back-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
</style>
