<template>
  <div v-if="project" class="container">
    <nuxt-link class="button-back" to="/"> retour </nuxt-link>
    <div class="right">
      <img :src="project.image[0].url" alt="" />
    </div>
    <div class="left">
      <h1 class="title">{{ project.name }}</h1>
      <p
        v-for="(technologie, index) in project.technologies"
        :key="index"
        class="under-title"
      >
        - {{ technologie.name }} -
      </p>
      <p class="description">
        {{ project.description }}
      </p>
    </div>
  </div>
  <!-- <div v-if="project">
    {{ project.name }}
    <img :src="project.image[0].url" alt="" width="500" />
    <nuxt-link to="/"> retour </nuxt-link>
    <h1 v-for="(technologie, index) in project.technologies" :key="index">
      {{ technologie.name }}
    </h1>

  </div> -->
</template>

<script setup>
const { findOne } = useStrapi();
const route = useRoute();
const project = ref();

onMounted(async () => {
  project.value = await findOne(
    `projects?filters[slug]=${route.params.slug}&populate=deep`
  );
  project.value = project.value.data[0];
});
</script>
<style>
html {
  margin: 0;
  padding: 0;
}

.button-back {
  position: absolute;
  top: 1px;
  left: 1px;
  border: solid 10px white;
  background: #ff3041;
  border-radius: 20px;
  padding: 10px 25px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 10px;

  text-decoration: none;
  color: white;
  font-size: 20px;
  font-weight: 500;
  text-transform: capitalize;
}
.container {
  position: relative;
  /* background: red; */
  height: 90vh;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
}
.right {
  border-radius: 30px;
  max-width: 600px;
}
.right img {
  width: 100%;
  border-radius: 30px;
}
.left {
  width: 40%;
}
.description {
  margin: 0 auto;
  text-align: center;
  max-width: 50%;
  margin-top: 20px;
}
</style>
