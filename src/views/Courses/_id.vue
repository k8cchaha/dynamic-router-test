<script>
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { onMounted, reactive, ref } from "@vue/runtime-core";
export default {
  setup() {
    const route = useRoute();
    const router = useRouter();
    const course = reactive({ data: {} });
    const isError = ref(false);
    onMounted(() => {
      axios
        .get(`https://vue-lessons-api.herokuapp.com/courses/${route.params.id}`)
        .then((res) => {
          course.data = res.data.data[0];
        })
        .catch((err) => {
          isError.value = true;
          course.data["error_message"] = err.response.data.error_message;
          setTimeout(() => {
            router.push({ path: "/course" });
          }, 3000);
        });
    });
    return { course, isError };
  },
};
</script>
<template>
  <div>
    <div v-if="!isError">
      <h1>{{ course.data.name }}</h1>
      <h2>NTD: {{ course.data.money }}</h2>
      <img :src="course.data.photo" alt="" />
      <div v-if="Object.keys(course.data).length > 0">
        <img :src="course.data.teacher.photo" alt="" />
        <p>{{ course.data.teacher.name }}</p>
      </div>
    </div>
    <!-- error_message -->
    <h1 v-if="isError">{{ course.data.error_message }}</h1>
  </div>
</template>

<style></style>
