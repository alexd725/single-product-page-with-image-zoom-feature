<template>
  <div class="bg-slate-100 p-1 ">
    <swiper
      :loop="true"
      :spaceBetween="10"
      :navigation="false"
      :thumbs="{ swiper: thumbsSwiper }"
      :modules="modules"
      class="mySwiper2 flex flex-col text-center h-90" 
    >
      <swiper-slide
        v-for="(item, index) in slides"
        :key="index"
        class="relative"
      >
        <!-- Conditionally render either InnerImageZoom or video element -->
        <template v-if="item.dataType === 'img'">
          <InnerImageZoom
            class="rounded-md w-[28rem] h-[36rem]"
            :src="item.src"
            :zoomSrc="item.src"
            :fullscreen="true"
          />
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
      class="mySwiper  bg-white"
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
import { VueImageZoomer } from "vue-image-zoomer";
import "vue-inner-image-zoom/lib/vue-inner-image-zoom.css";
import InnerImageZoom from "vue-inner-image-zoom";
import video from "@/assets/video.mp4";
import image1 from "@/assets/assets/phone3.jpeg";
import image2 from "@/assets/assets/phone2.jpeg";
import image3 from "@/assets/assets/phone.jpg";
import image4 from "@/assets/assets/phone4.jpeg";
import image5 from "@/assets/assets/phone5.jpg";
import image6 from "@/assets/assets/phone6.jpg";

// import required modules
import { FreeMode, Navigation, Thumbs } from "swiper/modules";

export default {
  components: {
    Swiper,
    SwiperSlide,
    InnerImageZoom,
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
        src: image1,
        
      },
      {
        dataType: "img",
        src: image2,
      },

      {
        dataType: "img",
        src: image3,
      },
      // {
      //   dataType: "img",
      //   src: image4,
    
      // },
     
      {
        dataType: "img",
        src: image5,
        
      },
      {
        dataType: "img",
        src: image6,
        
      },
      
    ];

    return {
      thumbsSwiper,
      setThumbsSwiper,
      InnerImageZoom,
      VueImageZoomer,
      modules: [FreeMode, Navigation, Thumbs],
      slides,
    };
  },
};
</script>
