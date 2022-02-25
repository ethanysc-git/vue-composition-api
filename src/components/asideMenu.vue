<template>
  <!-- aside-menu 左側欄 -->
  <div class="aside-menu">

    <div class="wraps">
      
        
        <va-select
            v-model="currCity"
            class="mb-3"
            placeholder="Default (solid)"
            :options="cityList">
          <template #prepend>
            <div>
                <va-icon tag="div" @click.stop name="location_city" />
                <va-icon 
                  tag="div" 
                  size="small"
                  class="mr-4"
                  name="縣市:"/>
            </div>
          </template>
        </va-select>
      

        <va-select
            v-model="currDistrict"
            class="mb-3"
            :options="districtList"
            :text-by="(option) => option.name"
            :value-by="(option) => option.name"
            >
          <template #prepend>
            <div>
                <va-icon tag="div" @click.stop />
                <va-icon 
                  tag="div" 
                  size="small"
                  class="mr-4"
                  name="行政區："/>
            </div>
          </template>
        </va-select>
        <div>
          <va-input
            type="text"
            class="mb-4"
            v-model="keywords"
            placeholder="請輸入關鍵字">
            <template #prepend>
              <div>
                <va-icon tag="div" name="search" />
                <va-icon 
                  tag="div" 
                  size="small"
                  class="mr-4"
                  name="關鍵字"/>
              </div>
            </template>  
          </va-input>
        </div>
    </div>

    <ul class="store-lists">
      <li class="store-info wraps" 
        v-for="s in filteredStores" :key="s.id" @click="$emit('triggerMarkerPopup', s.id)">

        <div class="mask-info">
        <va-icon name="medication" />
        <va-icon v-html="keywordHighlight(s.name)"/>
        </div>

        <div class="mask-info">
          <va-icon name="person" />
          <span>大人口罩: {{ s.mask_adult }} 個</span>
        </div>

        <div class="mask-info">
          <va-icon name="child_care" />
          <span>兒童口罩: {{ s.mask_child }} 個</span>
        </div>

        <div class="mask-info">
        <va-button size="small" class="mr-4" @click="openInfoBox(s.id)">  
          <va-icon name="info" />
          詳細資訊
        </va-button>

        </div>
        <div>
        <va-icon name="update" />
        <va-icon 
          tag="div" 
          name="最後更新時間:" 
          size="small"
          />
        <va-icon 
          size="small"
          class="mr-4"
          v-html="s.updated"/>
        </div>


        

      </li>
    </ul>

  </div>
</template>

<script>
import { mapGetters, mapState } from 'vuex';

export default {
  name: 'asideMenu',
  computed: {
    currCity: {
      get() {
        return this.$store.state.currCity;
      },
      set(value) {
        this.$store.commit('setcurrCity', value);
      },
    },
    currDistrict: {
      get() {
        return this.$store.state.currDistrict;
      },
      set(value) {
        this.$store.commit('setcurrDistrict', value);
      },
    },
    keywords: {
      get() {
        return this.$store.state.keywords;
      },
      set(value) {
        this.$store.commit('setKeywords', value);
      },
    }, 
    showModal: {
      get() {
        return this.$store.state.showModal;
      },
      set(value) {
        this.$store.commit('setshowModal', value);
      },
    },
    infoBoxSid: {
      get() {
        return this.$store.state.infoBoxSid;
      },
      set(value) {
        this.$store.commit('setInfoBoxSid', value);
      },
    },
    ...mapGetters(['cityList', 'districtList', 'filteredStores']),
  },
  watch: {
    districtList(v) {
      const [arr] = v;
      this.currDistrict = arr.name;
    },
  },
  methods: {
    openInfoBox(sid) {
      this.showModal = true;
      this.infoBoxSid = sid;
    },
    keywordHighlight(val) {
      return val.replace(new RegExp(this.keywords, 'g'), `<span class="highlight">${this.keywords}</span>`);
    },
  },
}
</script>

<style>
.highlight {
  color: #f08d49;
}
</style>