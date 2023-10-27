<template>
  <div class="container">
    <div class="row">
      <div class="col-md-2">
        <div class="form-group">
          <label for="ProductId">Product Id</label>
          <input
            type="text"
            class="form-control"
            id="ProductId"
            required
            v-model="product.ProductId"
            name="ProductId"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="ProductName">Product Name</label>
          <input
            type="text"
            class="form-control"
            id="ProductName"
            required
            v-model="product.ProductName"
            name="ProductName"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="title">Supplier</label>
          <select
            class="form-control"
            v-model="product.SupplierID"
            required
            id="SupplierID"
            name="SupplierID"
          >
            <option value="-1" selected>Select Supplier</option>
            <option
              v-bind:value="supplier.SupplierID"
              v-for="(supplier, index) in suppliers"
              :key="index"
            >
              {{ supplier.CompanyName }}
            </option>
          </select>
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="title">Category</label>
          <select
            class="form-control"
            v-model="product.CategoryID"
            id="CategoryID"
            name="CategoryID"
          >
            <option value="-1" selected>Select Category</option>
            <option
              v-bind:value="category.CategoryID"
              v-for="(category, index) in categories"
              :key="index"
            >
              {{ category.CategoryName }}
            </option>
          </select>
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="QuantityPerUnit">QuantityPerUnit</label>
          <input
            type="text"
            class="form-control"
            id="QuantityPerUnit"
            required
            v-model="product.QuantityPerUnit"
            name="QuantityPerUnit"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="UnitPrice">UnitPrice</label>
          <input
            type="text"
            class="form-control"
            id="UnitPrice"
            required
            v-model="product.UnitPrice"
            name="UnitPrice"
          />
        </div>
      </div>
    </div>
    <br />
    <div class="row">
      <div class="col-md-2">
        <div class="form-group">
          <label for="UnitsOnOrder">UnitsOnOrder</label>
          <input
            type="text"
            class="form-control"
            id="UnitsOnOrder"
            required
            v-model="product.UnitsOnOrder"
            name="UnitsOnOrder"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="UnitsInStock">UnitsInStock</label>
          <input
            type="text"
            class="form-control"
            id="UnitsInStock"
            required
            v-model="product.UnitsInStock"
            name="UnitsInStock"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="ReorderLevel">ReorderLevel</label>
          <input
            type="text"
            class="form-control"
            id="ReorderLevel"
            required
            v-model="product.ReorderLevel"
            name="ReorderLevel"
          />
        </div>
      </div>
      <div class="col-md-2">
        <div class="form-group">
          <label for="UnitPrice">Discontinued</label>
          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              value=""
              id="flexCheckDefault"
            />
            <label class="form-check-label" for="flexCheckDefault">
              Default checkbox
            </label>
          </div>
        </div>
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary">Search</button>
      </div>
    </div>
    <br />
    <div class="row">
      <h4>Products List</h4>
      <table class="table table-bordered">
        <tr
          :class="{ active: index == currentIndex }"
          v-for="(product, index) in products"
          :key="index"
          @click="setActiveproduct(product, index)"
        >
          <td>{{ product.ProductID }}</td>
          <td>{{ product.ProductName }}</td>
          <td>{{ product.SupplierID }}</td>
          <td>{{ product.CategoryID }}</td>
          <td>{{ product.QuantityPerUnit }}</td>
          <td>{{ product.UnitPrice }}</td>
          <td>{{ product.UnitsInStock }}</td>
          <td>{{ product.UnitsOnOrder }}</td>
          <td>{{ product.ReorderLevel }}</td>
          <td>{{ product.Discontinued }}</td>
          <td>
            <router-link
              :to="'/products/' + product.ProductID"
              class="badge badge-warning"
              >Edit</router-link
            >
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";
export default {
  name: "products-list",
  data() {
    return {
      suppliers: [],
      categories: [],
      products: [],
      product: {
        ProductID: null,
        ProductName: "",
        SupplierID: 0,
        CategoryID: 0,
        QuantityPerUnit: 0,
        UnitPrice: 0,
        UnitsInStock: 0,
        UnitsOnOrder: 0,
        ReorderLevel: 0,
        Discontinued: false,
      },
      currentproduct: null,
      currentIndex: -1,
      title: "",
    };
  },
  methods: {
    getSuppliers() {
      ProductDataService.getSuppliers()
        .then((response) => {
          this.suppliers = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    getCategories() {
      ProductDataService.getCategories()
        .then((response) => {
          this.categories = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    retrieveproducts() {
      ProductDataService.getAll()
        .then((response) => {
          this.products = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveproducts();
      this.currentproduct = null;
      this.currentIndex = -1;
    },

    setActiveproduct(product, index) {
      this.currentproduct = product;
      this.currentIndex = index;
    },

    removeAllproducts() {
      ProductDataService.deleteAll()
        .then((response) => {
          console.log(response.data);
          this.refreshList();
        })
        .catch((e) => {
          console.log(e);
        });
    },

    searchTitle() {
      ProductDataService.findByTitle(this.title)
        .then((response) => {
          this.products = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    this.retrieveproducts();
    this.getSuppliers();
    this.getCategories();
  },
};
</script>


