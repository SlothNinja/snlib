<script>

  const logo = require('@/components/lib/assets/slothninja_logo_fullsize.png')
  const logo100 = require('@/components/lib/assets/slothninja_logo_100.png')

  export default {
    methods: {
      imagePath: function (name) {
        switch (name) {
          case 'logo':
            return logo
          case 'logo100':
            return logo100
        }
      }
    }
  }
</script>
