<template>
    <div>
      <Slider :slides="slides" :loop="true" />
    </div>
  </template>
  
  <script>
  import Slider from '~/components/Slider.vue';
  
  export default {
    components: {
      Slider,
    },
    data() {
      return {
        slides: [
          {
            title: 'Slider Title 01',
            media: {
              type: 'image',
              src: '1.webp',
              w: '1080',
              h: '1920',
            },
          },
          {
            title: 'Slider Title 02',
            media: {
              type: 'image',
              src: '2.jpg',
              w: '1080',
              h: '1920',
            },
          },
          {
            title: 'Slider Title 03',
            media: {
              type: 'video',
              src: '3.webp',
              w: '1920',
              h: '1080',
            },
          },
        ],
      };
    },
  };
  </script>
  
  <style>
  /* Estilos globales */
  </style>