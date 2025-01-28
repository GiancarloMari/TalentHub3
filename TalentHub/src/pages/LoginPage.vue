<template>
  <q-page>
    <q-card class="q-mt-lg">
      <q-card-section>
        <div class="text-h5">Prijava</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="login"> <!-- Spriječi default ponašanje -->
          <q-input v-model="email" label="Email" type="email" />
          <q-input v-model="lozinka" label="Lozinka" type="password" />
          <q-btn label="Prijavi se" color="primary" type="submit" />
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from 'axios';
import { Notify } from 'quasar'; // Uvoz Quasar Notify

export default {
  data() {
    return {
      email: '',
      lozinka: ''
    };
  },
  methods: {
    login() {
      axios.post('http://localhost:3000/login', { email: this.email, lozinka: this.lozinka })
        .then(response => {
          if (response.data.success) {
            const korisnik = response.data.user;

            // Spremi korisnika u localStorage
            localStorage.setItem('user', JSON.stringify(korisnik));

            Notify.create({
              type: 'positive',
              message: 'Uspješno ste prijavljeni!',
              position: 'top-right',
              timeout: 2000
            });

            this.$router.push('/freelanceri'); // Preusmjeri korisnika
          } else {
            Notify.create({
              type: 'negative',
              message: 'Pogrešan email ili lozinka!',
              position: 'top-right',
              timeout: 2000
            });
          }
        })
        .catch(err => {
          console.error('Greška pri prijavi:', err);
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
