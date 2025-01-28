<template>
  <q-page>
    <q-card class="q-mt-lg">
      <q-card-section>
        <div class="text-h5">Moje narudžbe</div>
      </q-card-section>

      <q-card-section>
        <q-list>
          <q-item v-for="narudzba in narudzbe" :key="narudzba.id">
            <q-item-section>
              <q-item-label>
                Freelancer: {{ narudzba.freelancer }}
              </q-item-label>
              <q-item-label caption>
                Opis posla: {{ narudzba.opis_posla }}
              </q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>

      <q-card-section>
        <q-btn label="Dodaj narudžbu" color="primary" @click="otvoriFormu = true" />
        <q-dialog v-model="otvoriFormu">
          <q-card>
            <q-card-section>
              <div class="text-h6">Nova narudžba</div>
            </q-card-section>
            <q-card-section>
              <q-form @submit.prevent="dodajNarudzbu">
                <q-select v-model="novaNarudzba.freelancer_id" :options="freelanceri" label="Odaberi freelancera" />
                <q-input v-model="novaNarudzba.opis_posla" label="Opis posla" type="textarea" />
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
      narudzbe: [],
      freelanceri: [],
      novaNarudzba: {
        freelancer_id: null,
        opis_posla: '' // Promjena: opis -> opis_posla
      },
      otvoriFormu: false
    };
  },
  mounted() {
    axios.get('http://localhost:3000/narudzbe')
      .then(response => {
        this.narudzbe = response.data;
      })
      .catch(err => {
        console.error('Greška pri dohvaćanju narudžbi:', err);
      });

    axios.get('http://localhost:3000/freelanceri')
      .then(response => {
        this.freelanceri = response.data.map(f => ({
          label: f.ime,
          value: f.id
        }));
      })
      .catch(err => {
        console.error('Greška pri dohvaćanju freelancera:', err);
      });
  },
  methods: {
  dodajNarudzbu() {
    const korisnik = JSON.parse(localStorage.getItem('user'));
    if (!korisnik) {
      Notify.create({
        type: 'negative',
        message: 'Niste prijavljeni! Prijavite se kako biste poslali narudžbu.',
        position: 'top-right',
        timeout: 2000
      });
      return;
    }

    const podaciZaSlanje = {
      freelancer_id: this.novaNarudzba.freelancer_id.value,
      opis_posla: this.novaNarudzba.opis_posla,
      korisnik_id: korisnik.id // Dodaj korisnik_id
    };

    console.log('Podaci za slanje narudžbe:', podaciZaSlanje);

    axios.post('http://localhost:3000/narudzbe', podaciZaSlanje)
      .then(response => {
        if (response.data.success) {
          Notify.create({
            type: 'positive',
            message: 'Narudžba je uspješno dodana!',
            position: 'top-right',
            timeout: 2000
          });

          this.$router.go(); // Osvježi stranicu
        } else {
          Notify.create({
            type: 'negative',
            message: 'Greška pri dodavanju narudžbe!',
            position: 'top-right',
            timeout: 2000
          });
        }
      })
      .catch(err => {
        console.error('Greška pri dodavanju narudžbe:', err);
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
