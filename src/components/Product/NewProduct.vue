<template>
  <div>
    <v-container>
        <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
                <h1 class="text --secondary mb-3">Create New Product</h1>
                <v-form
                    ref="form"
                    v-model="valid"
                    lazy-validation
                    class="mb-3">
                    <v-text-field
                      label="title"
                      name="title"
                      type="text"
                      :rules="[v => !!v || 'title is required']"
                      v-model="title"
                      required
                    ></v-text-field>
                    <v-text-field
                      label="vendor"
                      name="vendor"
                      type="text"
                      v-model="vendor"
                    ></v-text-field>
                    <v-text-field
                      label="color product"
                      name="color"
                      type="text"
                      v-model="color"
                    ></v-text-field>
                    <v-text-field
                      label="material product"
                      name="material"
                      type="text"
                      v-model="material"
                    ></v-text-field>
                    <v-text-field
                      label="price product"
                      name="price"
                      type="text"
                      :rules="[v => !!v || 'title is required']"
                      v-model="price"
                      required
                    ></v-text-field>
                    <v-textarea
                      label="description product"
                      name="description"
                      type="text"
                      v-model="description"
                    ></v-textarea>
                  </v-form>
                  <v-layout class="mb-3">
                    <v-flex xs12>
                      <v-btn class="warning" @click="upload">
                        Upload
                        <v-icon right dark>cloud_upload</v-icon>
                      </v-btn>
                      <input
                        ref="fileInput"
                        type="file"
                        style="display: none;"
                        accept="image/*"
                        @change="onFileChange"
                      >
                    </v-flex>
                  </v-layout>
                  <v-layout>
                    <v-flex>
                      <img :src="imageSrc" alt="huy" height="200px" v-if="imageSrc">
                    </v-flex>
                  </v-layout>
                  <v-layout>
                    <v-flex>
                      <v-switch
                        color="primary"
                        v-model="promo"
                        :label="'add to promo?'"
                      ></v-switch>
                    </v-flex>
                  </v-layout>
                  <v-layout>
                    <v-flex xs12>
                      <v-spacer></v-spacer>
                      <v-btn
                      :loading="loading"
                        :disabled="!valid || loading || !image"
                        class="success"
                        @click="createProduct"
                        >create product
                      </v-btn>
                    </v-flex>
                  </v-layout>
            </v-flex>
        </v-layout>
    </v-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: '',
      vendor: '',
      color: '',
      material: '',
      price: 0,
      description: '',
      promo: false,
      valid: false,
      image: null,
      imageSrc: ''
    }
  },
  computed: {
    loading () {
      return this.$store.getters.loading
    }
  },
  methods: {
    createProduct () {
      if (this.$refs.form.validate() && this.image) {
        const product = {
          title: this.title,
          vendor: this.vendor,
          color: this.color,
          material: this.material,
          price: this.price,
          description: this.description,
          promo: this.promo,
          image: this.image
        }
        this.$store.dispatch('createProduct', product)
          .then(() => {
            this.$router.push('/list')
          })
          .catch(() => {})
      }
    },
    upload () {
      this.$refs.fileInput.click()
    },
    onFileChange (event) {
      const file = event.target.files[0]
      const reader = new FileReader()
      reader.onload = e => {
        this.imageSrc = reader.result
      }
      reader.readAsDataURL(file)
      this.image = file
    }
  }
}
</script>
