<!-- *************************************************************************
     TEMPLATE
     ************************************************************************* -->

<template lang="pug">
article(
  :class=`[
    "c-vue-timeline-update",
    "js-vue-timeline-update",
    {
      "c-vue-timeline-update--is-last": isLast
    }
  ]`
)
  .c-vue-timeline-update__left
    span.c-vue-timeline-update__ago {{ ago }}

  .c-vue-timeline-update__center
    base-number(
      :color="color"
      :icon="icon"
      class="c-vue-timeline-update__bullet"
      icon-size="16px"
      size="small"
    )

    span.c-vue-timeline-update__line

  div(
    @click="onContentClick"
    :class=`[
      "c-vue-timeline-update__right",
      {
        "c-vue-timeline-update__right--clickable": $listeners.click
      }
    ]`
  )
    .c-vue-timeline-update__information
      .c-vue-timeline-update__meta
        base-badge(
          v-if="category"
          :color="color"
          :filled="true"
          class="c-vue-timeline-update__category"
          size="small"
        ) {{ category }}

        span.c-vue-timeline-update__ago {{ ago }}

      h2(
        v-html="title"
        @click="onTitleClick"
        :class=`[
          "c-vue-timeline-update__title",
          {
            "c-vue-timeline-update__title--clickable": $listeners["click:title"]
          }
        ]`
      )

    img(
      v-if="thumbnail"
      @click="onThumbnailClick"
      :class=`[
        "c-vue-timeline-update__thumbnail",
        {
          "c-vue-timeline-update__thumbnail--clickable": $listeners["click:thumbnail"]
        }
      ]`
      :src="thumbnail"
    )

    p(
      v-html="description"
      class="c-vue-timeline-update__description"
    )

    div(
      v-if="$slots.default"
      class="c-vue-timeline-update__slot"
    )
</template>

<!-- *************************************************************************
     SCRIPT
     ************************************************************************* -->

<script>
// NPM
import { BaseBadge, BaseNumber } from "@growthbunker/vuedarkmode"
import { format } from "timeago.js"

export default {
  components: {
    BaseBadge,
    BaseNumber
  },

  props: {
    animation: {
      type: Boolean,
      default: true
    },
    animationContainer: {
      type: String,
      default: null
    },
    animationDuration: {
      type: Number,
      default: 1500
    },
    category: {
      type: String,
      default: null
    },
    color: {
      type: String,
      default: "blue",
      validator(x) {
        return [
          "black",
          "blue",
          "green",
          "orange",
          "purple",
          "red",
          "turquoise",
          "white"
        ].includes(x)
      }
    },
    date: {
      type: Date,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    icon: {
      type: String,
      required: true
    },
    isLast: {
      type: Boolean,
      default: false
    },
    thumbnail: {
      type: String,
      default: null
    },
    title: {
      type: String,
      required: true
    }
  },

  computed: {
    ago() {
      return format(this.date)
    }
  },

  mounted() {
    if (this.animation) {
      const ScrollReveal = require("scrollreveal").default

      ScrollReveal().reveal(this.$el, {
        container: this.animationContainer,
        duration: this.animationDuration
      })
    }
  },

  methods: {
    // --> EVENT LISTENERS <--

    onContentClick(event) {
      this.$emit("click", event)
    },

    onThumbnailClick(event) {
      this.$emit("click:thumbnail", event)
    },

    onTitleClick(event) {
      this.$emit("click:title", event)
    }
  }
}
</script>

<!-- *************************************************************************
     STYLE
     ************************************************************************* -->

<style lang="scss">
// IMPORTS
@import "node_modules/@growthbunker/stylesheets/settings/_colors.scss";
@import "node_modules/@growthbunker/stylesheets/tools/_mq.scss";

// VARIABLES
$c: ".c-vue-timeline-update";

#{$c} {
  display: flex;
  overflow: hidden;
  color: $white;

  a {
    color: $white;
    text-decoration: underline;
  }

  #{$c}__left,
  #{$c}__right  {
    #{$c}__ago {
      color: $regent-st-blue;
      font-size: 14px;
      user-select: none;
    }
  }

  #{$c}__left {
    display: none;
  }

  #{$c}__center {
    position: relative;
    flex: 0 0 auto;
    margin-right: 30px;
    margin-left: 16px;

    #{$c}__bullet {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
    }

    #{$c}__line {
      display: inline-block;
      margin-top: 32px;
      width: 1px;
      height: 100%;
      background-color: $oxford-blue;
    }
  }

  #{$c}__right {
    flex: 1;
    padding-bottom: 40px;

    #{$c}__title,
    #{$c}__description {
      font-size: 16px;
      line-height: 26px;
    }

    #{$c}__information {
      display: flex;
      flex-direction: column;
      margin-top: 4px;

      #{$c}__meta {
        display: flex;
        align-items: center;
        margin-bottom: 10px;

        #{$c}__category {
          align-self: flex-start;
          flex: 0 0 auto;
          margin-right: 10px;
        }

        #{$c}__ago {
          line-height: 24px; // Size of the category to align horizontally
        }
      }

      #{$c}__title {
        flex: 1;
        margin: 0 0 4px;
        text-transform: uppercase;
        font-weight: bold;

        &--clickable {
          cursor: pointer;
        }
      }
    }

    #{$c}__description {
      margin: 0;
    }

    #{$c}__thumbnail {
      box-sizing: border-box;
      margin: 6px 0 12px;
      max-width: 100%;
      border: 1px solid $regent-st-blue;
      border-radius: 6px;
      box-shadow: 0 1px 5px 0 rgba($woodsmoke, 0.6);
      transition: all 250ms linear;
      user-select: none;

      &--clickable {
        cursor: pointer;
      }
    }

    #{$c}__description {
      color: $regent-st-blue;
    }

    #{$c}__slot {
      margin-top: 20px;
    }

    &--clickable {
      cursor: pointer;
    }
  }

  // --> BOOLEANS <--

  &--is-last {
    #{$c}__center {
      #{$c}__line {
        background: linear-gradient($oxford-blue 50%, rgba($oxford-blue, 0));
      }
    }

    #{$c}__right {
      padding-bottom: 20px;
    }
  }
}

// --> BREAKPOINT: TABLET <--

@include mq($from: tablet) {
  #{$c} {
    #{$c}__left {
      display: block;
      flex: 0 0 auto;
      width: 120px;
      text-align: right;

      #{$c}__ago {
        display: inline-block;
        line-height: 32px; // Size of the badge to align horizontally
      }
    }

    #{$c}__center {
      margin-right: 40px;
      margin-left: 40px;
    }

    #{$c}__right {
      #{$c}__title,
      #{$c}__description {
        font-size: 18px;
        line-height: 28px;
      }

      #{$c}__information {
        flex-direction: row;

        #{$c}__meta {
          #{$c}__ago {
            display: none;
          }
        }

        #{$c}__title {
          margin-bottom: 8px;
        }
      }

      #{$c}__thumbnail {
        margin: 8px 0 16px;
      }
    }
  }
}
</style>
