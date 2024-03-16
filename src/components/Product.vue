<template>
  <div class="bg-gray-100">
    <div
      class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
    >
      <div class="lg:grid lg:grid-cols-2 lg:items-start lg:gap-x-8 relative">
        <!-- Image gallery -->
        <TabGroup as="div" class="w-full flex flex-col-reverse">
          <!-- Image selector -->
          <div class="mx-auto w-full max-w-2xl block lg:max-w-none">
            <products/>
            <div @click="setModal" class="mt-4 cursor-pointer text-center">
              Click Here To Open Expanded View
            </div>
          </div>
        </TabGroup>

        <!-- Product info -->
        <div class="mt-10 px-4 sm:mt-16 sm:px-0 lg:mt-0">
          <h1 class="text-3xl font-bold tracking-tight text-gray-900">
            {{ product.name }}
          </h1>

          <div class="mt-3">
            <h2 class="sr-only">Product information</h2>
            <p class="text-3xl tracking-tight text-gray-900">
              {{ product.price }}
            </p>
          </div>

          <!-- Reviews -->
          <div class="mt-3">
            <h3 class="sr-only">Reviews</h3>
            <div class="flex items-center">
              <div class="flex items-center">
                <StarIcon
                  v-for="rating in [0, 1, 2, 3, 4]"
                  :key="rating"
                  :class="[
                    product.rating > rating
                      ? 'text-indigo-500'
                      : 'text-gray-300',
                    'h-5 w-5 flex-shrink-0',
                  ]"
                  aria-hidden="true"
                />
              </div>
              <p class="sr-only">{{ product.rating }} out of 5 stars</p>
            </div>
          </div>

          <div class="mt-6">
            <h3 class="sr-only">Description</h3>

            <div
              class="space-y-6 text-base text-gray-700"
              v-html="product.description"
            />
          </div>

          <form class="mt-6">
            <!-- Colors -->

            <div class="mt-10 flex">
              <div
                @click="setModal"
                class="flex max-w-full flex-1 items-center justify-center rounded-md border border-transparent bg-yellow-400 px-8 py-3 text-base font-medium text-white hover:bg-yellow-300 focus:outline-none focus:ring-2 focus:ring-yellow-500 focus:ring-offset-2 focus:ring-offset-gray-50 sm:w-full"
              >
                Add to cart
              </div>
            </div>
          </form>

          <section aria-labelledby="details-heading" class="mt-20">
            <h2 id="details-heading" style="font-weight: bolder;">About this item</h2>
            <ul class="list-disc ml-4">
              <li>6.7-inch Super Retina XDR display</li>
              <li>All-day battery life and up to 26 hours of video playback</li>
              <li>Industry-leading durability features with Ceramic Shield and water resistance</li>
              <li>A15 Bionic chip with 5-core GPU for lightning-fast performance. Superfast 5G cellular</li>
              <li>Advanced camera system for better photos in any light</li>
            </ul>
          </section>
        </div>
      </div>
    </div>
    <!--  -->
    <!--  -->
    <!--  -->
    <!-- MODAL -->
    <Modal :modal="modal" :close-modal="closeModal">
      <template v-slot:images>
        <ModalProduct />
      </template>
      <template v-slot:videos>
        <videoModal />
      </template>
    </Modal>
   
  </div>
</template>

<script setup>
import { ref } from "vue";
import { Tab, TabGroup, TabList } from "@headlessui/vue";
import { StarIcon } from "@heroicons/vue/20/solid";
import { HeartIcon, MinusIcon, PlusIcon } from "@heroicons/vue/24/outline";
import products from "@/components/products.vue";
import videoModal from "@/components/videoModal.vue";
import ModalProduct from "@/components/ModalProduct.vue";
import Modal from "@/components/Modal.vue";
import video from "@/assets/video.mp4";

const modal = ref(false);

const product = {
  name: "Samsung Galaxy S23+ 5G Dual SIM Android Mobile Phone, 512GB, SIM Free Smartphone, Black, 3Y Extended Manufacturer Warranty (UK Version)",
  price: "£650",
  rating: 4,
  images: [
    {
      id: 1,
      name: "Angled view",
      src: "https://tailwindui.com/img/ecommerce-images/product-page-03-product-01.jpg",
      alt: "Samsung Galaxy S23 Plus 5G.",
    },
    
  ],
  colors: [
    {
      name: "Washed Black",
      bgColor: "bg-gray-700",
      selectedColor: "ring-gray-700",
    },
    { name: "White", bgColor: "bg-white", selectedColor: "ring-gray-400" },
    {
      name: "Washed Gray",
      bgColor: "bg-gray-500",
      selectedColor: "ring-gray-500",
    },
  ],
  description: `
    <p>The Samsung Galaxy S23+ is a flagship smartphone that belongs to the Galaxy S23 series, offering users a premium experience with cutting-edge technology and a sleek design.</p>
  `,
  details: [
    {
      name: "Features",
      items: [
        "Multiple strap configurations",
        "Spacious interior with top zip",
        "Leather handle and tabs",
        "Interior dividers",
        "Stainless strap loops",
        "Double stitched construction",
        "Water-resistant",
      ],
    },
    // More sections...
  ],
};

const setModal = (e) => {
  e.preventDefault();
  modal.value = !modal.value;
};

const closeModal = (val) => {
  console.log("cool", val);

  modal.value = false;
};
const selectedColor = ref(product.colors[0]);
</script>
