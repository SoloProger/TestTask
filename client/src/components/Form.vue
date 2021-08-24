<template>
  <body class="text-center">
    <main class="form-signin">
      <form @submit.prevent="onSubmit">
        <h1 class="h3 mb-3 fw-normal">Заполните форму</h1>

        <div class="form-floating mb-3">
          <input
            type="text"
            class="form-control"
            id="UserName"
            placeholder="Ваше имя"
            v-model="userName"
            :class="{ invalid: v$.userName.$error }"
          />
          <small
            class="errValid"
            v-for="(error, idx) of v$.userName.$errors"
            :key="idx"
          >
            {{ nameValid(error.$validator) }}
          </small>
          <label for="UserName" class="text-muted">Ваше имя</label>
        </div>

        <div class="form-floating mb-3">
          <input
            type="email"
            class="form-control"
            id="UserEmail"
            placeholder="name@example.com"
            v-model="userEmail"
            :class="{ invalid: v$.userEmail.$error }"
          />
          <small
            class="errValid"
            v-for="(error, idx) of v$.userEmail.$errors"
            :key="idx"
          >
            {{ emailValid(error.$validator) }}
          </small>
          <label for="UserEmail" class="text-muted">Ваш E-mail</label>
        </div>

        <div class="mb-3">
          <label for="UserMessage" class="text-muted form-label"
            >Текст сообщения</label
          >
          <textarea
            class="form-control"
            id="UserMessage"
            rows="3"
            v-model="userMessage"
            :class="{ invalid: v$.userMessage.$error }"
          ></textarea>
          <small
            class="errValid"
            v-for="(error, idx) of v$.userMessage.$errors"
            :key="idx"
          >
            {{ messageValid(error.$validator) }}
          </small>
        </div>
        <div class="checkbox mb-3">
          <label>
            <input type="checkbox" value="remember-me" /> Remember me
          </label>
        </div>
        <button class="w-100 btn btn-lg btn-primary" type="submit">
          Отправить
        </button>
      </form>
    </main>
  </body>
</template>

<script>
import useValidate from "@vuelidate/core";
import { required, email, minLength, maxLength } from "@vuelidate/validators";
import axios from "axios";

export default {
  name: "form",
  setup() {
    return {
      v$: useValidate(),
    };
  },
  data() {
    return {
      userName: "",
      userEmail: "",
      userMessage: "",
    };
  },
  validations: {
    userName: { required, minLength: minLength(3), maxLength: maxLength(10) },
    userEmail: { email, required },
    userMessage: {
      required,
      minLength: minLength(10),
      maxLength: maxLength(100),
    },
  },
  methods: {
    onSubmit() {
      this.v$.$touch();
      if (this.v$.$error) {
        return;
      }
      const formData = {
        userName: this.userName,
        userEmail: this.userEmail,
        userMessage: this.userMessage,
      };

      const url = "http://127.0.0.1:8000/api/emails";

      axios
        .post(url, formData)
        .then((res) => {
          res.data;
          this.userName = "";
          this.userEmail = "";
          this.userMessage = "";
          setTimeout(() => {window.location.href = "/"}, 2000);
        })
        .catch((err) => {
          err.message;
        });
    },
    emailValid($name) {
      if ($name === "required") {
        return "Поле не должно быть пустым";
      } else if ($name === "email") {
        return "Введите корректный email";
      }
    },
    nameValid($name) {
      if ($name === "required") {
        return "Поле не должно быть пустым";
      } else if ($name === "minLength") {
        return "Минимальная длинна должна быть 3 символов";
      } else if ($name === "maxLength") {
        return "Длинна не должна превышать 10 символов";
      }
    },
    messageValid($name) {
      if ($name === "required") {
        return "Поле не должно быть пустым";
      } else if ($name === "minLength") {
        return "Минимальная длинна должна быть 10 символов";
      } else if ($name === "maxLength") {
        return "Длинна не должна превышать 100 символов";
      }
    },
  },
};
</script>

<style>
.text-center {
  text-align: center !important;
}
.errValid {
  color: red;
}
</style>