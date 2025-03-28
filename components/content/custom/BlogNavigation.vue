<template>
  <div
    v-if="toc && toc.links"
    class="navigation"
  >
    <span class="title"> Navigation </span>

    <!-- {{ expandedCategories }} -->
    <!-- <ContentNavigation
      :query="blogContent"
    /> -->
    <ContentNavigation
      v-slot="{ navigation }"
    >
      <!-- {{ navigation }} -->
      <ul v-if="navigation">
        <!-- home button -->
        <li class="blog-category">
          <NuxtLink
            :id="`link-${navigation[0]._path}`"
            :to="`${navigation[0]._path}`"
            class="category-label"
          >
            <span class="category-label-text">
              {{ navigation[0].title }}
            </span>
          </NuxtLink>
        </li>
        <li
          v-for="directory in navigation[1].children"
          :key="directory.id"
        >
          <!-- {{ directory }} -->
          <ul v-if="directory.children">
            <template v-for="category in directory.children">
              <li
                v-if="category && category.children"
                :key="category.id"
                :class="{
                  'blog-category': true,
                  'expanded': expandedCategories.has(category.title.toLowerCase()),
                }"
              >
                <button
                  :id="`link-${category._path}`"
                  :href="`${category._path}`"
                  class="category-label"
                  @click.prevent="toggleCategory(category.title)"
                >
                  <span class="category-label-text">
                    {{ category.title }}
                  </span>
                  <ExpandIcon
                    :id="`navigation-${category.title?.toLowerCase().replace(/ /g, '-')}`"
                    class="expand-icon"
                    type="expand"
                  />
                </button>

                <ul class="category-children">
                  <li
                    v-for="child in category.children"
                    :key="child.id"
                    :class="{
                      'category-child': true,
                      'active': path === child._path,
                    }"
                  >
                    <NuxtLink
                      :id="`link-${child._path}`"
                      :to="`${child._path}`"
                    >
                      {{ child.title }}
                    </NuxtLink>
                  </li>
                </ul>
              </li>
            </template>
          </ul>
        </li>
      </ul>
    </ContentNavigation>
  </div>
</template>

<script lang="ts">
export default {
  name: "BlogNavigation",
  props: {
    activeTocItem: {
      type: String,
      default: "",
    },
  },
  async setup() {
    const { toc } = useContent();
    const { path } = useTrimmedPath();
    const { data: _current } = await useAsyncData(
      `categories@${path}`,
      async () => {
        const _blogs = await queryContent()
          .where({ _path: path })
          .only(["category"])
          .findOne();
        return _blogs;
      },
    );
    const currentCategories: string[] = _current?.value?.category || [];
    return {
      currentCategories,
      toc,
      path,
    };
  },
  data() {
    const expandedCategories = new Set<string>();
    this.currentCategories?.forEach((category) => {
      expandedCategories.add(category.toLowerCase());
    });
    return {
      refreshKey: 0,
      expandedCategories, // : new Set<string>(this.currentCategories),
    };
  },
  watch: {
    expandedCategories() {
      this.$forceUpdate();
    },
  },

  methods: {
    toggleCategory(category: string) {
      const _category = category.toLowerCase();
      const elements = document.querySelectorAll(`#navigation-${_category.replace(/ /g, "-")}`);
      if (this.expandedCategories.has(_category)) {
        this.expandedCategories.delete(_category);
        elements?.forEach((element) => element?.classList.remove("expanded"));
      } else {
        this.expandedCategories.add(_category);
        elements?.forEach((element) => element?.classList.add("expanded"));
      }
    },
  },
};
</script>

<style lang="sass">
@use "~/styles/typography"
@use "~/styles/colors"
@use "~/styles/geometry"

// .list-item
//   margin: 0 !important
.navigation
  line-height: 2
  counter-reset: toc-0
  width: 100%

  .title
    font-size: typography.font-size("xl")
    color: colors.color("primary-highlight")
    font-weight: 700
    line-height: 2
    width: 100%

  .blog-category
    font-size: typography.font-size("m")
    font-weight: 700
    position: relative
    margin: 0
    height: fit-content
    transition: height 5.2s ease-in-out

    & > ul
      display: block
      height: 100%
      transition-delay: 0.2s

    &:not(.expanded) > ul
      display: none
      height: 0%

    &.active
      color: colors.color("primary-highlight")

    .category-label
      height: fit-content
      display: inline-flex
      width: 100%
      font-size: typography.font-size("xs")
      // background: yellow

      .category-label-text
        vertical-align: bottom
        line-height: 1
        vertical-align: middle
        text-transform: uppercase
        width: fit-content
        padding-top: 0.5em
        transition: geometry.var("default-transition")

        &:hover
          color: colors.color("primary-highlight")

      .expand-icon
        position: absolute
        right: 0

    .category-child
      font-size: typography.font-size("xxs")
      font-weight: 400
      margin-left: 0.3rem
      padding-left: 0.5rem
      border-left: 1px solid

      &:hover > *
        transform: translateX(-6px)

      &.active
        color: colors.color("primary-highlight")
        font-weight: 600

</style>
