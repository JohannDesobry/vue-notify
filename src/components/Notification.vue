<template>
  <div class="notification-container">
    <transition-group name="slide">
      <div
        @click="removeNotif(notif), stopTimeout(notif.id)"
        v-for="notif in notifs"
        :key="notif.id"
        class="notif"
        :class="notif.type"
      >
        <div class="text-wrapper">
          <p>{{notif.text}}</p>
        </div>
      </div>
    </transition-group>
  </div>
</template>

<script>
// eslint-disable-next-line
import { clearTimeout } from "timers";

export default {
  name: "Notification",
  props: {
    message: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      notifs: [],
      timeouts: [],
      counter: 0
    };
  },
  methods: {
    // Create notif
    addNotif(message, expire_time, selected) {
      this.notifs.push({
        id: (this.counter += 1),
        text: message,
        type: selected
      });

      // Remove this notif after X ms
      this.timeouts[this.counter] = setTimeout(
        () => this.removeNotif(this.notifs[0], this.counter),
        expire_time
      );
    },
    // Remove notif from array
    removeNotif(notif) {
      this.notifs.splice(this.notifs.indexOf(notif), 1);
    },

    // Clear timeout when notification is clicked
    stopTimeout(id) {
      window.clearTimeout(this.timeouts[id]);
    }
  }
};
</script>

<style scoped>
/** Style notif */

.notification-container {
  position: fixed;
  right: 0;
  bottom: 0;
  width: auto;
  height: auto;
  z-index: 9999;
}

.notification-container span {
  display: flex;
  flex-direction: column-reverse;
  align-items: flex-end;
}

.notif {
  border-radius: 3px;
  padding: 1.25rem 2.5rem 1.25rem 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  bottom: 2rem;
  right: 2rem;
  z-index: 9999;
  width: 300px;
  cursor: pointer;
}

.notif.is-success {
  background-color: #23d160;
}

.notif.is-danger {
  background-color: #ff3860;
}

.text-wrapper {
  width: 100%;
  word-break: break-all;
  text-align: left;
}

p {
  margin: 0;
  color: #fff;
}

.notif:not(:first-child) {
  margin-bottom: 1.5rem;
}

.notif:first-child {
  margin-bottom: 0;
}

/** Transition notif */

@keyframes fadeInRight {
  from {
    opacity: 0;
    transform: translate3d(100%, 0, 0);
  }

  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

@keyframes fadeOutRight {
  from {
    opacity: 1;
  }

  to {
    opacity: 0;
    transform: translate3d(100%, 0, 0);
  }
}

.slide-enter-active {
  animation: fadeInRight 0.8s ease;
}

.slide-leave-active {
  animation: fadeOutRight 0.8s ease;
}
</style>
