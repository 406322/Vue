
<template>
  <div>
      <table v-if="data">
          <thead>
              <tr>
                  <th>Name</th>
                  <th>Language</th>
                  <th>Rating</th>
                  <th>Network</th>

              </tr>
          </thead>
          <tbody>
              <tr v-for="item in data">
                  <td>{{ item.name }}</td>
                  <td>{{ item.language }}</td>
                  <td>{{ item.rating }}</td>
                  <td>{{ item.network }}</td>
              </tr>
          </tbody>
      </table>
  </div>
</template>


<script setup>
import axios from "axios";
import { ref } from 'vue'

const data = ref(null)

axios.get('https://localhost:7299/')
  .then(response => myFunction(response.data))
  .then(fixedData => { return fixedData })

const getRows = (array) => {
  const arr = []
  for (const item of array) {
    const obj = {}
    obj.name = item.name
    obj.language = item.language
    obj.rating = item.rating.average
    obj.network = item.network?.name
    arr.push(obj)
  }
  data.value = arr
  return arr
}

const myFunction = (data) => {
  const array = [];
  let result;
  for (let item of data) {
    let obj = JSON.parse(item)
    array.push(obj)
    const result = getRows(array)
  }
  return array
}

</script>

<style scoped>

</style>
