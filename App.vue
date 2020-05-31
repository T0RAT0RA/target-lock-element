<script>
/* eslint-disable */
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import resourceTimelinePlugin from '@fullcalendar/resource-timeline'
import interactionPlugin from '@fullcalendar/interaction';
import TargetLock from './TargetLock.vue';

export default {
  components: {
    FullCalendar, // make the <FullCalendar> tag available
    TargetLock,
  },
  data() {
    return {
      calendarPlugins: [ dayGridPlugin, resourceTimelinePlugin, interactionPlugin ],
      resources: [
        {
          id: 'A',
          groupId: '1',
          title: 'Resource A'
        },
        {
          id: 'B',
          groupdId: '1',
          title: 'Resource B'
        },
        {
          id: 'C',
          groupId: '2',
          title: 'Resource C'
        }
      ],
      events: [
        { title: 'event 1', start: '2020-05-31T14:30:00Z', end: '2020-05-31T16:30:00Z', resourceId: 'A' },
        { title: 'event 2', start: '2020-05-31T09:30:00Z', end: '2020-05-31T11:15:00Z', resourceId: 'C' }
      ],
      targetElem: null,
      isDragging: false,
    }
  },
  methods: {
    onResizeStart(event) {
      console.log('onResizeStart', event);
      this.enableHover = true;
    },
    onResizeStop(event) {
      console.log('onResizeStop', event);
      this.enableHover = false;
    },
    onDragStart(event) {
      console.log('onDragStart', event);
      this.enableHover = true;
    },
    onDragStop(event) {
      console.log('onDragStop', event);
      this.enableHover = false;
      this.targetElem = null;
    },
    onRender(event) {
      console.log('onRender', event.isMirror);
      if (this.enableHover && event.isMirror) {
        this.targetElem = event.el
      } else if (!this.enableHover && this.targetElem) {
        this.targetElem = null
      }
    },
  }
}

</script>

<template>
    <div>
      <FullCalendar
        editable="true"
        defaultView="resourceTimelineWeek"
        contentHeight="auto"
        height="auto"
        schedulerLicenseKey='GPL-My-Project-Is-Open-Source'
        :plugins="calendarPlugins"
        :resources="resources"
        :events="events"
        @eventDragStart="onDragStart"
        @eventDragStop="onDragStop"
        @eventResizeStart="onResizeStart"
        @eventResizeStop="onResizeStop"
        @eventRender="onRender"
      />
      <TargetLock :targetElem="targetElem" v-if="targetElem">
        <div class="lock" slot-scope="{ position, center }" :style="{
            position:'absolute',
            left: `${center}px`,
            top: `${position.y}px`
          }">
          LOCK
        </div>
      </TargetLock>
    </div>
</template>

<style lang='scss'>

@import '~@fullcalendar/core/main.css';
@import '~@fullcalendar/daygrid/main.css';
@import '~@fullcalendar/timeline/main.css';
@import '~@fullcalendar/resource-timeline/main.css';

.lock {
  height: 100px;
  width: 50px;
  border: 1px solid orangered;
  background: orchid;
  z-index: 1000;
}
</style>
