<template>
  <full-calendar :events="events"></full-calendar>
</template>

<script>
import moment from 'moment';
export default {
  name: 'Planning',
  data() {
    return {
      events: [
        {
          id: 1,
          title: 'event1',
          start: moment().hours(12).minutes(0),
        },
        {
          id: 2,
          title: 'event2',
          start: moment().add(-1, 'days'),
          end: moment().add(1, 'days'),
          allDay: true,
        },
        {
          id: 3,
          title: 'event3',
          start: moment().add(2, 'days'),
          end: moment().add(2, 'days').add(6, 'hours'),
          allDay: false,
        },
      ],
      config: {
        eventClick: (event) => {
          this.selected = event;
        },
      },
      selected: {},
    };
  }
}

</script>