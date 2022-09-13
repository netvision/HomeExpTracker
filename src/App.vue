<script setup>
  import { initializeApp } from 'firebase/app'
import { getAuth, onAuthStateChanged, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
import { onMounted } from "vue";
const firebaseConfig = {
  apiKey: import.meta.env.VITE_APIKEY,
  authDomain: import.meta.env.VITE_AUTHDOMAIN,
}
const t = import.meta.env.VITE_APIKEY;
const app = initializeApp(firebaseConfig)
const provider = new GoogleAuthProvider();
const auth = getAuth(app)

const googleSignIn = () => {
  signInWithPopup(auth, provider).then((result) => {
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    // The signed-in user info.
    const user = result.user;
    console.log(user);
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // The email of the user's account used.
    const email = error.customData.email;
    // The AuthCredential type that was used.
    const credential = GoogleAuthProvider.credentialFromError(error);
    // ...
    console.log(errorCode)
  });
}
console.log(import.meta.env.VITE_APIKEY)
onMounted(() => {
  onAuthStateChanged(auth, (user) =>{
    if(!user)googleSignIn();
    else console.log(user.uid)
  })
})

</script>

<template>
  <div>
    <header class="bg-white shadow" v-if="$route.meta.title">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <h1
          @click="counter = 0"
          class="text-3xl font-bold leading-tight text-gray-900"
          title="click to reset a counter"
        >
          {{ $route.meta.title }} 
        </h1>
      </div>
    </header>
    <main>
      <router-view />
    </main>
  </div>
</template>
