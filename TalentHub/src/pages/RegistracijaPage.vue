<template>
  <q-page>
    <q-card class="q-mt-lg">
      <q-card-section>
        <div class="text-h5">Registracija</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="registriraj">
          <q-input v-model="ime" label="Ime" type="text" />
          <q-input v-model="email" label="Email" type="email" />
          <q-input v-model="lozinka" label="Lozinka" type="password" />
          <q-btn label="Registriraj se" color="primary" type="submit" />
        </q-form>
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
      ime: '',
      email: '',
      lozinka: ''
    };
  },
  methods: {
    registriraj() {
      axios.post('http://localhost:3000/registracija', {
        ime: this.ime,
        email: this.email,
        lozinka: this.lozinka
      })
        .then(response => {
          if (response.data.success) {
            Notify.create({
              type: 'positive',
              message: 'Korisnik uspješno registriran!',
              position: 'top-right',
              timeout: 2000
            });
            this.$router.push('/login');
          } else {
            Notify.create({
              type: 'negative',
              message: 'Greška pri registraciji!',
              position: 'top-right',
              timeout: 2000
            });
          }
        })
        .catch(err => {
          console.error('Greška pri registraciji:', err);
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
