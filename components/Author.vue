<template>
  <v-popover
    :open.sync="isPopoverOpen"
    :disabled="!hasAuthor || !showAuthorPopover"
    :open-group="Math.random().toString()"
    placement="top-start"
    trigger="manual"
    offset="0"
  >
    <a
      v-router-link
      :href="author.slug ? $router.resolve({ name: 'profile-slug', params: { slug: author.slug } }).href : null"
      style="white-space: nowrap; display: flex; align-items: center;"
      @mouseover="popoverMouseEnter"
      @mouseleave="popoveMouseLeave"
    >
      <div style="display: inline-block; float: left; margin-right: 4px;  height: 100%; vertical-align: middle;">
        <ds-avatar
          :image="author.avatar"
          :name="author.name"
          style="display: inline-block; vertical-align: middle;"
          size="32px"
        />
      </div>
      <div style="display: inline-block; height: 100%; vertical-align: middle;">
        <b
          class="username"
          style="vertical-align: middle;"
        >
          {{ author.name | truncate(trunc, 18) }}
        </b>
        <template v-if="post.createdAt">
          <br>
          <ds-text
            size="small"
            color="soft"
          >
            {{ post.createdAt | dateTime('dd. MMMM yyyy HH:mm') }}
          </ds-text>
        </template>
      </div>
    </a>
    <div
      slot="popover"
      style="min-width: 250px;"
      @mouseover="popoverMouseEnter"
      @mouseleave="popoveMouseLeave"
    >
      <!--<ds-avatar
        :image="author.avatar"
        :name="author.name || 'Anonymus'"
        class="profile-avatar"
        size="90px" />-->
      <hc-badges
        v-if="author.badges && author.badges.length"
        :badges="author.badges"
        style="margin-bottom: -10px"
      />
      <ds-flex>
        <ds-flex-item class="ds-tab-nav-item">
          <ds-space margin="small">
            <ds-number
              :count="fanCount"
              label="Folgen"
              size="x-large"
            />
          </ds-space>
        </ds-flex-item>
        <ds-flex-item class="ds-tab-nav-item ds-tab-nav-item-active">
          <ds-space margin="small">
            <ds-number
              :count="author.contributionsCount"
              label="Beiträge"
            />
          </ds-space>
        </ds-flex-item>
        <ds-flex-item class="ds-tab-nav-item">
          <ds-space margin="small">
            <ds-number
              :count="author.commentsCount"
              label="Kommentare"
            />
          </ds-space>
        </ds-flex-item>
      </ds-flex>
      <!--<ds-text
        color="soft"
        size="small">
        <ds-icon name="map-marker" /> Hamburg, Deutschland
      </ds-text>-->
      <ds-flex
        v-if="!itsMe"
        gutter="x-small"
        style="margin-bottom: 0;"
      >
        <ds-flex-item :width="{base: 3}">
          <hc-follow-button
            :follow-id="author.id"
            @update="voted = true"
          />
        </ds-flex-item>
        <ds-flex-item :width="{base: 1}">
          <ds-button full-width>
            <ds-icon name="user-times" />
          </ds-button>
        </ds-flex-item>
      </ds-flex>
      <!--<ds-space margin-bottom="x-small" />-->
    </div>
  </v-popover>
</template>

<script>
import HcFollowButton from '~/components/FollowButton.vue'
import HcBadges from '~/components/Badges.vue'

let mouseEnterTimer = null
let mouseLeaveTimer = null

export default {
  name: 'HcAuthor',
  components: {
    HcFollowButton,
    HcBadges
  },
  props: {
    post: { type: Object, default: null },
    trunc: { type: Number, default: null },
    showAuthorPopover: { type: Boolean, default: true }
  },
  data() {
    return {
      isPopoverOpen: false,
      voted: false
    }
  },
  computed: {
    itsMe() {
      return this.author.slug === this.$store.getters['auth/user'].slug
    },
    fanCount() {
      let count = Number(this.author.followedByCount) || 0
      if (this.voted) {
        // NOTE: this is used for presentation
        count += 1
      }
      return count
    },
    author() {
      return this.hasAuthor
        ? this.post.author
        : {
            name: 'Anonymus'
          }
    },
    hasAuthor() {
      return Boolean(this.post && this.post.author)
    }
  },
  beforeDestroy() {
    clearTimeout(mouseEnterTimer)
    clearTimeout(mouseLeaveTimer)
  },
  methods: {
    popoverMouseEnter() {
      clearTimeout(mouseEnterTimer)
      clearTimeout(mouseLeaveTimer)
      if (!this.isPopoverOpen) {
        mouseEnterTimer = setTimeout(() => {
          this.isPopoverOpen = true
        }, 500)
      }
    },
    popoveMouseLeave() {
      clearTimeout(mouseEnterTimer)
      clearTimeout(mouseLeaveTimer)
      if (this.isPopoverOpen) {
        mouseLeaveTimer = setTimeout(() => {
          this.isPopoverOpen = false
        }, 300)
      }
    }
  }
}
</script>

<style lang="scss">
.profile-avatar {
  display: block;
  margin: auto;
  margin-top: -45px;
  border: #fff 5px solid;
}
</style>
