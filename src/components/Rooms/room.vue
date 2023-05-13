<template>
  <div class="room" :class="room.checked && 'checked'">
    <div class="room__header">
      <div class="room__title">
        <p class="room__price">{{ room.price }}</p>
        <div class="room__type">
          <div class="room__type-icon" :class="roomTypeClass(room.type)"></div>
          <span>{{ room.type }}</span>
          <div class="tooltip">Подземная встроенно-пристроенная</div>
        </div>
        <p class="room__housing-set">{{ room.housingSet }}, <small>
          {{ room.housingSetDesc }}
          </small>
        </p>
      </div>

      <div class="room__status" :class="roomStatusClass(room.status)">
          <span>{{ room.status }}</span>
      </div>
    </div>

    <div class="room__main">
      <div class="room__basic-info" v-if="room.type === 'Квартира'">
        <div class="room__basic-info-item">
          <p>кв. {{ room.apartmentNumber }}</p>
          <p>{{ room.numberOfRooms }} комн. кв</p>
        </div>
        <div class="room__basic-info-item">
          <p>{{ room.quadrature }} м<sup>2</sup></p>
          <p> {{ room.floor }} этаж</p>
        </div>
        <div class="room__address">
          <div class="icon-place"></div>
          <span>{{ room.adress }}</span>
        </div>
      </div>

      <div v-else class="room__basic-info">
        <div class="room__basic-info-item">
          <p>{{ room.placeNumber }}</p>
        </div>
        <div class="room__basic-info-item">
          <p>{{ room.quadrature }} м<sup>2</sup></p>
        </div>

        <div class="room__address">
          <div class="icon-place"></div>
          <span>{{ room.adress }}</span>
        </div>
      </div>

      <div class="room__desc-info">
        <img src="@/assets/images/room.png" class="room__layout"/>
        <p class="room__date">Добавлено {{ room.date }}</p>
      </div>
    </div>
    <div class="checkbox room__checkbox">
      <input 
        type="checkbox" 
        :id="'check' + room.id" 
        @change="toogleCheck"
        :checked="room.checked"
        />
      <label :for="'check' + room.id"></label>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'roomEl',
  props: {
    room: {
      require: true,
      type: Object
    }
  },
  methods: {
    roomTypeClass(type) {
      switch (type) {
        case 'Квартира': return 'icon-apartments';
        case 'Паркинг' : return 'icon-parking';
        default: return ''
      }
    },
    roomStatusClass(status) {
      switch(status) {
        case 'Уступка от юр. лица': return 'jurFaces';
        case 'Уступка от физ. лица': return 'fysFaces';
        case 'Забронировано': return 'booked';
        case 'Продано': return 'sale';
        default: return ''
      }
    },
    toogleCheck () {
      this.$emit('toggle-check-item', this.room.id)
    }
  }
}
</script>