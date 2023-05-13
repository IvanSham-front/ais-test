<template>
  <section class="rooms">
    <section class="rooms__search search">
      <div class="search__icon search-icon"></div>
      <input type="text" class="search__input" v-model="searchText" placeholder="Введите ЖК / корпус / № квартиры / № паркинга"/>
    </section>

    <section class="rooms__filter filters">
      <h2 class="filters__title">Фильтры</h2>
      
      <div class="filters__list">
        <div class="checkbox">
          <input 
            type="checkbox" id="jurFaces" 
            :checked="filters.jur"
            @change="toggleFiltersCheck('jur')"
          >
          <label for="jurFaces">Уступка от юр. лица</label>
        </div>
        <div class="checkbox">
          <input 
            type="checkbox" id="physFaces" 
            :checked="filters.phys"
            @change="toggleFiltersCheck('phys')"
          >
          <label for="physFaces">Уступка от физ. лица</label>
        </div>
        <div class="checkbox">
          <input 
            type="checkbox" id="sale" 
            :checked="filters.sale"
            @change="toggleFiltersCheck('sale')"
          >
          <label for="sale">Продано</label>
        </div>
        <div class="checkbox">
          <input 
            type="checkbox" id="booked" 
            :checked="filters.booked"
            @change="toggleFiltersCheck('booked')"

            >
          <label for="booked">Забронировано</label>
        </div>
      </div>
    </section>

    <section class="rooms__actions actions">
      <div class="checkbox">
        <input 
          type="checkbox" 
          id="selectAll" 
          @change="checkAll"
          :checked="checkAllChecked"
          >
        <label for="selectAll">Все</label>
      </div>

      <button class="delete-button">
        Удалить
        <div class="trash-icon"></div>
      </button>
    </section>

    <section class="rooms__list">
      <room-el 
        v-for="room in filterList" 
        :key="room.id" 
        :room="room"
        @toggleCheckItem="toggleCheckItem"
        />
    </section>
  </section>
</template>

<script>
/* eslint-disable */
import rooms from './rooms.js'
import roomEl from './room.vue'
import './rooms.scss';

export default {
  name: 'RoomsEl',
  data () {
    return {
      rooms,
      searchText: '',
      filters: {
        jur: true,
        phys: true,
        sale: true,
        booked: true
      }
    }
  },
  components: {
    roomEl
  },
  computed: {
    checkAllChecked () {
      for (let room of this.rooms) {
        if (!room.checked) {
          return false
        }
      }
      return true
    },
    searchList () {
      if (!this.searchText) {
        return this.rooms
      }
      const searchList = [];
      this.rooms.forEach(item => {
        console.log(item)
        if (item.housingSet.includes(this.searchText) || item.housingSetDesc.includes(this.searchText) ||
        (item.hasOwnProperty('apartmentNumber') && item.apartmentNumber.toString().includes(this.searchText)) || 
        (item.hasOwnProperty('placeNumber') && item.placeNumber.includes(this.searchText))) {
          searchList.push(item)
        }
      })
      return searchList
    },
    filterList() {
      if (this.filters.jur && this.filters.phys && this.filters.booked && this.filters.sale) {
        return this.searchList;
      }
      let filterList = [...this.searchList];
      if (!this.filters.jur) {
        filterList = filterList.filter((item) => item.status !== 'Уступка от юр. лица')
      }
      if (!this.filters.phys) {
        filterList = filterList.filter((item) => item.status !== 'Уступка от физ. лица')
      }
      if (!this.filters.sale) {
        filterList = filterList.filter((item) => item.status !== 'Продано')
      }
      if (!this.filters.booked) {
        filterList = filterList.filter((item) => item.status !== 'Забронировано')
      }
      return filterList
    }
  },
  methods: {
    addCheckedField () {
      this.rooms = this.rooms.map(item => {
        item.checked = false;
        return item
      })    
    },
    toggleCheckItem (id) {
      const index = this.rooms.findIndex(item => item.id === id);
      this.rooms[index].checked = !this.rooms[index].checked;
    },
    checkAll() {
      if (this.checkAllChecked) {
        this.rooms.forEach(item => item.checked = false)
      } else {
        this.rooms.forEach(item => item.checked = true)
      }
    },
    toggleFiltersCheck(filter) {
      switch(filter) {
        case 'jur': {
          this.filters.jur = !this.filters.jur
          return
        }
        case 'phys': {
          this.filters.phys = !this.filters.phys
          return
        }
        case 'sale': {
          this.filters.sale = !this.filters.sale
          return
        }
        case 'booked': {
          this.filters.booked = !this.filters.booked;
          return;
        }
        default: return
      }
    }
  },
  mounted () {
    this.addCheckedField()
  }
}

</script>