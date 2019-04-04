<template>
  <section>
    <h1>Vue.js Notification</h1>
    <iframe
      src="https://giphy.com/embed/s2qXK8wAvkHTO"
      width="480"
      height="316"
      frameborder="0"
      class="giphy-embed"
      allowfullscreen
    ></iframe>
    <p>
      <a href="https://giphy.com/gifs/party-birthday-celebration-s2qXK8wAvkHTO">via GIPHY</a>
    </p>
    <form @submit.prevent="sendMessageNotif(message, expire_time, selected)">
      <div class="col">
        <fieldset>
          <input
            type="text"
            v-model="message"
            name="message"
            class="notif-message"
            v-bind:class="{ 'has_error': has_error }"
          >
          <div class="invalid-feedback">Please insert a message</div>
        </fieldset>
        <fieldset>
          <select v-model="selected" class="custom-select">
            <option
              v-for="option in options"
              :key="option.id"
              v-bind:value="option.value"
            >{{ option.text }}</option>
          </select>
        </fieldset>
        <fieldset>
          <input type="number" v-model="expire_time" name="expire_time">
        </fieldset>
      </div>
      <div class="col justify-content-center">
        <input type="submit" value="Show notification" class="btn">
      </div>
    </form>
    <Notification ref="notif" message="{message}"/>
    <footer>
      <p>Coded by Johann DESOBRY 👨‍💻</p>
    </footer>
  </section>
</template>

<script>
import Notification from "./Notification.vue";

export default {
  name: "Home",
  components: {
    Notification
  },
  data() {
    return {
      message: "Je suis une belle notif",
      expire_time: 2000,
      selected: "is-success",
      has_error: false,
      options: [
        {
          text: "success",
          value: "is-success"
        },
        {
          text: "danger",
          value: "is-danger"
        },
        {
          text: "info",
          value: "is-info"
        },
        {
          text: "warning",
          value: "is-warning"
        }
      ]
    };
  },
  methods: {
    // Send data to notif
    sendMessageNotif(message, expire_time, selected) {
      if (!this.has_error) {
        this.$refs.notif.addNotif(message, expire_time, selected);
      }
    },

    // Check if form is valid
    validateForm(message) {
      if (message == "") {
        return (this.has_error = true);
      } else {
        return (this.has_error = false);
      }
    }
  },
  mounted() {
    let inputNotif = document.querySelector("input.notif-message");

    // Update status form
    inputNotif.addEventListener("input", () => {
      this.validateForm(this.message);
    });
  }
};
</script>

<style scoped>
/** Style form */

.justify-content-center {
  justify-content: center;
}

form {
  width: 511px;
  margin: auto;
}

input.has_error + .invalid-feedback {
  display: block;
}

input[type="text"].has_error {
  border-color: #dc3545;
}

fieldset {
  margin: 0;
  padding: 0;
  border: none;
}

input[type="text"],
input[type="number"] {
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
}

input[type="text"],
input[type="number"],
input[type="submit"],
select {
  margin: 0 0.4rem 0.75rem 0.4rem;
}

input:focus {
  outline: none;
}

.invalid-feedback {
  padding-left: 8px;
  width: 100%;
  margin-top: 0.25rem;
  font-size: 80%;
  color: #dc3545;
  display: none;
  text-align: left;
}

.custom-select {
  display: inline-block;
  height: calc(2.25rem + 2px);
  padding: 0.375rem 1.75rem 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
}

.custom-select:focus {
  outline: none;
}

.col {
  display: flex;
  max-width: 100vw;
  flex-wrap: wrap;
}

.col:not(:last-child) {
  margin-bottom: 0.75rem;
}

.btn {
  width: calc(50% - 10px);
  cursor: pointer;
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
  display: inline-block;
  font-weight: 400;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  user-select: none;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out;
}

.btn:hover {
  color: #fff;
  background-color: #0069d9;
  border-color: #0062cc;
}

/** Style footer */

footer {
  position: fixed;
  bottom: 20px;
  left: 20px;
}

footer p {
  margin: 0;
}
</style>
