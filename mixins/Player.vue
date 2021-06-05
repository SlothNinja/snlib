<script>

  import CurrentUser from '@/components/lib/mixins/CurrentUser'

  const _ = require('lodash')

  export default {
    mixins: [ CurrentUser ],
    computed: {
      cp: function () {
        let pid = this.pidByUID(this.cuid)
        if (_.includes(this.game.cpids, pid)) {
          return this.playerByPID(pid)
        }
        return _.first(this.cps)
      },
      cps: function () {
        let self = this
        return _.map(self.game.cpids, function (cpid) {
          return self.playerByPID(cpid)
        })
      },
      cpid: function () {
        return _.get(this.game.cpids, 0, -1)
      },
      isCP: function () {
        return this.isPlayerFor(this.cp, this.$root.cu)
      },
      isCPS: function () {
        let self = this
        return _.some(self.cps, function (p) {
          return self.isPlayerFor(p, self.$root.cu)
        })
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
        return _.find(this.game.players, ['id', pid])
      },
      playersByPIDS: function (pids) {
        return _.map(pids, function (pid) {
          return this.playerByPID(pid)
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
        let pid = this.uidFor(player)
        let uid = _.get(user, 'id', -2)
        return pid === uid
      },
      uidIndex: function (uid) {
        return _.indexOf(this.game.userIds, uid)
      },
      nameFor: function (p) {
        if (p) {
          let index = p.id - 1
          return this.game.userNames[index]
        }
        return ""
      },
      uidFor: function (p) {
        if (p) {
          let index = p.id - 1
          return this.game.userIds[index]
        }
        return -1
      },
      emailHashFor: function (p) {
        if (p) {
          let index = p.id - 1
          return this.game.userEmailHashes[index]
        }
        return -1
      },
      gravTypeFor: function (p) {
        if (p) {
          let index = p.id - 1
          return this.game.userGravTypes[index]
        }
        return -1
      },
      userFor: function (p) {
        return {
          id: this.uidFor(p),
          name: this.nameFor(p),
          emailHash: this.emailHashFor(p),
          gravType: this.gravTypeFor(p)
        }
      },
    }
  }
</script>
