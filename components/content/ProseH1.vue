<template>
  <NuxtLink
    v-if="generate"
    :to="`#${id}`"
    class="prose-title-wrapper"
  >
    <h1
      :id="id"
      class="prose-h1"
    >
      <slot />
    </h1>
  </NuxtLink>
  <div
    v-else
    class="prose-title-wrapper"
  >
    <h1
      :id="id"
      class="prose-h1"
    >
      <slot />
    </h1>
  </div>
  <br>
</template>

<script setup lang="ts">
import { useRuntimeConfig } from "#imports";

defineProps<{ id: string }>();
const heading = 1;
const { anchorLinks } = useRuntimeConfig().public.content;
const generate = anchorLinks?.depth >= heading;
</script>

<style lang="sass" scoped>

@use "~/styles/colors"
@use "~/styles/typography"
@use "~/styles/geometry"

.prose-title-wrapper

  margin-top: 1.5rem
  margin-bottom: 1.5rem

.prose-h1
  font-weight: 600
  font-size: 1.4rem
  color: colors.color("primary-highlight")
  display: inline

  &::before
    content: "#"
    margin-right: 0.5rem
    opacity: 0.5
    transition: geometry.var("default-transition")

  &:hover::before
    opacity: 1
</style>
