<script>
/* eslint-disable */
export default {
  name: 'TargetLock',
  props: ['targetElem'],
  data() {
    return {
      position: {
        x: null,
        y: null,
      }
    };
  },
  watch: {
    targetElem: {
      handler: 'bindEvents',
      immediate: true
    }
  },
  created() {
    console.log('CREATED');
  },
  destroyed() {
    console.log('DESTROYED');
    document.body.removeEventListener("mousemove", this.onMouseMove);
  },
  methods: {
    onMouseMove(e) {
      console.log('onMouseMove')
      const {x, y} = this.targetElem.getBoundingClientRect();
      this.position.x = x;
      this.position.y = y;
    },
    bindEvents() {
      console.log('bindEvents')
      document.body.removeEventListener("mousemove", this.onMouseMove);
      if (this.targetElem) {
        document.body.addEventListener("mousemove", this.onMouseMove);
      }
    }
  },
  computed: {
    center() {
      const {x, y, width} = this.targetElem.getBoundingClientRect();
      return x + (width / 2);
    }
  },
  render() {
    return this.$scopedSlots.default({
      position: this.position,
      center: this.center,
    });
  }
}
</script>
