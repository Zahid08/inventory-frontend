<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h6 style="text-align: left">Signup</h6>
        <router-link
            to="/"
            class="nav-link"
            style="text-align: right"
        >
          <i class="fa fa-plus-circle" /> Login
        </router-link>
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
              <label>Name:</label>
              <Field
                  id="name"
                  v-model="users.name"
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
              <label>Email:</label>
              <Field
                  id="name"
                  v-model="users.email"
                  name="email"
                  type="text"
                  class="form-control"
              />
              <ErrorMessage
                  name="email"
                  class="text-capitalize text-danger"
              />
            </div>
            <div class="col-6">
              <label>Password:</label>
              <Field
                  v-model="users.password"
                  name="password"
                  type="password"
                  class="form-control"
              />
              <ErrorMessage
                  name="password"
                  class="text-capitalize text-danger"
              />
            </div>
            <div class="col-6">
              <label>Confirm Password:</label>
              <Field
                  v-model="users.password_confirmation"
                  name="password_confirmation"
                  type="password"
                  class="form-control"
              />
              <ErrorMessage
                  name="password_confirmation"
                  class="text-capitalize text-danger"
              />
            </div>
          </div>

          <div class="form-group">
            <input
                v-if="!isCreating"
                type="submit"
                class="btn btn-primary mx-2 my-2"
                value="Signup"
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
      name: yup.string().required(),
      email: yup.string().required(),
      password: yup.string().required(),
      password_confirmation: yup.string().required(),
    });

    return {
      schema,
    };
  },

  data() {
    return {
      users: {},
    };
  },

  computed: { ...mapGetters(["isCreating", "createdData"]) },

  watch: {
    createdData: function () {
      if (this.createdData !== null && !this.isCreating) {
        this.$swal.fire({
          text: "Success, Login",
          icon: "success",
          position: "top-end",
          timer: 1000,
        });
        this.$router.push({ name: "Products" });
      }
    },
  },
  created: function () {

  },
  methods: {
    ...mapActions(["registerUser"]),
    onSubmit() {
      const { email, password, password_confirmation,name} = this.users;
      this.registerUser({
        name: name,
        email: email,
        password: password,
        password_confirmation: password_confirmation,
      });
    },
  },
};
</script>
