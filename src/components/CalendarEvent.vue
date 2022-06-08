<template>
  <div id="calendar-event">
    <div class="alert text-center" :class="alertColor">
      <transition name="fade" mode="out-in">
        <div v-if="!event.edit">
          <div>
            <!-- <strong>{{ priorityDisplayName }}</strong> -->

            <slot
              name="eventPriority"
              :priorityDisplayName="priorityDisplayName"
            >
              <strong>{{ priorityDisplayName }}</strong>
            </slot>
          </div>
          <!-- <div>{{ event.title }}</div> -->

          <slot :event="event">
            <div>{{ event.title }}</div>
          </slot>

          <div>
            <i class="fas fa-edit me-2" role="button" @click="editEvent"></i>
            <i class="far fa-trash-alt" role="button" @click="deleteEvent"></i>
          </div>
        </div>
        <div v-else>
          <input
            type="text"
            class="form-control"
            ref="newEventTitleInput"
            :value="newEventTitle"
            @input="setNewEventTitle"
          />
          <select class="form-select mt-2" v-model="newEventPriority">
            <option value="-1">Hoch</option>
            <option value="0">Mittel</option>
            <option value="1">Tief</option>
          </select>
          <hr />
          <i class="fas fa-check" role="button" @click="updateEvent"></i>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import Store from '../store.js';

export default {
  name: 'CalendarEvent',
  props: {
    event: Object,
    day: Object,
  },
  data() {
    return {
      newEventTitle: this.event.title,
      newEventPriority: this.event.priority,
    };
  },
  computed: {
    priorityDisplayName() {
      switch (this.event.priority) {
        case 1:
          return 'Tief';
        case 0:
          return 'Mittel';
        case -1:
          return 'Hoch';
      }
      return 'Unbekannte Priorität';
    },
    alertColor() {
      return 'alert-' + this.event.color;
    },
  },
  methods: {
    setNewEventTitle(event) {
      this.newEventTitle = event.target.value;
    },
    deleteEvent() {
      Store.mutations.deleteEvent(this.day.id, this.event.title);
    },
    editEvent() {
      Store.mutations.editEvent(this.day.id, this.event.title);
      this.$nextTick(() => {
        if (this.$refs.newEventTitleInput) {
          this.$refs.newEventTitleInput.focus();
        }
      });
    },
    updateEvent() {
      Store.mutations.updateEvent(this.day.id, this.event.title, {
        title: this.newEventTitle,
        priority: this.newEventPriority,
      });
    },
  },
};
</script>

<style scoped></style>
