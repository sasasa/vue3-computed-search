<template>
  <h1 v-cloak>{{ message }}</h1>
  <div>検索：<input type="text" v-model="search" /></div>
  <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Website</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in search_users" :key="user.id">
        <td v-html="highLight(user.name)"></td>
        <td v-html="highLight(user.email)"></td>
        <td v-html="highLight(user.website)"></td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import axios from "axios";
import { ref, computed, onMounted } from "vue";

const message = ref("Search/Filter In Table");
const users = ref([]);
const search = ref("");

const highLight = (text) => {
  let searchWord = search.value.trim();

  if (searchWord === "") return text;

  const re = new RegExp(searchWord, "ig");
  if (!text.match(re)) return text;

  return text.replace(re, function (search) {
    return `<span style="background-color:yellow;font-weight:bold">
          ${search}
          </span>`;
  });
};

const search_users = computed(() => {
  let searchWord = search.value.trim();

  if (searchWord === "") return users.value;
  const re = new RegExp(searchWord, "ig");
  return users.value.filter((user) => {
    return (
      user.name.match(re) || user.email.match(re) || user.website.match(re)
    );
  });
});
onMounted(() => {
  axios.get("https://jsonplaceholder.typicode.com/users").then((response) => {
    console.log(response.data);
    users.value = response.data;
  });
});
</script>

<style>
table {
  border-collapse: collapse;
  width: 100%;
}
td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
th {
  color: white;
  background-color: #1E90FF;
}
input {
  width: 30%;
  padding: 0.5em 1em;
  border-radius: 2px;
  margin-bottom: 1em;
}
[v-cloak] {
  display: none;
}
</style>
