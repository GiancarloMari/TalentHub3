<template>
  <q-page>
    <q-card class="q-mt-lg">
      <q-card-section>
        <div class="text-h5">Kategorije usluga</div>
      </q-card-section>

      <q-card-section>
        <q-list>
          <q-item v-for="kategorija in kategorije" :key="kategorija.id">
            <q-item-section>
              <q-item-label>{{ kategorija.naziv }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>

      <q-card-section>
        <q-btn label="Dodaj kategoriju" color="primary" @click="otvoriFormu = true" />
        <q-dialog v-model="otvoriFormu">
          <q-card>
            <q-card-section>
              <div class="text-h6">Nova kategorija</div>
            </q-card-section>
            <q-card-section>
              <q-form @submit.prevent="dodajKategoriju">
                <q-input v-model="novaKategorija" label="Naziv kategorije" />
                <q-btn label="Dodaj" color="primary" type="submit" />
              </q-form>
            </q-card-section>
          </q-card>
        </q-dialog>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from 'axios';
import { Notify } from 'quasar';

export default {
  data() {
    return {
      kategorije: [], // Svi prikazani podaci
      novaKategorija: '', // Naziv nove kategorije
      otvoriFormu: false
    };
  },
  mounted() {
    // Dohvati sve kategorije
    axios.get('http://localhost:3000/kategorije')
      .then(response => {
        this.kategorije = response.data;
      })
      .catch(err => {
        console.error('Greška pri dohvaćanju kategorija:', err);
      });
  },
  methods: {
    dodajKategoriju() {
      // Dodaj novu kategoriju putem API-ja
      axios.post('http://localhost:3000/kategorije', { naziv: this.novaKategorija })
        .then(response => {
          if (response.data.success) {
            Notify.create({
              type: 'positive',
              message: 'Kategorija je uspješno dodana!',
              position: 'top-right',
              timeout: 2000
            });

            this.kategorije.push(response.data); // Dodaj novu kategoriju u prikaz
            this.novaKategorija = ''; // Očisti input
            this.otvoriFormu = false; // Zatvori dijalog
          } else {
            Notify.create({
              type: 'negative',
              message: 'Greška pri dodavanju kategorije!',
              position: 'top-right',
              timeout: 2000
            });
          }
        })
        .catch(err => {
          console.error('Greška pri dodavanju kategorije:', err);
          Notify.create({
            type: 'negative',
            message: 'Greška na serveru!',
            position: 'top-right',
            timeout: 2000
          });
        });
    }
  }
};
</script>
