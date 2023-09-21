<template>
  <div class="job-list">
    <div class="search-jobs">
      <input
        type="text"
        v-model="search"
        placeholder="Search"
        class="search-input"
      />
    </div>
    <transition-group name="list" tag="ul">
      <li v-for="job in orderedJobs" :key="job.id">
        <h2>{{ job.title }} <span class="in">in</span> {{ job.location }}</h2>
        <div class="salary">
          <img src="../assets/rupee.svg" alt="rupee icon" />
          <p>{{ job.salary }} rupees</p>
        </div>
        <div class="description">
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Ex
            recusandae accusamus cupiditate ratione quos ab, velit blanditiis
            aliquid in distinctio mollitia nesciunt temporibus molestiae.
            Incidunt, rem magnam. Labore, maiores reiciendis!
          </p>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref, defineProps } from "vue";
import Job from "../types/Job";
import OrderTerm from "../types/OrderTerm";

interface Props {
  jobs: Job[],
  order: OrderTerm
}

const props = defineProps<Props>();

const search = ref("");

const orderedJobs = computed(() => {
  
  let sortedJobs: Job[] = [...props.jobs]

  if(search.value !== '') {
    sortedJobs = sortedJobs.filter(item => item.title.toLowerCase().includes(search.value.toLowerCase()))
  }
  
  return sortedJobs.sort((a: Job, b: Job) => a[props.order] > b[props.order] ? 1 : -1);
});
</script>

<style scoped>
.job-list {
  max-width: 960px;
  margin: 40px auto;
}
.in {
  color: rgb(0, 166, 255);
}
.job-list ul {
  padding: 0;
}
.job-list li {
  list-style-type: none;
  background: white;
  padding: 16px;
  margin: 16px 0;
  border-radius: 4px;
}
.job-list h2 {
  margin: 0 0 10px;
  text-transform: capitalize;
}
.salary {
  display: flex;
}
.salary img {
  width: 30px;
}
.salary p {
  color: #17bf66;
  font-weight: bold;
  margin: 10px 4px;
}
.list-move {
  transition: all 0.7s;
}

.search-jobs {
  display: flex;
  justify-content: center;
}

.search-input {
  padding: 10px;
  width: 50%;
}
</style>
