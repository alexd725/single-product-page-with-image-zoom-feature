<template>
  <div class="bg-white p4">
    <swiper
      :loop="true"
      :spaceBetween="10"
      :navigation="false"
      :thumbs="{ swiper: thumbsSwiper }"
      :modules="modules"
      class="mySwiper2 flex flex-col text-center h-100" 
    >
      <swiper-slide
        v-for="(item, index) in slides"
        :key="index"
        class="relative"
      >
        <!-- Conditionally render either InnerImageZoom or video element -->
        <template v-if="item.dataType === 'img'">
          <div class="container" @mousemove="zoomImage">
            <img
              class="image rounded-md w-[28rem] h-[37rem]"
              :src="item.src"
              :fullscreen="true"
            />
            <div class="lens w-[12rem] h-[12rem] absolute top-0 left-0 border-2 border-solid border-red-500 bg-white bg-opacity-60" ></div>
            <div class="result w-[30rem] h-[30rem] absolute top-0 left-40 border-2 border-solid border-blue-500" ></div>
          </div>
        </template>
        <template v-else-if="item.dataType === 'video'">
          <video width="100%" height="100%" controls>
            <source :src="item.src" type="video/mp4" />
            <source :src="item.src" type="video/ogg" />
            Your browser does not support the video tag.
          </video>
        </template>
        <div class="absolute top-0 left-0 z-50 bg-red-400 h-44 w-400"></div>
      </swiper-slide>
    </swiper>
    <!-- Thumbs Swiper -->
    <swiper
      @swiper="setThumbsSwiper"
      :loop="true"
      :spaceBetween="10"
      :navigation="true"
      :slidesPerView="4"
      :freeMode="true"
      :watchSlidesProgress="true"
      :modules="modules"
      class="mySwiper mt-4"
    >
      <swiper-slide v-for="(item, index) in slides" :key="index">
        <template v-if="item.dataType === 'img'">
          <img class="rounded-md w-full object-contain h-24" :src="item.src" />
        </template>
        <template v-else-if="item.dataType === 'video'">
          <img
            class="rounded-md h-24 object-contain w-full"
            src="../assets/videoThumbnail.png"
          />
        </template>
      </swiper-slide>
    </swiper>
  </div>
  
</template>

<script>
import { ref } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/free-mode";
import "swiper/css/navigation";
import "swiper/css/thumbs";
import video from "@/assets/video.mp4";
import image1 from "@/assets/assets/phone3.jpeg";
import image2 from "@/assets/assets/phone2.jpeg";
import image3 from "@/assets/assets/phone.jpg";

// import required modules
import { FreeMode, Navigation, Thumbs } from "swiper/modules";

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      container: null,
      image: null,
      lens:null,
      result:null,
      containerRect:null,
      imageRect:null,
      lensRect:null,
      resultRect:null,

    }
  },
  mounted(){
    this.container = this.$el.querySelector('.container')
    this.image = this.$el.querySelector('.image')
    this.lens = this.$el.querySelector('.lens')
    this.result = this.$el.querySelector('.result')

    this.containerRect = this.container.getBoundingClientRect();
    this.imageRect = this.image.getBoundingClientRect();
    this.lensRect = this.lens.getBoundingClientRect();
    this.resultRect = this.result.getBoundingClientRect();

  },
  methods: {
  zoomImage(e) {
    console.log("zoom image", e.clientX, e.clientY);
    const { x, y } = this.getMousePos(e); 

    this.lens.style.left = x + "px";
    this.lens.style.top = y + "px";

    let fx = this.resultRect.width / this.lensRect.width
    let fy = this.resultRect.height / this.lensRect.height

    this.result.style.backgroundImage = `url(${this.image.src})`
    this.result.style.backgroundSize = `${this.imageRect.width * fx}px ${this.imageRect.height * fy}px`
    this.result.style.backgroundPosition = `-${x * fx}px -${y * fy}px`
  },

  getMousePos(e) {
    let x = (e.clientX - this.containerRect.left) - this.lensRect.width / 2;
    let y = (e.clientY - this.containerRect.top) - this.lensRect.height / 2;

    let minX = 0;
    let minY = 0;
    let maxX = this.containerRect.width - this.lensRect.width;
    let maxY = this.containerRect.height - this.lensRect.height;

    if (x <= minX) {
      x = minX;
    } else if (x >= maxX) {
      x = maxX;
    }

    if (y <= minY) {
      y = minY;
    } else if (y >= maxY) {
      y = maxY;
    }

    return { x, y };
  }
},
  setup() {
    const thumbsSwiper = ref(null);

    const setThumbsSwiper = (swiper) => {
      thumbsSwiper.value = swiper;
    };

    // Your array of slides
    const slides = [
      {
        dataType: "img",
        src: image2,
        alt: "Description for image 2"
      },

      {
        dataType: "img",
        src: image1,
        alt: "Description for image 1"
      },
      {
        dataType: "img",
        src: image3,
        alt: "Description for image 3"
    
      },
      
      { dataType: "video", 
      src: video },
      {
        dataType: "img",
        src: image3,
        
      },
      
    ];

    return {
      thumbsSwiper,
      setThumbsSwiper,
      modules: [FreeMode, Navigation, Thumbs],
      slides,
    };
  },
};
</script>
