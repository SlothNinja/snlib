<template>
  <fragment>
    <div
      v-if="!bottom"
      class="d-flex"
      >
      <div class="d-flex align-center">
        <v-btn 
          v-bind="attrs"
          :color="color"
          :href="showlink"
          fab
          >
          <v-avatar :size="avatarSize" class="d-line-flex">
            <img :src="gravatar(user.emailHash, avatarSize, user.gravType)" />
          </v-avatar>
        </v-btn>
      </div>
      <div class="d-flex align-center mx-1">
        <slot></slot>
      </div>
    </div>

    <div
      v-if="bottom"
      class="d-flex flex-column"
      >
      <div class="d-flex justify-center">
        <v-btn 
          v-bind="attrs"
          :color="color"
          :href="showlink"
          fab
          >
          <v-avatar :size="avatarSize" >
            <img :src="gravatar(user.emailHash, avatarSize, user.gravType)" />
          </v-avatar>
        </v-btn>
      </div>
      <div class="d-flex justify-center">
        <slot></slot>
      </div>
    </div>
  </fragment>
</template>

<script>
import Gravatar from "@/components/lib/mixins/Gravatar"

const _ = require("lodash")

export default {
  mixins: [ Gravatar ],
  name: "sn-user-btn",
  props: {
    color: {
      type: String,
      default: "black"
    },
    user: {
      type: Object,
      required: true
    }
  },
  computed: {
    direction: function () {
      if (this.bottom) {
        return "flex-column"
      }
      return "flex-row"
    },
    margin: function () {
      if (this.bottom) {
        return ""
      }
      return "ml-1"
    },
    attrs: function () {
      let obj = this.$attrs
      if (_.has(this.$attrs, 'xx-small')) {
        obj.height = 18
        obj.width = 18
      }
      return obj
    },
    avatarSize: function () {
      if (_.has(this.$attrs, 'xx-small')) {
        return 14
      }
      if (_.has(this.$attrs, 'x-small')) {
        return 26
      }
      if (_.has(this.$attrs, 'small')) {
        return 30
      }
      if (_.has(this.$attrs, 'medium')) {
        return 48
      }
      return 54
    },
    bottom: function () {
      if (_.has(this.$attrs, 'bottom')) {
        return true
      }
      return false
    },
    showlink: function () {
      var self = this
      let userv = process.env.VUE_APP_USER_DOMAIN
      return `${userv}#/show/${self.user.id}`
    }
  }
}
</script>
