<template>
  <div>
    <h1>{{ foo }}</h1>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute, type LocationQueryValue } from 'vue-router'

const route = useRoute()

const locationQueryValueToString = (input: LocationQueryValue): string =>
  input === null ? '' : input

const locationQueryValueOrValuesToString = (
  input: LocationQueryValue | LocationQueryValue[]
): string =>
  typeof input === 'string'
    ? input
    : input === null
    ? ''
    : input.map(locationQueryValueToString).join(',')

const foo = computed(() => locationQueryValueOrValuesToString(route.query.foo))
</script>
