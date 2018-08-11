<template>
  <div
    :class="classNames"
    v-finger:pinch="pinch"
    v-finger:touch-start="pinchStart"
    v-finger:press-move="pressMove.bind(this, 12)"
    :style="style"
  >
    <slot />
  </div>
</template>

<script>
import Vue from 'vue'
import AlloyFinger from 'alloyfinger';

const AlloyFingerVue = require('alloyfinger/vue/alloy_finger.vue');
Vue.use(AlloyFingerVue, { AlloyFinger });

  export default {
    name: 'ui-drag',
    props: {
      classNames: {
        type: Object,
        default: () => ({}),
      },
      maxScale: {
        type: Number,
        default: 3,
      },
      minScale: {
        type: Number,
        default: 0.5,
      },
      maxX: {
        type: Number,
        required: true,
        default: 0,
      },
      minX: {
        type: Number,
        required: true,
        default: 0,
      },
      maxY: {
        type: Number,
        required: true,
        default: 0,
      },
      minY: {
        type: Number,
        required: true,
        default: 0,
      },
      x: {
        type: Number,
        default: 0,
      },
      y: {
        type: Number,
        default: 0,
      },
      scale: {
        type: Number,
        default: 1,
      },
    },
    data () {
      return {
        lastScale: this.scale,
      }
    },
    methods: {
      pressMove: function(num, evt) {
        evt.preventDefault();
        const { maxX, minX, minY, maxY } = this;
        
        let x = this.x + evt.deltaX;
        if ( x > maxX ) {
          x = maxX;
        } else if ( x < minX ) {
          x = minX;
        }
        this.$emit('update:x', x);

        let y = this.y + evt.deltaY;
        if ( y > maxY ) {
          y = maxY;
        } else if ( y < minY ) {
          y = minY;
        }
        this.$emit('update:y', y);
        
      },
      pinchStart: function () {
        this.lastScale = this.scale;
      },
      pinch: function(evt) {
        evt.preventDefault();
        
        let scale = this.lastScale * evt.zoom;
        if ( scale > this.maxScale ) {
          scale = this.maxScale
        } else if ( scale < this.minScale ) {
          scale = this.minScale;
        }
        this.$emit('update:scale', scale);
      },
    },
    computed: {
      style() {
        return {
          transform: `translate3d(${this.x}px, ${this.y}px, 0) scale(${this.scale})`,
          '-webkit-transform': `translate3d(${this.x}px, ${this.y}px, 0) scale(${this.scale})`
        };
      },
    }
  }
</script>
