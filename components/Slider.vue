<template>
    <div class="slider-container">
      <div class="slider-background" :style="{ backgroundImage: `url(${currentSlide.media.src})` }"></div>
      <div class="slider-wrapper">
        <div 
          v-for="(slide, index) in slides" 
          :key="index"
          :class="['slide', {
            'previous': isPrevious(index),
            'current': isCurrent(index),
            'next': isNext(index),
            'entering': isEntering && isCurrent(index),
            'leaving': isLeaving && isPrevious(index)
          }]"
          @click="handleSlideClick(index)"
        >
          <div class="slide-inner">
            <img :src="slide.media.src" :alt="slide.title">
            <div class="slide-title" v-if="isCurrent(index)">{{ slide.title }}</div>
          </div>
        </div>
      </div>
      <div class="slider-controls">
        <button class="control-button prev" @click="previousSlide">&lt;</button>
        <button class="control-button next" @click="nextSlide">&gt;</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      slides: {
        type: Array,
        required: true
      },
      loop: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {
        currentIndex: 0,
        isAnimating: false,
        isEntering: false,
        isLeaving: false
      }
    },
    computed: {
      currentSlide() {
        return this.slides[this.currentIndex]
      }
    },
    methods: {
      isPrevious(index) {
        return index === (this.currentIndex - 1 + this.slides.length) % this.slides.length;
      },
      isCurrent(index) {
        return index === this.currentIndex;
      },
      isNext(index) {
        return index === (this.currentIndex + 1) % this.slides.length;
      },
      handleSlideClick(index) {
        if (this.isAnimating) return;
        if (index < this.currentIndex) this.previousSlide();
        if (index > this.currentIndex) this.nextSlide();
      },
      nextSlide() {
        if (this.isAnimating) return;
        this.isAnimating = true;
        this.isEntering = true;
        this.currentIndex = (this.currentIndex + 1) % this.slides.length;
        setTimeout(() => {
          this.isAnimating = false;
          this.isEntering = false;
        }, 1500); // Duración de la animación: 1.5 segundos
      },
      previousSlide() {
        if (this.isAnimating) return;
        this.isAnimating = true;
        this.isLeaving = true;
        this.currentIndex = (this.currentIndex - 1 + this.slides.length) % this.slides.length;
        setTimeout(() => {
          this.isAnimating = false;
          this.isLeaving = false;
        }, 1500); // Duración de la animación: 1.5 segundos
      }
    }
  }
  </script>
  
  <style scoped>
  .slider-container {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-color: #000;
  }
  
  .slider-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    filter: blur(15px);
    opacity: 0.2;
    transition: background-image 1.5s cubic-bezier(0.25, 0.1, 0.25, 1);
  }
  
  .slider-wrapper {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    gap: 2rem;
    padding: 0 15%;
    perspective: 2000px;
  }
  
  .slide {
    position: relative;
    min-width: 300px;
    height: 300px;
    cursor: pointer;
    transition: all 1.5s cubic-bezier(0.25, 0.1, 0.25, 1);
    opacity: 0.7;
    pointer-events: none;
    transform-origin: center center;
    will-change: transform;
  }
  
  .slide-inner {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    border-radius: 10px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
  }
  
  .slide img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 1.5s cubic-bezier(0.25, 0.1, 0.25, 1);
  }
  
  .slide-title {
    position: absolute;
    top: 50%;
    left: 0;
    width: 100%;
    text-align: center;
    color: white;
    font-size: 1.5rem;
    font-weight: 300;
    text-transform: uppercase;
    letter-spacing: 2px;
    opacity: 0;
    transform: translateY(-50%);
    transition: opacity 0.4s ease;
    background: rgba(0, 0, 0, 0.5);
    padding: 1rem;
  }
  
  .slide.previous,
  .slide.next {
    opacity: 0.7;
    pointer-events: auto;
    height: 300px;
  }
  
  .slide.previous {
    transform: scale(0.7) translateX(-100px);
  }
  
  .slide.next {
    transform: scale(0.7) translateX(100px);
  }
  
  .slide.current {
    transform: scale(1) translateX(0);
    opacity: 1;
    z-index: 2;
    pointer-events: auto;
    height: 500px;
  }
  
  .slide.entering {
    animation: enterSlide 1.5s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
  }
  
  .slide.leaving {
    animation: leaveSlide 1.5s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
  }
  
  @keyframes enterSlide {
    0% {
      transform: scale(0.7) translateX(100px) rotateZ(-90deg);
      opacity: 0;
    }
    100% {
      transform: scale(1) translateX(0) rotateZ(0deg);
      opacity: 1;
    }
  }
  
  @keyframes leaveSlide {
    0% {
      transform: scale(1) translateX(0) rotateZ(0deg);
      opacity: 1;
    }
    100% {
      transform: scale(0.7) translateX(-100px) rotateZ(-90deg);
      opacity: 0;
    }
  }
  
  .slide.current .slide-title {
    opacity: 1;
  }
  
  .slide:hover img {
    transform: scale(1.1);
  }
  
  .slider-controls {
    position: absolute;
    bottom: 2rem;
    left: 0;
    width: 100%;
    display: flex;
    justify-content: center;
    gap: 2rem;
    z-index: 10;
  }
  
  .control-button {
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    color: white;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.2rem;
    transition: all 0.3s ease;
  }
  
  .control-button:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: scale(1.1);
  }
  
  @media (max-width: 768px) {
    .slider-wrapper {
      padding: 0 5%;
    }
  
    .slide {
      min-width: 250px;
      height: 250px;
    }
  
    .slide.current {
      height: 400px;
    }
  
    .slide.previous,
    .slide.next {
      height: 250px;
    }
  
    .slide-title {
      font-size: 1.2rem;
      padding: 0.75rem;
    }
  }
  </style>