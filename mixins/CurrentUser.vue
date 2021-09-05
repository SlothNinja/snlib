<script>
  const _ = require('lodash')

  export default {
    methods: {
      isCu: function (user) {
        let uid = _.get(user, 'id', -1)
        return this.cuid ? this.cuid == uid : false
      },
      isCuOrAdmin: function (user) {
        return this.isCu(user) || this.isAdmin(user)
      },
      isAdmin: function (user) {
        return _.get(user, "admin", false)
      }
    },
    computed: {
      cu: {
        get: function () {
          return this.$root.cu
        },
        set: function (value) {
          this.$root.cu = value
        }
      },
      cuid: function () {
        return _.get(this.cu, 'id', false)
      },
      cuLoading: {
        get: function () {
          return this.$root.cuLoading
        },
        set: function (value) {
          this.$root.cuLoading = value
        }
      }
    }
  }
</script>
