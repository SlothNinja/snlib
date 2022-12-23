<script>

  import CurrentUser from '@/components/lib/mixins/CurrentUser'

  const _ = require('lodash')

  export default {
    mixins: [ CurrentUser ],
    computed: {
      cp: function () {
        return this.playerByPID(this.cpid)
      },
      cps: function () {
        let self = this
        return _.map(self.game.cpids, function (cpid) {
          return self.playerByPID(cpid)
        })
      },
      cpid: function () {
        if (_.isNull(this.cpids)) {
          return null
        }

        let pid = this.pidByUID(this.cuid)
        return _.includes(this.game.cpids, pid) ? pid : _.first(this.cpids)
      },
      cpids: function () {
        return _.get(this.game, 'cpids', null)
      },
      isCP: function () {
        return this.isPlayerFor(this.cp, this.cu)
      },
      isCPS: function () {
        return _.some(this.cps, p => this.isPlayerFor(p, this.$root.cu))
      },
      isCPorAdmin: function () {
        return (this.$root.cu && this.$root.cu.admin) || this.isCP
      }
    },
    methods: {
      indexOf: function (player) {
        return _.findIndex(this.game.players, ['id', player.id])
      },
      cpIs: function (player) {
        var pid = _.get(player, 'id', -2)
        return this.cpid == pid
      },
      playerByPID: function (pid) {
        let found = _.find(this.game.players, ['id', pid])
        return _.isUndefined(found) ? null : found
      },
      playersByPIDS: function (pids) {
        let self = this
        return _.map(pids, function (pid) {
          return self.playerByPID(pid)
        })
      },
      playerByUser: function (u) {
        let uid = _.get(u, 'id', false)
        if (uid) {
          return this.playerByUID(uid)
        }
        return {}
      },
      playerByUID: function (uid) {
        let index = this.uidIndex(uid)
        if (index == -1) {
          return {}
        }
        let pid = index + 1
        return _.find(this.game.players, [ 'id', pid ])
      },
      pidByUID: function (uid) {
        let player = this.playerByUID(uid)
        if (player == {}) {
          return -1
        }
        return player.id
      },
      isPlayerFor: function (player, user) {
        let pid = _.get(player, 'id', -1)
        if (pid == -1) {
          return false
        }
        let uid1 = this.uidFor(player.id)
        let uid2 = _.get(user, 'id', -2)
        return uid1 === uid2
      },
      uidIndex: function (uid) {
        return _.indexOf(this.game.userIds, uid)
      },
      nameFor: function (pid) {
        let index = pid - 1
        return this.game.userNames[index]
      },
      uidFor: function (pid) {
        let index = pid - 1
        return this.game.userIds[index]
      },
      emailHashFor: function (pid) {
        let index = pid - 1
        return this.game.userEmailHashes[index]
      },
      gravTypeFor: function (pid) {
        let index = pid - 1
        return this.game.userGravTypes[index]
      },
      userFor: function (pid) {
        return {
          id: this.uidFor(pid),
          name: this.nameFor(pid),
          emailHash: this.emailHashFor(pid),
          gravType: this.gravTypeFor(pid)
        }
      },
    }
  }
</script>
