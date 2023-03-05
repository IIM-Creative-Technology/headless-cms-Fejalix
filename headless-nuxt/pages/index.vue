<template>
  <div v-if="projects" class=" ">
    <h1 class="title">Portfolio</h1>
    <p class="under-title">- Felix Janot -</p>

    <div class="filter-buttons-container">
      <button class="button-filter" @click="filterProjects('all')">
        reset
      </button>
      <button
        class="button-filter"
        :key="type"
        @click="filterProjects(type)"
        v-for="type in types"
      >
        {{ type }}
      </button>
    </div>
    <div class="container">
      <div
        class="link-card"
        :to="`/projects/${project.slug}`"
        v-for="(project, index) in filteredProjects"
        :key="index"
      >
        <div class="container-name-project">
          <h3 class="name-project">{{ project.name }}</h3>
        </div>

        <img :src="project.image[0].url" alt="" />
        <nuxt-link class="button-card" :to="`/projects/${project.slug}`">
          See more
          <svg
            fill="#ffffff"
            width="22"
            height="22"
            version="1.1"
            id="lni_lni-arrow-right"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            x="0px"
            y="0px"
            viewBox="0 0 64 64"
            style="enable-background: new 0 0 64 64"
            xml:space="preserve"
          >
            <path
              d="M57.6,30.4l-20.7-21c-0.9-0.9-2.3-0.9-3.2,0c-0.9,0.9-0.9,2.3,0,3.2l16.8,17.1H8c-1.2,0-2.2,1-2.2,2.2s1,2.3,2.2,2.3h42.7
	l-17,17.3c-0.9,0.9-0.9,2.3,0,3.2c0.4,0.4,1,0.6,1.6,0.6c0.6,0,1.2-0.2,1.6-0.7l20.7-21C58.5,32.7,58.5,31.3,57.6,30.4z"
            />
          </svg>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script setup>
const { find } = useStrapi();
const projects = ref();
const types = ref([]);
const activeFilter = ref("all");

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") {
    return projects.value.data;
  }
  return projects.value.data.filter(
    (project) => project.type === activeFilter.value
  );
});

onMounted(async () => {
  projects.value = await find("projects", { populate: "deep" });
  types.value = new Set(
    projects.value.data.map((project) => {
      return project.type;
    })
  );
});
</script>
<style>
.title {
  font-size: 50px;
  text-align: center;
  font-weight: 700;
  margin: 0;
  padding-top: 50px;
}
.under-title {
  font-size: 20px;
  text-align: center;
  font-weight: 400;
  margin: 0;
  color: #ff6016;
}
.container-name-project {
  position: absolute;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #0000001e;
  border-radius: 30px;
}
.name-project {
  text-transform: capitalize;
  color: white;
}
.filter-buttons-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  padding: 80px 0px;
  gap: 50px;
}

.button-filter {
  font-size: 18px;
  padding: 20px 40px;
  border: none;
  background: none;
  text-transform: capitalize;
  border-radius: 10px;
}
.button-filter:hover {
  background: #ff6016;
  transition: 0.3s;
  color: white;
}

.container {
  display: flex;
  width: 100%;
  /* background: green; */
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 100px;
}
.link-card {
  display: flex;
  position: relative;
  /* background: red; */
  width: 350px;

  border-radius: 20px;
  text-decoration: none;
}

.link-card img {
  width: 100%;
  border-radius: 30px;
}

.button-card {
  position: absolute;
  bottom: -30px;
  right: -30px;
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
}

.button-card svg {
  max-width: 30px;
}
</style>
