<script setup>
import { ref } from "vue";

const previewImage = ref([]);
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
  <div class="preview" v-for="(item, x) in previewImage" :key="x">
    <h1>{{ item.caption }}</h1>
    <img :src="item.href" />
  </div>

  <div class="footer">
    <span v-for="(item, i) in footerImage" :key="i">
      <img @click="changePreview(i)" :src="item.href" />
    </span>
  </div>

  <button class="button" @click="prev">-</button>

  <button class="button" @click="next">+</button>
</template>

<style>
.preview {
  width: 100%;
}
.footer {
  width: 100%;
  display: flex;
  flex-direction: flex-row;
  justify-content: center;
  align-content: center;
  gap: 10px;
  border: 1px solid;
  padding: 10px;
}

.footer > span > img {
  width: 50px;
  cursor: pointer;
}

.button {
  padding: 30px;
  margin: 20px;
}
</style>
