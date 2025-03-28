<template>
  <section id="projects">
    <h2 class="numbered-heading">
      Some Things I've Built
    </h2>
    <StyledProjectsGrid>
      <StyledProject
        v-for="project, i in projects"
        :key="i"
        class="hide fade-in"
      >
        <div class="project-content">
          <div>
            <p class="project-overline">
              Featured Project
            </p>
            <h3 class="project-title">
              <NuxtLink
                v-if="project?.url"
                :to="project.url"
              >
                {{ project.title }}
              </NuxtLink>
              <span v-else>
                {{ project.title }}
              </span>
              <template v-if="hasCompany(project)">
                <span
                  v-if="hasCompany(project)"
                  class="project-company"
                >
                  &nbsp;@&nbsp;
                </span>
                <NuxtLink
                  v-if="project.company.url"
                  :to="project.company.url"
                >
                  {{ project.company.name }}
                </nuxtLink>
              </template>
            </h3>
            <div class="project-description">
              <ContentDoc :value="project" />

              <Date
                v-if="project.date"
                class="featured-project-date"
                :date="project.date"
                :weekday="false"
                :left="(!isMobile()) && i % 2 === 0"
              />
            </div>
          </div>
          <ul class="project-tech-list">
            <li
              v-for="tech in project.tech"
              :key="tech"
            >
              {{ tech }}
            </li>
          </ul>
          <div class="project-links">
            <NuxtLink
              v-if="project.repo"
              :to="project.repo"
              aria-label="GitHub Link"
            >
              <Icon type="GitHub" />
            </NuxtLink>
            <NuxtLink
              v-if="project.url"
              :to="project.url"
              aria-label="External Link"
            >
              <Icon type="ExternalLink" />
            </NuxtLink>
          </div>
        </div>
        <div class="project-image">
          <div class="image-wrapper">
            <NuxtImg
              :src="`${project.cover}`"
              :alt="project.title"
              loading="lazy"
              class="img-project"
            />
          </div>
        </div>
      </StyledProject>
    </StyledProjectsGrid>
  </section>
</template>

<script lang="ts">
export default {
  name: "Featured",
  data() {
    return {
      size: 0,
    };
  },

  mounted() {
    this.size = window.innerWidth;

    window.addEventListener("resize", () => {
      this.size = window.innerWidth;
    });
  },

  beforeUnmount() {
    window.removeEventListener("resize", () => {
      this.size = window.innerWidth;
    });
  },
  methods: {
    isMobile() {
      return this.size < 768;
    },
  },
};
</script>

<script lang="ts" setup>

const hasCompany = (project: any) => typeof project.company !== "undefined";

// read 'featured projects' data
const { data } = await useAsyncData(
  `projects-${useRoute().path}`,
  async () => {
    const _projectsData = queryContent()
      // match '/projects...'
      .where({ _path: { $regex: "^/projects" } })
      .where({ featured: true })
      .sort({ date: -1, order: 1 })
      .find();
    return _projectsData;
  },
);

const projects = data.value || [];

</script>

<style lang="sass" scoped>
@use "~/styles/transitions"
@use "~/styles/typography"

.project-title
  font-weight: 600
  font-family: typography.font(fancy)
  font-variation-settings: "cuts" 300

</style>
