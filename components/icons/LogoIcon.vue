<!-- eslint-disable max-len -->
<template>
  <div class="container">
    <!-- <div class="logo-text">
      <span class="name first">amitt</span>
      <span class="name second">ai</span>
    </div> -->
    <svg
      width="256px"
      height="256px"
      viewBox="0 0 256 256"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      preserveAspectRatio="xMidYMid"
    >
      <title>LaunchDarkly</title>
      <g>
        <path
          d="M129.865412,255.30629 C127.709796,255.310592 125.740451,254.085222 124.791982,252.149477 C123.873203,250.32725 124.069945,248.141225 125.299325,246.512333 L185.278543,163.928165 L80.2585397,206.826835 C79.5467341,207.136387 78.7798555,207.299348 78.0036819,207.305992 C75.5087308,207.293508 73.3187726,205.64244 72.6202088,203.247248 C71.9042394,200.903721 72.8523572,198.371553 74.9314381,197.074575 L167.296052,142.535201 L4.66443083,133.177541 C1.96746731,132.705081 0,130.362987 0,127.624953 C0,124.886919 1.96746731,122.544826 4.66443083,122.072366 L167.380609,112.742892 L74.9878095,58.2035179 C72.9087286,56.9065395 71.9606108,54.3743714 72.6765803,52.0308445 C73.3721478,49.6455781 75.5473775,47.9969829 78.0318676,47.9721004 C78.8120809,47.9694492 79.5828378,48.1428695 80.2867255,48.4794434 L185.334915,91.4344852 L125.355696,8.87850272 C124.126316,7.24960998 123.929574,5.06358524 124.848353,3.24135814 C125.768246,1.28181499 127.728991,0.0221188008 129.893598,0 C131.374678,0.00984494931 132.788409,0.620088674 133.811413,1.69114338 L253.347064,121.198609 C256.884312,124.741712 256.884312,130.480009 253.347064,134.023113 L133.755042,253.671506 C132.731228,254.717522 131.329087,255.30629 129.865412,255.30629 Z"
          fill="currentColor"
        />
      </g>
    </svg>
  </div>
</template>

<script lang="ts" scoped>
export default {
  name: "LogoIcon",
};
</script>

<style lang="sass" scoped>

@use "~/styles/colors"
@use "~/styles/geometry"
@use "~/styles/typography"

.container
  display: inline-flex
  place-content: center
  height: 70%
  aspect-ratio: 1/1
  padding: 0
  place-items: center
  margin: auto
  margin-left: -1.5px

  svg
    width: 50%
    height: 50%
    color: colors.color(primary-highlight)

  .icon
    transition: geometry.var("default-transition")
    user-select: none
    margin: 0
    padding: 0 !important
    margin: 0 !important
    height: 32px !important
    aspect-ratio: 1/1

    height: 100%
    aspect-ratio: 1/1
    display: none

    &:hover
      //transform: rotate(-180deg)
      transform: scale(1.1)
      cursor: pointer

  .name
    //background: red
    font-family: typography.font("fancy")
    font-weight: 600
    font-variation-settings: "cuts" 300

    &.first
      color: colors.color(one)

      &::after
        content: ""
        color: colors.color(three)

    &.second
      color: colors.color(two)

</style>
