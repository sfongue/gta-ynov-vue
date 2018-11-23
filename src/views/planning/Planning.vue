<template>
  <div>
    <button id="show-modal" @click="showModal = true">Add event</button>
    <!-- use the modal component, pass in the prop -->
    <modal v-if="showModal" @close="showModal = false">
      <!--
        you can use custom content here to overwrite
        default content
      -->
      <h3 slot="header">Add an event</h3>
      <div slot="body">
        <label>Event :</label><br/>
        <b-form-input v-model="nameEvent"/><br/>
        <label>Date begin :</label><br/>
        <b-form-input type="date" v-model="dateStart" /><br/>
        <label>Time begin :</label><br/>
        <b-form-input type="time" v-model="timeStart" /><br/>
        <label>Date end :</label><br/>
        <b-form-input type="date" v-model="dateEnd" /><br/>
        <label>Time end :</label><br/>
        <b-form-input type="time" name="timeEnd" /><br/>
        
      </div>
      <div slot="footer">
        <b-button variant="primary" class="buttonConfirm" @click="addEvent()">Confirm event</b-button>
        <b-button variant="danger" class="buttonCancel" @click="showModal = false">Cancel</b-button>
      </div>
    </modal>
    <full-calendar :events="events" :config="config"></full-calendar>

    <!-- Modal -->
    <script type="text/x-template" id="modal-template">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">
              <div class="modal-header">
                <slot name="header">
                </slot>
              </div>
              <div class="modal-body">
                <slot name="body">
                  default body
                </slot>
              </div>

              <div class="modal-footer">
                <slot name="footer">
                  <button class="modal-default-button" @click="$emit('close')">
                    OK
                  </button>
                </slot>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </script>
  </div>
</template>

<script>
export default {
  data () {
    return {
      showModal: false,
      events: [],
      config: {
        defaultView: "listWeek"
      }  
    }
  },
  mounted() {
    console.log(localStorage.getItem('events'))
    if(localStorage.getItem('events')) {
      try {
        this.events = JSON.parse(localStorage.getItem('events'));
      } catch(e) {
        localStorage.removeItem('events');
      }
    }
  },
  methods: {
    addEvent() {
      // ensure they actually typed something
      // if(!this.newEvent) return;
      var newEvent = {};
      newEvent.title = this.nameEvent
      if(!this.timeStart) {
        newEvent.start = this.dateStart
      } else {
        newEvent.start = this.dateStart + "T" + this.timeStart + ":00"
      }

      if(!this.timeEnd) {
        newEvent.end = this.dateEnd
      } else {
        newEvent.start = this.dateEnd + "T" + this.timeEnd + ":00"
      }
      this.events.push(newEvent)
      this.saveEvents();
      this.showModal= false;
    },
    removeEvent(x) {
      this.event.splice(x,1);
      this.saveEvent();
    },
    saveEvents() {
      let parsed = JSON.stringify(this.events);
      localStorage.setItem('events', parsed);
    }
  }
  
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 500px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.buttonConfirm {
  margin-right: 1em;
}

</style>
