<template>
    <div>
        <div class="text-center container mt-3 justify-content-center align-items-center">
      <form @submit.prevent="search">
        <div class="row text-center container mt-3 justify-content-center align-items-center">
          <div class="text-center col-md-2 ">
              <div class="mb-3">
              <label for="name" class="text-center form-label">Имя</label>
              <input type="text" v-model="name" id="name" class="form-control" placeholder="Имя">
            </div>
          </div>
          <div class="col-md-2">
            <div class="mb-3">
              <label for="bedrooms" class="text-center form-label">Спален</label>
              <input type="number" v-model="bedrooms" id="bedrooms" class="form-control" placeholder="Спален">
            </div>
          </div>
        </div>
        <div class="row text-center container mt-3 justify-content-center align-items-center">
          <div class="col-md-2">
            <div class="mb-3">
              <label for="bathrooms" class="text-center form-label">Ванных комнат</label>
              <input type="number" v-model="bathrooms" id="bathrooms" class="form-control" placeholder="Ванных комнат">
            </div>
          </div>
          <div class="col-md-2">
            <div class="mb-3">
              <label for="storeys" class="text-center form-label">Этажей</label>
              <input type="number" v-model="storeys" id="storeys" class="form-control" placeholder="Этажей">
            </div>
          </div>
        </div>
        <div class="row text-center container mt-3 justify-content-center align-items-center">
          <div class="col-md-2">
            <div class="mb-3">
              <label for="garages" class="text-center form-label">Гаражей</label>
              <input type="number" v-model="garages" id="garages" class="form-control" placeholder="Гаражей">
            </div>
          </div>
          <div class="col-md-2">
            <div class="mb-3">
              <label for="price_from" class="text-center form-label">Цена от</label>
              <input type="number" v-model="price_from" id="price_from" class="form-control" placeholder="Цена от">
            </div>
          </div>
        </div>
        <div class="row text-center container mt-3 justify-content-center align-items-center">
          <div class="col-md-2">
            <div class="mb-3">
              <label for="price_to" class="text-center form-label">Цена до</label>
              <input type="number" v-model="price_to" id="price_to" class="form-control" placeholder="Цена до">
            </div>
          </div>
          <div class="col-md-2">
            <div class="mb-3">
                <label for="price_to" class="text-center form-label invisible">валдай</label>
              <button type="submit" class="btn btn-primary justify-content-center align-items-center table mt-6 my-1">Поиск</button>
            </div>
          </div>
        </div>
      </form>
    </div>

      <table v-if="houses.length" class="text-center table mt-6">
        <thead>
          <tr>
            <th>Name</th>
            <th>Bedrooms</th>
            <th>Bathrooms</th>
            <th>Storeys</th>
            <th>Garages</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="house in houses" :key="house.id">
            <td>{{ house.name }}</td>
            <td>{{ house.bedrooms }}</td>
            <td>{{ house.bathrooms }}</td>
            <td>{{ house.storeys }}</td>
            <td>{{ house.garages }}</td>
            <td>{{ house.price }}</td>
          </tr>
        </tbody>
      </table>

      <div v-if="loading" class="text-center mt-4 d-flex justify-content-center align-items-center">
  <div class="spinner-border" role="status">
    <span class="visually-hidden">Загрузка...</span>
  </div>
</div>

    <div v-else-if="!loading" class="text-center mt-4">
      <p v-if="houses.length === 0">Ничего не нашли.</p>
    </div>
    </div>
  </template>

<script>
export default {
  data() {
    return {
      name: '',
      bedrooms: null,
      bathrooms: null,
      storeys: null,
      garages: null,
      price_from: null,
      price_to: null,
      houses: [],
      loading: false,
      showTable: false,
    };
  },
  methods: {
    search() {
      this.loading = true;
      this.showTable = false;

      setTimeout(() => {
        axios
          .get('/houses/search', {
            params: {
              name: this.name,
              bedrooms: this.bedrooms,
              bathrooms: this.bathrooms,
              storeys: this.storeys,
              garages: this.garages,
              price_from: this.price_from,
              price_to: this.price_to,
            },
          })
          .then((response) => {
            this.houses = response.data;
            this.showTable = true;
            this.loading = false;
          })
          .catch((error) => {
            console.error(error);
            this.loading = false;
          });
      }, 2000);
    },
  },
};
</script>

