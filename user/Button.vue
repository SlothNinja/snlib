<template>
  <fragment>
    <v-btn 
      v-if="!bottom"
      v-bind="attrs"
      :color="color"
      :href="showlink"
      fab
      >
      <v-avatar :size="avatarSize" >
        <img :src="gravatar(user.emailHash, avatarSize, user.gravType)" />
      </v-avatar>
    </v-btn>
    <slot v-if="!bottom"></slot>
    <div v-if="bottom">
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
    <div v-if="bottom" class="mt-1">
    <slot></slot>
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
