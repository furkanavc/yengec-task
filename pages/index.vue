<script setup lang="ts">
import { useAuthStore } from "../store/auth";
import { useCatStore } from "../store/cat";
import { useRouter } from "vue-router";
import { storeToRefs } from "pinia";
import { onMounted } from "vue";
import loadingImage from "@/assets/images/loading.png";

const router = useRouter();
const { logout } = useAuthStore();
const catStore = useCatStore();
const { data, loading } = storeToRefs(catStore);
const handleLogout = () => {
  logout();
  router.push("/login").then(() => {
    alert("Logged Out");
  });
};
onMounted(() => {
  catStore.getData();
});
definePageMeta({
  middleware: "auth",
});
</script>
<template>
  <div
    class="container mx-auto flex items-center justify-center px-5 lg:px-0 pt-12"
  >
    <UCard>
      <template #header>
        <h1 class="items-center font-black">CATS</h1>
      </template>
      <div class="max-[384px]:w-full w-96 h-96 overflow-hidden">
        <Transition name="slide-fade">
          <img
            :src="loading ? loadingImage : data?.url"
            class="object-cover w-full h-full"
            :key="data?.id"
          />
        </Transition>
      </div>
      <template #footer>
        <div class="flex w-full justify-between">
          <UButton @click="catStore.getData()">Change</UButton>
          <UButton @click="handleLogout">Logout</UButton>
        </div>
      </template>
    </UCard>
  </div>
</template>
<style>
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
