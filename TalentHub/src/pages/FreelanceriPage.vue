<template>
  <q-page>
    <q-card class="q-mt-lg">
      <q-card-section>
        <div class="text-h5">Freelanceri</div>
      </q-card-section>

      <q-card-section>
        <q-list>
          <q-item v-for="freelancer in freelanceri" :key="freelancer.id">
            <q-item-section>
              <q-item-label>{{ freelancer.ime }}</q-item-label>
              <q-item-label caption>{{ freelancer.usluga }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      freelanceri: [] // Inicijalno prazan niz za freelancere
    };
  },
  mounted() {
    // Dohvati freelancere s backenda
    axios.get('http://localhost:3000/freelanceri')
      .then(response => {
        this.freelanceri = response.data; // Postavi podatke u freelanceri
      })
      .catch(err => {
        console.error('Greška pri dohvaćanju freelancera:', err);
      });
  }
};
</script>
