<template>
  <div class="misc">
    <div id="wanderer-container" />
    <div
      id="wanderer"
      :style="{ 'background-color': getColor(activeCallOutIndex) }"
    />
  </div>
  <section id="hero">
    <div class="hero">
      <div class="hero-container">
        <div class="blurb-container">
          <div class="big-title">
            <span class="strike-through">
              {{ "Talkers" }}
            </span>
            <span>Doers.</span>
          </div>
          <h1
            class="pique"
            :style="{ 'color': getColor(activeCallOutIndex) }"
          >
            sure, let's talk about it.
          </h1>

          <div class="digression">
            <span class="normal"> but first, let's </span>
            <span
              id="action"
              :style="{ 'color': getColor(activeCallOutIndex) }"
            >
              {{ currentAction }}
            </span>
          </div>
        </div>
      </div>
      <div class="hero-footer">
        <div class="down-link">
          <NuxtLink
            class="down-link-inner"
            to="/#about"
            aria-label="scroll down to about section"
          >
            <Icon
              type="down-arrow"
              :style="{ 'color': getColor(activeCallOutIndex) }"
            />
          </NuxtLink>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
const { heroFootItems, heroCallOuts } = useConfig();
export default {
  name: "Hero",
  data() {
    return {
      footItemIndex: 0,
      activeCallOutIndex: 0,
      heroColorsDict: {
        teal: "#16f1d1",
        purple: "#a374ff",
        khaki: "#ffd074",
        white: "#ecf7fa",
        black: "#282723",

        one: "#EE765C",
        two: "#3A594D",
        three: "#BD8FC0",
      },
      currentAction: heroCallOuts[0].action,
      changingAction: null,
    };
  },
  computed: {
    activeCallOut() {
      const index = this.activeCallOutIndex % heroCallOuts.length;
      return heroCallOuts[index];
    },
    heroColors() {
      return Array.from([
        this.heroColorsDict.khaki,
        // this.heroColorsDict.one,
        this.heroColorsDict.three,
        this.heroColorsDict.purple,
        this.heroColorsDict.two,
        // this.heroColorsDict.teal,
      ]);
    },
    font() {
      const fonts = [
        "cyber",
        // "fredericka",
        // "megrim",
        // "macondo",
        // "rubik-puddles",
        // "DM Mono",
      ];
      return fonts[Math.floor(Math.random() * fonts.length)];
    },
    footItems() {
      const active = [];

      for (let i = this.footItemIndex; i < this.footItemIndex + 3; i++) {
        const index = i % heroFootItems.length;
        active.push(index);
      }
      return active;
    },
  },
  watch: {
    currentAction() {
      this.$forceUpdate();
    },
  },
  mounted() {
    // this.changeAction();
    this.tick();

    // on mouse move, move the wanderer
    const wanderer = document.getElementById("wanderer");
    const wandererContainer = document.getElementById("wanderer-container");
    wandererContainer.onpointermove = (e) => {
      // animate wanderer
      const { clientX: x, clientY: y } = e;

      // offset by scroll
      const { top } = wandererContainer.getBoundingClientRect();
      const yOff = top;

      wanderer.animate({
        top: `${y - yOff}px`,
        left: `${x}px`,
      }, {
        duration: 3000,
        fill: "forwards",
      });
      // }
    };
  },

  unmounted() {
    clearInterval(this.changingActions);

    // remove the wanderer event listener
    document.removeEventListener("mousemove", () => {});
  },

  methods: {
    changeAction() {
      const curr = this.activeCallOutIndex % heroCallOuts.length;
      const nextAction = heroCallOuts[curr].action;
      const actionElement = document.getElementById("action");

      if (actionElement.innerText.length > nextAction.length) {
        actionElement.innerText = actionElement.innerText.slice(0, nextAction.length);
      } else {
        for (let i = actionElement.innerText.length; i < nextAction.length; i++) {
          actionElement.innerText += String.fromCharCode(Math.floor(Math.random() * 26) + 97);
        }
      }

      // randomize the entire word
      let iterations = 0;
      const shuffle = setInterval(() => {
        if (iterations >= 2 * nextAction.length) {
          clearInterval(shuffle);
        }

        actionElement.innerText = actionElement.innerText
          .split("")
          .map((char, index) => {
            const randomChar = String.fromCharCode(Math.floor(Math.random() * 26) + 97);
            const modIndex = iterations - nextAction.length;
            if (iterations < nextAction.length) {
              return (index <= iterations)
                ? randomChar
                : char;
            } else {
              return (index === modIndex)
                ? nextAction.charAt(index)
                : char;
            }
          }).join("");
        iterations++;
      }, 60);
    },
    getColor(index: number) {
      const i = index % this.heroColors.length;
      return this.heroColors[i];
    },
    tick() {
      this.changingActions = setInterval(() => {
        this.footItemIndex = (this.footItemIndex + 1) % heroFootItems.length;
        this.activeCallOutIndex += 1 % 1000;
        this.changeAction();
      }, 5000);
    },
  },
};
</script>

<style lang="sass">
@use "~/styles/mixins"
@use "~/styles/colors"
@use "~/styles/typography"
@use "~/styles/geometry"

.misc
  position: absolute
  top: 0
  left: 0
  width: 100svw
  height: 100svh
  overflow: hidden

  @media screen and (max-width: 960px)
    display: none

#wanderer-container
  background-color: rgba(colors.color(background), 0.9)
  background: linear-gradient(rgba(colors.color(background), 1), rgba(colors.color(background), 0.9), rgba(colors.color(background), 1))
  width: 200svw
  height: 100svh
  position: absolute
  top: 0
  left: 0
  z-index: 2 !important
  pointer-events: all
  backdrop-filter: blur(80px)

#wanderer
  width: 400px
  aspect-ratio: 1/1
  border-radius: 50%
  z-index: 1 !important
  position: absolute
  top: 0
  left: 0
  transform: translate(-50%, -50%)

  transition: all 3s ease-in-out

.profile
  background: colors.color("light-background")
  width: 400px
  aspect-ratio: 2/1
  border-radius: geometry.var("border-radius")

#hero
  min-height: calc(100svh - 2 *  geometry.var("nav-height"))
  width: 100%
  position: relative
  z-index: 3
  pointer-events: none

  .hero
    @include mixins.flex-center
    flex-direction: column
    width: 100%
    min-height: 100%
    align-items: center
    position: absolute
    top: 0
    left: 0

  .hero-container
    .blurb-container
      width: fit-content
      font-family: typography.font("sans-serif")

      &.inactive
        display: none

      .big-title
        width: fit-content
        //font-family: typography.font("big-heading")
        font-family: typography.font("fancy")
        font-variation-settings: "cuts" 300
        color:  darken(colors.color(primary-highlight), 10%) //white
        font-size: clamp(45px, 8vw, 100px)
        text-transform: uppercase
        font-weight: 600
        margin: 1rem 0
        padding: 0
        line-height: 1

        & > .strike-through
          // background: yellow
          text-decoration: line-through
          text-decoration-thickness: clamp(5px, 1vw, 10px)
          text-decoration-color: darken(colors.color(primary-highlight), 10%)
          //color: colors.color(dark-foreground)
          padding-right: 0.5em
          margin-right: 0

          //color: black
          -webkit-text-fill-color: transparent //colors.color(background)
          -webkit-text-stroke-color: darken(colors.color(primary-highlight), 30%)
          -webkit-text-stroke-width: clamp(1px, 0.3vw, 3px)

    .pique
      margin: 1.5em 0 0.5em 0.2rem
      margin: 0 0 0 0.5em
      line-height: 2

      font-size: clamp(typography.font-size("l"), 3vw, typography.font-size("xl"))
      font-weight: 800
      width: 100%
      font-variation-settings: "cuts" 50
      // animate change of color
      -webkit-transition: all 3s ease-in-out
      -moz-transition: all 3s ease-in-out
      -ms-transition: all 3s ease-in-out
      -o-transition: all 3s ease-in-out
      transition: all 3s ease-in-out

      @media screen and (min-width: 720px)
        line-height: 3

    .digression
      font-weight: 400
      margin: 0.5rem 0 1.5rem 0.2rem
      margin: 0 0 0 0.5em
      line-height: 1

      &:is(:last-child)::after
        content: '.'
        color: colors.color("white")

      .normal
        color: colors.color("white")

      @media (max-width: 480px)

  .hero-footer
    position: absolute
    bottom: 0
    right: 0
    width: 100%
    align-self: flex-end

    .down-link
      height: 60px
      width: 60px
      display: flex
      align-items: center
      justify-content: center
      position: absolute
      bottom: 0
      right: 0
      margin-right: 10%
      margin-bottom: 10%
      float: right
      pointer-events: all

      .down-link-inner
        width: 100%
        height: 100%

        &:is(:hover, :focus, :selected)
          color: colors.color(lightest-foreground)

        svg
          // animate up and down
          animation: up-down 1s ease-in-out infinite alternate
          margin: auto
          // animate change of color
          -webkit-transition: color 3s ease-in-out
          -moz-transition: color 3s ease-in-out
          -ms-transition: color 3s ease-in-out
          -o-transition: color 3s ease-in-out
          transition: color 3s ease-in-out

          &:is(:hover, :selected, :focus)
            stroke-width: 1px
            cursor: pointer
            transform: scale(1.5)
            transition: all 0.2s ease-out

          @keyframes up-down
            0%
              transform: translateY(0)
            50%
              transform: translateY(-5px)
            100%
              transform: translateY(0)

</style>
