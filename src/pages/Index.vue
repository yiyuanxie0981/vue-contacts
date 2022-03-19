<template>
  <q-page class="flex flex-center">
    <Contacts></Contacts>
  </q-page>
</template>

<script>
import Contacts from '../components/Contacts.vue';

export default {
  name: 'PageIndex',
  components: {
    Contacts
  }
}
</script>
