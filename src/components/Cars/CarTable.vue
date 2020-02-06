<template>
  <div class="container">
    <div class="mt-5">
      <router-link :to="{ name: 'car.add' }">Add</router-link>
    </div>
    <div class="card mt-5">
      <div class="card-header">Manage Cars</div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">Name</th>
              <th scope="col">Color</th>
            </tr>
          </thead>
          <draggable v-model="items" tag="tbody">
            <tr v-for="item in items" :key="item.id">
              <td>
                <router-link
                  :to="{ name: 'car.edit', params: { id: item.id } }"
                >{{ item.name }}</router-link>
              </td>
              <td>{{ item.color }}</td>
            </tr>
          </draggable>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import { apiService } from "@/common/api.service.js";

import draggable from "vuedraggable";

export default {
  name: "car-table",
  components: {
    draggable
  },
  data() {
    return {
      items: [],
      currentItems: {},
      loading: false,
      dragging: false
    };
  },
  methods: {
    getCars() {
      this.loading = true;
      let endpoint = `/api/v1/cars/`;
      apiService(endpoint)
        .then(data => {
          this.items.push(...data);
          this.loading = false;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    this.getCars();
  }
};
</script>