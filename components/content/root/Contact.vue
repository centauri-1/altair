<template>
  <section
    id="contact"
    class="contact-section"
  >
    <h2 class="numbered-heading overline">
      What's Next?
    </h2>
    <h2 class="title">
      {{ contact.title }}
    </h2>
    <ContentDoc :value="contact" />
  </section>
</template>

<script lang="ts" setup>

import { MarkdownParsedContent } from "@nuxt/content/dist/runtime/types";

const { data: contact } = await useAsyncData(
  "personal-contact",
  async () => {
    const _contactData = await queryContent<MarkdownParsedContent>()
      .where({ category: "contact" })
      .findOne();
    return _contactData;
  },
);
</script>

<style lang="sass">
@use "~/styles/mixins"
@use "~/styles/typography"
@use "~/styles/colors"

.contact-section
  max-width: 600px
  margin: 0 auto
  text-align: center

  @media (max-width: 768px)
    margin: 0 auto 50px

  .overline
    display: block
    margin-bottom: 20px
    color: colors.color("primary-highlight")
    font-family: typography.font("monospace")
    font-size: typography.font-size("m")
    font-weight: 400
    text-decoration: none

    &:before
      bottom: 0
      font-size: typography.font-size("s")
      font-weight: 600

    &:after
      display: none

  .title
    font-size: clamp(40px, 5vw, 60px)
    font-family: typography.font(fancy)
    font-weight: 600
    color: colors.color("lightest-foreground")
    font-variation-settings: "cuts" 300

  .email-link
    @include mixins.big-button
    margin-top: 50px
</style>
