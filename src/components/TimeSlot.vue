<template>
  <div uk-toggle="target: #modal" @click="handleClick" class="slot-container uk-flex uk-flex-center uk-flex-column" :style="slotStyle">
    <div v-if="mode === 'week'">
      <span>
        {{ timeSlot.status }}
      </span>
    </div>
    <div v-if="mode === 'month'">
      <span>
        {{ timeSlot.status }} - {{ timeSlot.start_time }} {{ timeSlot.end_time }}
      </span>
    </div>
    <!-- <a class="uk-button uk-button-default" href="#modal-center" uk->Open</a> -->
    <div id="modal" class="uk-flex-top" uk-modal>
      <div class="uk-modal-dialog uk-modal-body uk-margin-auto-vertical">
        <button class="uk-modal-close-default" type="button" uk-close></button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    schedule: {
      type: Object,
      required: false,
    },
    timeSlot: {
      type: Object,
      required: false,
    },
    increment: {
      type: String,
      required: false,
    },
    mode: {
      type: String,
      required: true,
      default: 'week',
    }
  },
  data() {
    return {
      base_colors: {
        free: {
          font: 'white',
          back: 'green'
        },
        taken: {
          font: 'white',
          back: 'orange'
        },
        ended: {
          font: 'white',
          back: 'grey'
        },
        blocked: {
          font: 'white',
          back: 'red'
        },
      },
    }
  },
  computed: {
    slotStyle() {
      if (this.mode === 'month') {
        return {
          'width': '100%',
          'height': '25px',
          'font-size': '0.6rem',
          'text-align': 'left',
          'margin': '2px',
          'padding': '2px',
          'border-radius': '10px',
          'color': `${this.base_colors[this.timeSlot.status].font}`,
          'background-color': `${this.base_colors[this.timeSlot.status].back}`
        }
      }
      if (this.mode === 'week') {
        return {
          'top': `${(+this.timeSlot.start_time.split(':')[0] * ((15 / +this.increment) * 200)) + ((+this.timeSlot.start_time.split(':')[1] / this.increment) * 50)}px`,
          'height': `${this.timeSlot.range * ((15 / (+this.increment)) * 200)}px`,
          'width': '100%',
          'align-items': 'center',
          'border': '1px solid rgba(0,0,0,0.1)',
          'position': 'absolute',
          'color': `${this.base_colors[this.timeSlot.status].font}`,
          'background-color': `${this.base_colors[this.timeSlot.status].back}`
        }
      }
    }
  },
  methods: {
    handleClick() {

    }
  }
}
</script>
<style>
  .slot-container:hover {
    cursor: pointer;
  }
</style>


