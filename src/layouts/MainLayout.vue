<template>
  <q-layout view="hhh Lpr lFf">
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { openURL } from 'quasar'
import BrowserNotifications from '../mixins/BrowserNotifications'
export default {
  name: 'MainLayout',
  mixins: [BrowserNotifications],
  mounted () {
    this.askNotificationPermission()
  },
  created() {
    this.askNotificationPermission()
  }

}
</script>
