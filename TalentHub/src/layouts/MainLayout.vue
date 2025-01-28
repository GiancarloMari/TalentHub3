<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Freelacer Aplikacija
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label header>
          Navigacija
        </q-item-label>

        <q-item 
          v-for="link in linksList" 
          :key="link.title" 
          clickable 
          tag="router-link" 
          :to="link.link"
        >
          <q-item-section avatar>
            <q-icon :name="link.icon" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ link.title }}</q-item-label>
            <q-item-label caption>{{ link.caption }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'

const linksList = [
  {
    title: 'Freelanceri',
    caption: 'Pregled freelancera',
    icon: 'people',
    link: '/freelanceri'
  },
  {
    title: 'Kategorije usluga',
    caption: 'Različite vrste usluga',
    icon: 'category',
    link: '/kategorije-usluga'
  },
  {
    title: 'Narudžbe',
    caption: 'Vaše narudžbe',
    icon: 'shopping_cart',
    link: '/narudzbe'
  },
  {
    title: 'O nama',
    caption: 'Više informacija',
    icon: 'info',
    link: '/onama'
  },
  {
    title: 'Login',
    caption: 'Prijavite se',
    icon: 'login',
    link: '/'
  },
  {
    title: 'Registracija',
    caption: 'Kreirajte račun',
    icon: 'person_add',
    link: '/registracija'
  }
]

const leftDrawerOpen = ref(false)

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>
