<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
    uploader: {
      type: Object,
      required: true,
    },
  },

  data: () => ({
    filename: '',
  }),

  created() {
    this.filename = this.uploader.methods.getName(this.id)
    this.interceptSetName()
  },

  methods: {
    interceptSetName() {
      const oldSetName = this.uploader.methods.setName

      this.uploader.methods.setName = (id, newName) => {
        oldSetName.call(this.uploader.methods, id, newName)

        if (id === this.id) {
          this.filename = newName
        }
      }
    },
  },

  render() {
    return this.$scopedSlots.default({
      filename: this.filename,
    })
  },
}
</script>
