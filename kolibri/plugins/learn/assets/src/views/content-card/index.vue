<template>

  <router-link :to="link" class="card" :class="{ 'mobile-card': isMobile }">
    <card-thumbnail
      class="thumbnail"
      v-bind="{ thumbnail, progress, kind, isMobile, showContentIcon }"
    />

    <div
      class="text"
      dir="auto"
    >
      <h3 class="title">
        <shaved-text
          :title="title"
          :maxHeight="maxTitleHeight"
        />
      </h3>
      <p
        v-if="subtitle"
        class="subtitle"
      >
        {{ subtitle }}
      </p>
      <coach-content-label
        class="coach-content-label"
        :value="numCoachContents"
      />
    </div>
  </router-link>

</template>


<script>

  import { validateLinkObject, validateContentNodeKind } from 'kolibri.utils.validators';
  import coachContentLabel from 'kolibri.coreVue.components.coachContentLabel';
  import cardThumbnail from './card-thumbnail';
  import shavedText from './shaved-text';

  export default {
    components: {
      cardThumbnail,
      coachContentLabel,
      shavedText,
    },
    props: {
      title: {
        type: String,
        required: true,
      },
      subtitle: {
        type: String,
        required: false,
      },
      thumbnail: {
        type: String,
        required: false,
      },
      kind: {
        type: String,
        required: true,
        validator: validateContentNodeKind,
      },
      showContentIcon: {
        type: Boolean,
        default: true,
      },
      // ContentNode.coach_content will be `0` if not a coach content leaf node,
      // or a topic without coach content. It will be a positive integer if a topic
      // with coach content, and `1` if a coach content leaf node.
      numCoachContents: {
        type: Number,
        default: 0,
      },
      progress: {
        type: Number,
        required: false,
        default: 0.0,
        validator(value) {
          return value >= 0.0 && value <= 1.0;
        },
      },
      link: {
        type: Object,
        required: true,
        validator: validateLinkObject,
      },
      isMobile: {
        type: Boolean,
        default: false,
      },
    },
    computed: {
      maxTitleHeight() {
        if (this.subtitle) {
          return this.isMobile ? 20 : 40;
        }
        return this.isMobile ? 40 : 60;
      },
    },
  };

</script>


<style lang="stylus" scoped>

  @require '~kolibri.styles.definitions'
  @require './card.styl'

  .coach-content-label
    padding: 8px 0

  .card
    text-decoration: none
    display: inline-block
    width: $thumb-width-desktop
    border-radius: 2px
    background-color: $core-bg-light
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14),
                0 3px 1px -2px rgba(0, 0, 0, 0.2),
                0 1px 5px 0 rgba(0, 0, 0, 0.12)
    transition: box-shadow 0.4s cubic-bezier(0.25, 0.8, 0.25, 1)
    &:hover, &:focus
      box-shadow: 0 8px 10px 1px rgba(0, 0, 0, 0.14),
                  0 3px 14px 2px rgba(0, 0, 0, 0.12),
                  0 5px 5px -3px rgba(0, 0, 0, 0.2)

  .text
    color: $core-text-default
    overflow: hidden
    padding: 16px
    height: 92px
    position: relative

  .title, .subtitle
    margin: 0

  .subtitle
    position: absolute
    bottom: 12px
    left: 16px
    right: 16px
    font-size: 14px
    white-space: nowrap
    overflow: hidden
    text-overflow: ellipsis

  .mobile-card.card
    width: 100%
    height: $thumb-height-mobile

  .mobile-card
    .thumbnail
      position: absolute
    .text
      margin-left: $thumb-width-mobile + 16

</style>
