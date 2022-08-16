<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h6>Add Product</h6>
      </div>
      <div class="card-body">
        <!-- <form v-on:submit.prevent="onSaveProduct" :validation-schema="schema"> -->
        <VeeForm
          :validation-schema="schema"
          @submit="onSubmit"
        >
          <!-- <Form @submit="onSaveProduct" :validation-schema="schema"> -->
          <div class="form-group row my-1">
            <div class="col-6">
              <label>Product Name:</label>
              <Field
                id="name"
                v-model="product.name"
                name="name"
                type="text"
                class="form-control"
              />
              <ErrorMessage
                name="name"
                class="text-capitalize text-danger"
              />
            </div>
            <div class="col-6">
              <label>Product Price:</label>
              <Field
                v-model="product.price"
                name="price"
                type="number"
                class="form-control"
              />
              <ErrorMessage
                name="price"
                class="text-capitalize text-danger"
              />
            </div>
          </div>

          <div class="form-group row my-1">
          <div class="col-6">
            <label>Quantity:</label>
            <Field
                name="quantity"
                type="number"
                class="form-control"
                v-model="product.quantity"
            />
            <ErrorMessage
                name="quantity"
                class="text-capitalize text-danger"
            />
          </div>

          <div class="col-6">
            <label>Category:</label>
            <select  v-model="product.category_id"   class="form-control" name="category_id">
              <option :value="item.id" v-for="(item) in category.data" :key="item.id">{{item.name}}</option>
            </select>

            <ErrorMessage
                name="category_id"
                class="text-capitalize text-danger"
            />
          </div>
          </div>


          <div class="form-group row my-1">
            <div class="col-12">
              <label>Product Details:</label>
              <Field
                v-model="product.detail"
                name="detail"
                as="textarea"
                class="form-control"
              />
              <ErrorMessage
                name="detail"
                class="text-capitalize text-danger"
              />
            </div>
          </div>

          <div class="form-group">
            <router-link
              to="/products"
              class="btn btn-secondary mr-2"
            >
              Cancel
            </router-link>
            <input
              v-if="!isCreating"
              type="submit"
              class="btn btn-primary mx-2 my-2"
              value="Add Product"
            >
            <button
              v-if="isCreating"
              class="btn btn-primary mx-2 my-2"
              type="button"
              disabled
            >
              <span
                class="spinner-border spinner-border-sm"
                role="status"
                aria-hidden="true"
              />
              Saving...
            </button>
          </div>
        </VeeForm>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import { Field, Form as VeeForm, ErrorMessage } from "vee-validate";
import * as yup from "yup";

export default {
  components: {
    Field,
    VeeForm,
    ErrorMessage,
  },

  setup() {
    // Define a validation schema
    const schema = yup.object({
      quantity: yup.string().required(),
      price: yup.string().required(),
      detail: yup.string().required().min(5),
    });

    return {
      schema,
    };
  },

  data() {
    return {
      product: {},
    };
  },

  computed: { ...mapGetters(["isCreating", "createdData","category"]) },

  watch: {
    createdData: function () {
      if (this.createdData !== null && !this.isCreating) {
        this.$swal.fire({
          text: "Success, Product has been added.",
          icon: "success",
          position: "top-end",
          timer: 1000,
        });

        this.$router.push({ name: "Products" });
      }
    },
  },
  created: function () {
    this.fetchCategoryInfo();
  },
  methods: {
    ...mapActions(["storeProduct","fetchCategoryInfo"]),
    onSubmit() {
      const { name, price, detail,quantity,category_id} = this.product;
      this.storeProduct({
        name: name,
        price: price,
        detail: detail,
        quantity:quantity,
        category_id:category_id,
      });
    },
  },
};
</script>
