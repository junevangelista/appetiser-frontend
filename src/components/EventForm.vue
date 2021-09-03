<template>
  <div class="pa-6">
    <v-text-field
      v-model="form.name"
      label="Event Name"
      required
    ></v-text-field>

    <v-menu
      ref="startDateMenu"
      v-model="startDateMenu"
      :close-on-content-click="false"
      :return-value.sync="form.startDate"
      transition="scale-transition"
      offset-y
      min-width="auto"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="form.startDate"
          label="Start Date"
          prepend-icon="mdi-calendar"
          readonly
          v-bind="attrs"
          v-on="on"
        ></v-text-field>
      </template>
      <v-date-picker v-model="form.startDate" no-title scrollable>
        <v-spacer></v-spacer>
        <v-btn text color="primary" @click="startDateMenu = false">
          Cancel
        </v-btn>
        <v-btn
          text
          color="primary"
          @click="$refs.startDateMenu.save(form.startDate)"
        >
          OK
        </v-btn>
      </v-date-picker>
    </v-menu>

    <v-menu
      ref="endDateMenu"
      v-model="endDateMenu"
      :close-on-content-click="false"
      :return-value.sync="form.endDate"
      transition="scale-transition"
      offset-y
      min-width="auto"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
          v-model="form.endDate"
          label="End Date"
          prepend-icon="mdi-calendar"
          readonly
          v-bind="attrs"
          v-on="on"
        ></v-text-field>
      </template>
      <v-date-picker v-model="form.endDate" no-title scrollable>
        <v-spacer></v-spacer>
        <v-btn text color="primary" @click="endDateMenu = false">
          Cancel
        </v-btn>
        <v-btn
          text
          color="primary"
          @click="$refs.endDateMenu.save(form.endDate)"
        >
          OK
        </v-btn>
      </v-date-picker>
    </v-menu>

    <v-checkbox
      v-for="day in days"
      :key="day"
      v-model="form.days"
      :label="day"
      :value="day"
      dense
    ></v-checkbox>

    <v-btn @click="handleSubmit">Submit</v-btn>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      startDateMenu: false,
      endDateMenu: false,

      days: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],

      form: {
        name: "",
        startDate: "",
        endDate: "",
        days: [],
      },
    };
  },

  methods: {
    async handleSubmit() {
      try {
        const res = await axios.post("/events", this.form);

        this.$emit("event:created", res.data);

        // reset form
        this.resetForm();
      } catch (error) {
        console.log(error);
      }
    },

    resetForm() {
      this.form = {
        name: "",
        startDate: "",
        endDate: "",
        days: [],
      };
    },
  },
};
</script>
