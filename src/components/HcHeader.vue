<template>
  <header id="main-header">
    <div id="logo">
      <img :src="'./assets/images/google-hashcode-logo.png'" alt="hashcode logo">
      <h1><a href="/">Hashcode Grader</a></h1>
    </div>
    <nav id="options">
      <hc-dropdown
        :values="yearValues"
        @change="onYearChange">
      </hc-dropdown>
      <hc-dropdown
        :values="roundValues"
        :labels="roundLabels"
        @change="$emit('roundChange', $event)">
      </hc-dropdown>
    </nav>
  </header>
</template>

<script>
  import HcDropdown from './HcDropdown.vue'

export default {
  name: 'hc-header',
  props: ['manifest'],
  components: {
    HcDropdown
  },

  data () {
    return {
      selectedYear: null
    }
  },

  created() {
    this.selectedYear = this.yearValues[0]
  },

  methods: {
    onYearChange(year) {
      this.selectedYear = year
      this.$emit('yearChange', year)
    }
  },

  computed: {
    yearValues() {
      return Object
        .keys(this.manifest)
        .map(y => parseInt(y))
        .sort()
        .reverse()
    },

    roundValues() {
      return !this.selectedYear ? []: this.manifest[this.selectedYear].map(r => r.round)
    },

    roundLabels() {
      return !this.selectedYear ? []: this.manifest[this.selectedYear].map(r => r.label || r.round)
    }
  }
}
</script>

<style lang="scss">
  @import "../styles/utils.scss";

  $logo-size: 40px;
  $padding-ds: 32px;

  #main-header {
    display: flex;
    justify-content: space-between;
    // border-bottom: 1px solid #ddd;
    background-color: #fafafa;
    margin-bottom: $padding-ds;

    @include responsive() {
      flex-direction: column;
    }
  }

  #logo {
    display: flex;
    align-items: center;
    padding: $padding-ds;

    @include responsive() {
      justify-content: center;
    }

    img {
      display: block;
      height: $logo-size;
      width: $logo-size;
    }

    h1 {
      font-weight: 700;
      font-size: 24px;
      padding-left: 16px;

      a {
        text-decoration: none;
        color: #363636;
      }
    }
  }

  nav#options {
    display: flex;
    align-items: center;
    padding: $padding-ds;

    @include responsive() {
      justify-content: center;
      padding-top: 0;
    }

    .dropdown {
      margin-right: 16px;

      &:last-child {
        margin-right: 0;
      }
    }
  }
</style>
