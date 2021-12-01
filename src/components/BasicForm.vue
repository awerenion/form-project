<template>
  <div>
    <div class="basic-body">
      <div
        class="form"
      >
        <div
          class="closeModal"
          @click="closeModal"
        >
          <img
            class="img"
            src="@/assets/close.png"
            alt="close"
          >
        </div>
        <div class="form-container">
          <div class="form-title">
            Title form
          </div>
          <div class="input-holder">
            <label
              :class="getActive(formData.firstName)"
              for="firstName"
              class="label-input"
            >First Name *</label>
            <input
              id="firstName"
              v-model="formData.firstName"
              class="form-input"
              type="text"
            >
            <div
              v-if="!$v.formData.firstName.required"
              class="error"
            >
              Please fill in first name
            </div>
          </div>
          <div class="input-holder">
            <label
              :class="getActive(formData.lastName)"
              for="lastName"
              class="label-input"
            >Last Name *</label>
            <input
              id="lastName"
              v-model.trim="$v.formData.lastName.$model"
              class="form-input"
              type="text"
            >
            <div
              v-if="!$v.formData.lastName.required"
              class="error"
            >
              Please fill in last name
            </div>
          </div>
          <div class="input-holder">
            <label
              :class="getActive(formData.email)"
              for="mail"
              class="label-input"
            >user@gmail.com</label>
            <input
              id="mail"
              v-model.trim="$v.formData.email.$model"
              class="form-input"
              type="text"
            >
            <div
              v-if="!$v.formData.email.required"
              class="error"
            >
              Please fill in email
            </div>
            <div
              v-if="!$v.formData.email.email"
              class="error"
            >
              Please enter a valid email
            </div>
          </div>
          <div class="form-product">
            <span class="product-title">Product type</span>
            <div class="dropdown-holder">
              <dropdown
                class="dropdown-input"
                :options="payType"
                :disabled="false"
                :max-item="3"
                placeholder="Select product type"
                @selected="selectType"
              />
              <div
                v-if="!$v.formData.productType.required"
                class="error"
              >
                Please select product type
              </div>
            </div>
          </div>
          <basic-box
            v-model="state1"
            class="form-box"
          >
            Additinal feature for $100
          </basic-box>
          <basic-box
            v-model="state2"
            class="form-box"
          >
            Additinal feature for $200
          </basic-box>
          <div class="description-holder">
            <label
              v-if="formData.description"
              :class="getActive(formData.description)"
              for="description"
              class="label-input"
            >Type your comment</label>
            <textarea
              id="description"
              v-model="formData.description"
              placeholder="Type your comment"
              type="text"
              class="description"
            />
          </div>
          <div class="form-price">
            <div>Total price</div>
            <div>
              {{ totalPrice }} $
            </div>
          </div>
          <div class="btn-holder">
            <button
              class="form-button"
              @click="submitInfo"
            >
              Send form
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Dropdown from 'vue-simple-search-dropdown'
import BasicBox from './BasicBox.vue'
import { required, email } from 'vuelidate/lib/validators'

export default {
  name: 'BasicForm',
  components: {
    Dropdown,
    BasicBox
  },
  data () {
    return {
      formData: {
        firstName: '',
        lastName: '',
        email: '',
        description: '',
        productType: ''
      },
      state1: false,
      state2: false,
      typeValue: 0,
      payType: [
        {
          id: 1,
          name: 'Product 50',
          value: 50
        },
        {
          id: 2,
          name: 'Product 100',
          value: 100
        },
        {
          id: 3,
          name: 'Product 300',
          value: 300
        }
      ]
    }
  },
  validations: {
    formData: {
      firstName: {
        required
      },
      lastName: {
        required
      },
      email: {
        required,
        email
      },
      productType: {
        required
      }
    }
  },
  computed: {
    totalPrice () {
      if (this.typeValue) {
        const feature1 = this.state1 ? 100 : 0
        const feature2 = this.state2 ? 200 : 0
        return this.typeValue + feature1 + feature2
      } else {
        return 0
      }
    }
  },
  methods: {
    selectType (type) {
      if (type) {
        this.typeValue = type.value
        this.$v.formData.productType.$model = type.name
      }
    },
    submitInfo () {
      const submitData = { ...this.formData, total: this.totalPrice }
      this.$v.$touch()
      if (!this.$v.$invalid) {
        this.$emit('closeModal')
        console.log(submitData)
      }
    },
    getActive (value) {
      if (value) {
        return {
          'label-active': value
        }
      }
    },
    closeModal () {
      this.$emit('closeModal')
    }
  }

}
</script>

<style scoped>
  .basic-body {
    width: 100%;
  }

  .form {
    position: fixed;
    top: 50%;
    left: 50%;
    width: 380px;
    background: white;
    margin: -250px 0 0 -190px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  }

  .form-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .form-container {
    width: 90%;
    margin: 20px auto;
  }

  .form-title {
    font-weight: 400;
    font-size: 22px;
    letter-spacing: 0.04rem;
    margin-bottom: 15px;
  }

  .form-product {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .dropdown-input /deep/ .dropdown-input {
    min-width: 180px;
    max-width: 180px;
    margin-left: 70px;
  }

  .description {
    width: 100%;
    height: 80px;
    border: 1px solid #BEBEBE;
    border-radius: 2px;
    margin-bottom: 15px;
    max-height: 250px;
    max-width: 100%;
  }

  .form-input {
    width: 100%;
    height: 40px;
    border: 1px solid #BEBEBE;
    border-radius: 2px;
  }

  input[type=text] {
    padding-top: 5px;
    padding-left: 15px;
  }

  .form-price {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .btn-holder {
    margin-top: 20px;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .form-button {
    background: #448AE0;
    border-radius: 4px;
    color: white;
    border: none;
    height: 40px;
    width: 120px;
    font-size: 18px;
    cursor: pointer;
  }

  .label-input {
    position: absolute;
    font-size: 12px;
    padding-left: 15px;
    top: calc(50% - 12px);
    opacity: 0.7;
    z-index: 5;
  }

  textarea[type=text] {
    padding-top: 15px;
    padding-left: 15px;
  }

  .error {
    font-size: 10px;
    color: red;
  }

  .input-holder {
    margin-bottom: 10px;
    position: relative;
  }

  .label-active {
    top: 6%;
    font-size: 12px;
  }

  .dropdown-holder {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .description-holder {
    position: relative;
  }

  .closeModal {
    position: absolute;
    top: 0;
    right: 0;
    margin: 5px;
    cursor: pointer;
  }
</style>
