<template>
  <div class="edit-button-container">
    <NuxtLink
      :to="editLink"
      class="edit-button"
    >
      <Icon type="edit" />
      <span> Edit on GitHub</span>
    </NuxtLink>
  </div>
</template>

<script lang="ts">
export default {
  name: "BlogEditButton",
  data() {
    const { path } = useTrimmedPath();
    return {
      editLink: `https://github.com/siavava/amitt.ai/blob/main/content/${path}/index.md`,
    };
  },
};
</script>

<style lang="sass" scoped>
@use "@/styles/colors"
.edit-button-container
  width: 100%
  padding: 2rem 0
  display: inline-flex
  justify-content: flex-end

  .edit-button
    display: inline-flex
    gap: 0.5rem
    line-height: 1em
    justify-content: flex-end
    align-items: center
    font-weight: 500

    & > span
      border-bottom: 2px dotted

    &:hover
      color: colors.color(primary-highlight)
      & > span
        border-bottom: 2px solid

</style>
