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
    },
    data () {
      return {
        scaleX: 1,
        lastScaleX: 1,
        x: 0,
        y: 0,
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
        this.x = x;

        let y = this.y + evt.deltaY;
        if ( y > maxY ) {
          y = maxY;
        } else if ( y < minY ) {
          y = minY;
        }
        this.y = y;
        
      },
      pinchStart: function () {
        this.lastScaleX = this.scaleX;
      },
      pinch: function(evt) {
        evt.preventDefault();
        
        let scale = this.lastScaleX * evt.zoom;
        if ( scale > this.maxScale ) {
          scale = this.maxScale
        } else if ( scale < this.minScale ) {
          scale = this.minScale;
        }
        this.scaleX = scale;
      },
    },
    computed: {
      style() {
        return {
          transform: `translate3d(${this.x}px, ${this.y}px, 0) scale(${this.scaleX})`,
          '-webkit-transform': `translate3d(${this.x}px, ${this.y}px, 0) scale(${this.scaleX})`
        };
      },
    }
  }
</script>
