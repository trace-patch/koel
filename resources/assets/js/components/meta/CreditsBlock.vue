<template>
  <div v-koel-overflow-fade class="max-h-[9rem] overflow-auto" data-testid="demo-credits">
    Music by
    <ul class="inline">
      <li v-for="credit in credits" :key="credit.name" class="inline">
        <a :href="credit.url" target="_blank">{{ credit.name }}</a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
import { orderBy } from 'lodash'
import { onMounted, ref } from 'vue'
import { http } from '@/services/http'

interface DemoCredits {
  name: string
  url: string
}

const credits = ref<DemoCredits[]>([])

onMounted(async () => {
  credits.value = window.IS_DEMO ? orderBy(await http.get<DemoCredits[]>('demo/credits'), 'name') : []
})
</script>

<style lang="postcss" scoped>
li&:last-child {
  &::before {
    content: ', and ';
  }

  &::after {
    content: '.';
  }
}

li + li {
  &::before {
    content: ', ';
  }
}
</style>
