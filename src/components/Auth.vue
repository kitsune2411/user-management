<script setup>
import { ref } from 'vue'
import { supabaseClient } from "../supabase";

const loading = ref(false)
const email = ref('')

const handleLogin = async () => {
    try {
        loading.value = true
        const { error } = await supabaseClient.auth.signInWithOtp({ email: email.value })
        if (error) throw error
        alert('Check your email for the login link!')
    } catch (error) {
        if (error instanceof Error) alert(error.message)
        console.error(error)
    } finally {
        loading.value = false
    }
}
</script>

<template>
    <form class="row flex flex-center" @submit.prevent="handleLogin">
        <div class="col-6 form-widget">
            <h1 class="header">User Management</h1>
            <p class="description">Sign in via magic link with your email below</p>
            <div class="">
                <input type="email" placeholder="example@gmail.com" v-model="email" class="inputField">
            </div>
            <div class="">
                <input type="submit" class="button block" :value="loading ? 'Loading...' : 'Send magic link'" :disabled="loading">
            </div>
        </div>
    </form>
</template>