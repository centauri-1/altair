<template>
  <svg
    fill="none"
    stroke="currentColor"
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 64 64"
    stroke-width="1"
    width="128px"
    height="128px"
    baseProfile="basic"
  >
    <path d="M54,37c2.209,0,4,1.791,4,4v8c0,4.962-4.037,9-9,9H23c-4.963,0-9-4.038-9-9V23c0-4.962,4.037-9,9-9h8c2.209,0,4,1.791,4,4	s-1.791,4-4,4h-8c-0.552,0-1,0.449-1,1v26c0,0.551,0.448,1,1,1h26c0.552,0,1-0.449,1-1v-8C50,38.791,51.791,37,54,37z" /><path d="M56,12c2.209,0,4,1.791,4,4v13c0,2.209-1.791,4-4,4s-4-1.791-4-4v-3.343L40.828,36.829	C40.048,37.609,39.023,38,38,38s-2.048-0.391-2.828-1.171c-1.562-1.562-1.562-4.095,0-5.657L46.343,20H43c-2.209,0-4-1.791-4-4	s1.791-4,4-4H56z" />
  </svg>
</template>

<script lang="ts">
export default {
  name: "ExternalLinkIcon",
};
</script>

<style lang="sass" scoped>
  @use "@/styles/colors"
  svg
    stroke: none
    fill: currentcolor

    &:hover
      fill: colors.color("green")
</style>
