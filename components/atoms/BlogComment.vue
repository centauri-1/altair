<template>
  <div class="comment-wrapper">
    <div
      v-if="comment.text"
      :id="`comment-${id}`"
      class="comment"
    >
      <div class="comment-header">
        <div
          v-if="comment.avatar && comment.text"
          class="comment-avatar"
          v-html="comment.avatar"
        />
        <div class="comment-meta">
          <span class="comment-author">
            {{ comment.author }}
          </span>
          <span
            v-if="comment.date"
            class="comment-date"
          >
            {{ new Date(comment.date).toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' }) }}
          </span>
        </div>
      </div>
      <div class="comment-body">
        <ContentRenderer
          class="markdown-comment"
          :value="parsedMarkdown"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import markdownParser from "@nuxt/content/transformers/markdown";
// import { Comment } from "~/modules/utils";

interface Comment {
  text: string,
  author: string,
  avatar: string,
  date: string,
  path: string,
}

// types

export default {
  name: "BlogComment",
  components: {},

  // define props
  props: {
    comment: {
      type: Object as () => Comment,
      required: true,
    },
    id: Number,
  },
  // init
  async setup(props) {
    return {
      parsedMarkdown: await markdownParser.parse(props.id, props.comment.text),
    };
  },
};
</script>

<style lang="sass">
@use "~/styles/colors"
@use "~/styles/typography"

.comment-wrapper
  display: flex
  flex-direction: row
  width: 100%

  .comment
    //border: 3px solid colors.color("light-background")
    border-radius: 0.5rem
    line-height: 1.5
    align-self: flex-end
    width: 100%

    .comment-body
      white-space: pre-line
      line-height: 1.3

    .comment-header
      color: colors.color("primary-highlight")
      height: 2em
      line-height: 2em
      display: inline-flex
      width: 100%
      height: 42.4px
      margin: 0 0 6px

      .comment-avatar
        width: 32px
        height: 32px
        border-radius: 50%

      .comment-meta
        display: flex
        flex-direction: column
        width: fit-content
        height: 100%
        padding: 0 0 0 12px
        // background: red

        .comment-author
          font-size: 0.9em
          font-weight: 400
          color: colors.color("primary-highlight")
          text-align: left
          align-self: flex-start
          line-height: 20px
          height:  20px

        .comment-date
          font-size: 0.9em
          // margin-left: 0.5em
          line-height: 1
          color: colors.color("secondary-highlight")
          text-align: left
          font-weight: 400
          height: 20px
          line-height: 20px

    .markdown-comment
      padding: 1em 1em

      .prose-ul
        font-size: 0.9em

        .prose-li
          font-size: 1em
          line-height: 1
          margin: 0.4em 0
</style>
