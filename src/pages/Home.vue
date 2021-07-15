<template>
  <q-page>
    <div class="column q-pa-lg">
     <h4>Page d'accueil</h4>
    </div>
  </q-page>
</template>

<script>

import { LocalStorage, SessionStorage } from 'quasar';

let value = LocalStorage.getItem("userToken");
let value_session = SessionStorage.getItem("userToken");

if (value_session && value_session.length==0){
  window.location.href="#/";
}


export default {
  name: 'homePage',

}
</script>
