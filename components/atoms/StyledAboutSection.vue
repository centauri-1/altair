<template>
  <section class="about-section">
    <h2 class="numbered-heading">
      About Me
    </h2>
    <div class="inner">
      <slot />
    </div>
  </section>
</template>

<script lang="ts">
export default {
  name: "StyledAboutSection",
};
</script>

<style lang="sass">
@use "../styles/typography"
@use "../styles/colors"
@use "../styles/geometry"

.about-section
  max-width: 900px
  .inner
    display: grid
    grid-gap: 50px
    @media (max-width: 768px)
      display: block

  ul
    width: fit-content
</style>
