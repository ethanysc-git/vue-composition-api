<template>
  <div id="app">
    <header>
        <nav>
          <a href="#">
            <span>Home</span>
          </a>
        </nav>
        <nav>
          <a href="#">
            <span>Mask Map</span>
          </a>
        </nav>
        <nav>
          <a href="#">
            <span>Vaccine Map</span>
          </a>
        </nav>
    </header>
    <aside-menu @triggerMarkerPopup="openPopup" ref="menu" />
    <mask-map ref="map" />
    <light-box />
    
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import asideMenu from './components/asideMenu.vue';
import lightBox from './components/lightbox.vue';
import maskMap from './components/maskMap.vue'; 

export default {
  name: 'App',
  components: {
    asideMenu,
    lightBox,
    maskMap
  },
  methods: {
    ...mapActions(['fetchLocations', 'fetchPharmacies']),
    openPopup(id) {
      this.$refs.map.triggerPopup(id);
    },
  },
  mounted () {
    this.fetchLocations();
    this.fetchPharmacies();
  }
}
</script>

<style lang="scss" src="./style.scss"></style>
