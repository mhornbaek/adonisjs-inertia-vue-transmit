<script setup lang="ts">
import { Head } from '@inertiajs/vue3'
import { getTransmit } from '../lib/transmit'
import { onMounted, ref } from 'vue'

const last_message = ref<string | null>(null)

onMounted(() => {
  const transmit = getTransmit()

  if (transmit) {
    const global_subscription = transmit.subscription('global');

    global_subscription.create();

    global_subscription.onMessage((data: string) => {
      last_message.value = data
    })
  }
})

defineProps<{ version: number }>()
</script>

<template>

  <Head title="Homepage" />

  <div class="container">
    <div class="title">AdonisJS {{ version }} x Inertia x Vue.js</div>

    <span>
      Learn more about AdonisJS and Inertia.js by visiting the
      <a href="https://docs.adonisjs.com/guides/inertia">AdonisJS documentation</a>.
    </span>

    <span v-if="last_message != null">{{ last_message }}</span>
  </div>
</template>
