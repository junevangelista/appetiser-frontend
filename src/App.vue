<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <event-form @event:created="appendItemsIntoCalendar"></event-form>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Appetiser Apps Coding Challenge</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <div>
        <v-btn icon class="ma-2" @click="$refs.calendar.prev()">
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
        <v-btn icon class="ma-2" @click="$refs.calendar.next()">
          <v-icon>mdi-chevron-right</v-icon>
        </v-btn>
        <v-calendar
          ref="calendar"
          v-model="value"
          type="month"
          :events="events"
        ></v-calendar>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import EventForm from "./components/EventForm.vue";

export default {
  components: { EventForm },

  data: () => ({
    drawer: null,

    value: "",
    events: [],
  }),

  methods: {
    appendItemsIntoCalendar(items) {
      // push new items into the events array
      items.forEach((item) => {
        this.events.push(item);
      });
    },
  },

  async mounted() {
    try {
      const res = await axios.get("/events");
      this.events = res.data;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>
