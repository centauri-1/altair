<template>
  <svg
    width="40px"
    height="40px"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    class="list-icon-svg"
  >
    <g id="Edit / List_Unordered">
      <path
        id="Vector"
        d="M9 17H19M9 12H19M9 7H19M5.00195 17V17.002L5 17.002V17H5.00195ZM5.00195 12V12.002L5 12.002V12H5.00195ZM5.00195 7V7.002L5 7.00195V7H5.00195Z"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
    </g>
  </svg>
</template>
<script lang="ts">
export default {
  name: "ListIcon",
};
</script>

<style lang="sass" scoped>
@use "@/styles/colors"
.bookmark-icon-svg
  height: 15px !important
  aspect-ratio: 1/1 !important
  color: colors.color(foreground)
</style>
