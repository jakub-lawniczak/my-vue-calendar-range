<template>
  <div>
    <form @submit.prevent="checkForm" novalidate="true">
      <div class="form-data">
        <div class="form-data__row">
          <label class="form-data__label" for="first-name">First Name</label>
          <input
            type="text"
            class="form-data__input"
            id="first-name"
            :class="{ 'form-data__input--error': error.firstName }"
            v-model="userData.firstName"
          />
          <p class="form-data__error">{{ error.firstName }}</p>
        </div>
        <div class="form-data__row">
          <label class="form-data__label" for="last-name">Last Name</label>
          <input
            type="text"
            class="form-data__input"
            id="last-name"
            :class="{ 'form-data__input--error': error.lastName }"
            v-model="userData.lastName"
          />
          <p class="form-data__error">{{ error.lastName }}</p>
        </div>
        <div class="form-data__row">
          <label class="form-data__label" for="email">Mail</label>
          <input
            type="text"
            class="form-data__input"
            id="email"
            :class="{ 'form-data__input--error': error.email }"
            v-model="userData.email"
          />
          <p class="form-data__error">{{ error.email }}</p>
        </div>
        <div class="form-data__row"></div>
      </div>
      <button type="submit" class="form-data__submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ["value"],
  data() {
    return {
      userData: {
        firstName: "",
        lastName: "",
        email: ""
      },
      error: {
        firstName: "",
        lastName: "",
        email: ""
      }
    };
  },
  watch: {
    value: {
      immediate: true,
      handler(newValue, oldValue) {
        if (JSON.stringify(newValue) != JSON.stringify(oldValue)) {
          this.userData = { ...newValue };
        }
      }
    },
    userData: {
      deep: true,
      handler(userData) {
        this.$emit("input", userData);
      }
    }
  },
  methods: {
    checkForm: function() {
      if (!this.userData.firstName) {
        this.error.firstName = "First name required.";
      } else {
        this.error.firstName = null;
      }
      if (!this.userData.lastName) {
        this.error.lastName = "Last name required.";
      } else {
        this.error.lastName = null;
      }
      if (!this.userData.email) {
        this.error.email = "E-mail required.";
      } else if (!this.validEmail(this.userData.email)) {
        this.error.email = "E-mail address is not valid";
      } else {
        this.error.email = null;
      }
    },
    validEmail: function(email) {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    }
  }
};
</script>

<style lang="scss" scoped>
.form-data {
  margin-top: 20px;
  &__row {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 10px;
  }
  &__col {
    width: 50%;
  }
  &__label {
    display: block;
    font-size: 11px;
    font-weight: 700;
  }
  &__input {
    width: 100%;
    background-color: transparent;
    box-shadow: none;
    border: 1px solid #d9d9d9;
    font-size: 18px;
    margin-top: 5px;
    padding: 8px;
    &:focus {
      outline: none;
    }
    &--error {
      border: 1px solid red;
    }
  }
  &__submit {
    display: block;
    border: 1px solid #d9d9d9;
    color: #aaa;
    box-shadow: none;
    background-color: transparent;
    margin-left: auto;
    font-size: 18px;
    margin-bottom: 20px;
    padding: 8px;
    transition: all 0.3s;
    &:hover {
      background-color: #d9d9d9;
      color: #fefefe;
    }
    &:focus {
      outline: none;
    }
  }
  &__error {
    font-size: 14px;
    font-weight: 700;
    color: red;
  }
}
</style>
