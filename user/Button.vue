<template>
  <v-row
    no-gutters
    align="center"
    >
    <v-col>
      <v-btn 
        v-bind="$attrs"
        :color="color"
        :href="showlink"
        fab
        >
        <v-avatar :size="avatarSize" >
          <img :src="gravatar(user.emailHash, avatarSize, user.gravType)" />
        </v-avatar>
      </v-btn>
      <span class="ml-1"><slot></slot></span>
    </v-col>
  </v-row>
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
    avatarSize: function () {
      if (_.has(this.$attrs, 'x-small')) {
        return 24
      }
      if (_.has(this.$attrs, 'small')) {
        return 30
      }
      if (_.has(this.$attrs, 'medium')) {
        return 48
      }
      return 54
    },
    showlink: function () {
      var self = this
      let userv = process.env.VUE_APP_USER_DOMAIN
      return `${userv}#/show/${self.user.id}`
    }
  }
}
</script>
