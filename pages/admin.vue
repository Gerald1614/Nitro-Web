<template>
  <div>
    <ds-heading tag="h1">
      {{ $t('admin.name') }}
    </ds-heading>
    <ds-flex gutter="small">
      <ds-flex-item :width="{ base: '200px' }">
        <ds-menu :routes="routes" />
      </ds-flex-item>
      <ds-flex-item>
        <transition
          name="slide-up"
          appear
        >
          <nuxt-child />
        </transition>
      </ds-flex-item>
    </ds-flex>
  </div>
</template>

<script>
export default {
  computed: {
    routes() {
      return [
        {
          name: this.$t('admin.dashboard.name'),
          path: `/admin`
        },
        {
          name: this.$t('admin.users.name'),
          path: `/admin/users`
        },
        {
          name: this.$t('admin.organizations.name'),
          path: `/admin/organizations`
        },
        {
          name: this.$t('admin.pages.name'),
          path: `/admin/pages`
        },
        {
          name: this.$t('admin.notifications.name'),
          path: `/admin/notifications`
        },
        {
          name: this.$t('admin.categories.name'),
          path: `/admin/categories`
        },
        {
          name: this.$t('admin.tags.name'),
          path: `/admin/tags`
        },
        {
          name: this.$t('admin.settings.name'),
          path: `/admin/settings`
        }
      ]
    }
  }
}
</script>
