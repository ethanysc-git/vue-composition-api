<template>
  <div id="app">
    <header>

        <nav >
          <va-navbar color="primary" class="mb-2">
            <template #left>
              <va-navbar-item>
                <a href="#" class="top-bar-link">
                  <i class="icofont-spoon-and-fork"></i>
                  <span>首頁</span>
                </a>
              </va-navbar-item>
            </template>
            <template #center>
              <va-navbar-item>
                <a href="#" class="top-bar-link">
                  <span>口罩地圖</span>
                </a>
              </va-navbar-item>
            </template>
            <template #right>
              <va-navbar-item>
                <a href="#" class="top-bar-link">
                  <span>疫苗地圖</span>
                </a>
              </va-navbar-item>
            </template>
          </va-navbar>
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
