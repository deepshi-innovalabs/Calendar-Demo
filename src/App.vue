<template>
  <div>
    <button @click="addEvent">Add Event</button>
    <button @click="addRecurringEvent">Add Recurring Event</button>
    <button @click="addMoreRecurringEvent">Add More Recurring Events</button>
    <FullCalendar :options="calendarOptions" />
  </div>
</template>
/* eslint-disable */
<script>
import FullCalendar from "@fullcalendar/vue"
import dayGridPlugin from "@fullcalendar/daygrid"
import rrulePlugin from '@fullcalendar/rrule'
import interactionPlugin from "@fullcalendar/interaction"
export default {
  name: "App",
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  updated () {
    console.log(this.events)
  },
  data () {
    return {
      calendarOptions: {
        plugins: [dayGridPlugin, interactionPlugin, rrulePlugin],
        initialView: "dayGridMonth",
        dateClick: this.handleDateClick,
        eventClick: this.handleEventClick,
        eventChange: this.eventChange,
        events: [],
        editable: true
      }
    }
  },
  methods: {
    eventChange (changeInfo) {
      console.log(changeInfo, 'changed')
    },
    addEvent () {
      this.calendarOptions.events.push({ title: 'Event 1', id: 1, date: '2023-10-14' })
      this.calendarOptions.events.push({ title: 'Event 2', id: 2, date: '2023-10-16' })
    },
    addMoreRecurringEvent () {
      let recurringEvents = [
        {
          title: 'Repeated Event',
          id: 5,
          rrule: {
            dtstart: '2023-07-09',
            freq: 'weekly',
            count: 18,
            bymonthday: [13]
          }
        },
        {
          title: 'REPEATING EVENT',
          id: 6,
          allDay: true,
          rrule: {
            freq: 'weekly',
            byweekday: null,
            bymonthday: null,
            dtstart: '2023-10-15',
            until: '2024-11-15'
          }
        }
      ]
      this.calendarOptions.events.push(recurringEvents[0])
      this.calendarOptions.events.push(recurringEvents[1])
    },
    handleDateClick: function (arg) {
      alert("date click! " + arg.dateStr)
    },
    handleEventClick (info) {
      let eventObj = info.event
      let id = eventObj.id
      if (Number(id) === 1 || Number(id) === 2) {
        let updatedDate = prompt(
          'Clicked ' + eventObj.title + '.\n' +
          'Enter updated start date for this event in YYYY-MM-DD'
        )
        this.calendarOptions.events.forEach(element => {
          if (element.id === Number(id)) {
            debugger
            element.date = updatedDate
          }
        })
        if (updatedDate) {
          alert('Event with title ' + eventObj.title + 's start date is updated')
        }
      } else if (eventObj.id && Number(eventObj.id) !== 1 && Number(eventObj.id) !== 2) {
        let updatedFrequency = prompt(
          'Clicked ' + eventObj.title + '.\n' +
          'Enter updated frequency for this event(weekly/daily/monthly)'
        )
        this.calendarOptions.events.forEach(element => {
          if (element.id === Number(eventObj.id)) {
            element.rrule.freq = updatedFrequency
          }
        })
      } else {
        alert('Clicked ' + eventObj.title)
      }
    },
    addRecurringEvent () {
      let recurringEvents = [
        {
          title: 'recurring event',
          id: 3,
          rrule: {
            dtstart: '2023-07-01',
            freq: 'weekly',
            count: 13,
            bymonthday: [13]
          }
        },
        {
          title: 'RECURRING EVENT',
          id: 4,
          allDay: true,
          rrule: {
            freq: 'weekly',
            byweekday: null,
            bymonthday: null,
            dtstart: '2023-07-05',
            until: '2024-08-05'
          }
        }
      ]
      this.calendarOptions.events.push(recurringEvents[0])
      this.calendarOptions.events.push(recurringEvents[1])
    }
  }
}
</script>

<style>
table td {
  word-break: break-word;
}
</style>
