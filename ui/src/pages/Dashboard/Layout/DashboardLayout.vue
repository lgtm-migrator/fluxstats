<template>
  <div
    class="wrapper"
    :class="{'nav-open': $sidebar.showSidebar}"
  >
    <notifications />
    <side-bar>
      <template slot="links">
        <sidebar-item :link="{name: 'Dashboard', icon: 'nc-icon nc-chart-pie-35', path: '/flux/dashboard/overview'}" />
        <sidebar-item :link="{name: 'Flux Network', icon: 'nc-icon nc-grid-45', path: '/flux/network/visualization'}" />
        <sidebar-item :link="{name: 'Node Info', icon: 'nc-icon nc-app'}">
          <sidebar-item :link="{name: 'Node', path: '/flux/nodeinfo/node'}" />
          <sidebar-item :link="{name: 'Benchmark', path: '/flux/nodeinfo/benchmark'}" />
          <sidebar-item :link="{name: 'Application', path: '/flux/nodeinfo/app'}" />
          <sidebar-item :link="{name: 'Hashes', path: '/flux/nodeinfo/hashes'}" />
          <sidebar-item :link="{name: 'Address Info', path: '/flux/nodeinfo/address'}" />
          <sidebar-item :link="{name: 'Geolocation', path: '/flux/nodeinfo/location'}" />
          <sidebar-item :link="{name: 'Connection', path: '/flux/nodeinfo/connection'}" />
          <sidebar-item :link="{name: 'Up Time', path: '/flux/nodeinfo/uptime'}" />
          <sidebar-item :link="{name: 'Daemon', path: '/flux/nodeinfo/daemon'}" />
        </sidebar-item>
        <sidebar-item :link="{name: 'History', icon: 'nc-icon nc-notes'}">
          <sidebar-item :link="{name: 'Info', path: '/flux/nodehistory/historyinfo'}" />
        </sidebar-item>
      </template>
    </side-bar>
    <div class="main-panel">
      <top-navbar />

      <dashboard-content @click.native="toggleSidebar" />

      <content-footer />
    </div>
  </div>
</template>
<script>
import PerfectScrollbar from 'perfect-scrollbar';
import TopNavbar from './TopNavbar.vue';
import ContentFooter from './ContentFooter.vue';
import DashboardContent from './Content.vue';
import 'perfect-scrollbar/css/perfect-scrollbar.css';

function hasElement(className) {
  return document.getElementsByClassName(className).length > 0;
}

function initScrollbar(className) {
  if (hasElement(className)) {
    new PerfectScrollbar(`.${className}`);
  } else {
    // try to init it later in case this component is loaded async
    setTimeout(() => {
      initScrollbar(className);
    }, 100);
  }
}

export default {
  components: {
    TopNavbar,
    ContentFooter,
    DashboardContent,
  },
  mounted() {
    this.initScrollbar();
  },
  methods: {
    toggleSidebar() {
      if (this.$sidebar.showSidebar) {
        this.$sidebar.displaySidebar(false);
      }
    },
    initScrollbar() {
      const docClasses = document.body.classList;
      const isWindows = navigator.platform.startsWith('Win');
      if (isWindows) {
        // if we are on windows OS we activate the perfectScrollbar function
        initScrollbar('main-panel');

        docClasses.add('perfect-scrollbar-on');
      } else {
        docClasses.add('perfect-scrollbar-off');
      }
    },
  },
};

</script>
