<template>
</template>
<script>
import { toLngLat } from '../utils/convert-helper';
import registerMixin from '../mixins/register-component';
import { compile } from '../utils/compile';
export default {
  name: 'el-amap-info-window',
  mixins: [registerMixin],
  props: [
    'vid',
    'isCustom',
    'autoMove',
    'closeWhenClickMap',
    'content',
    'size',
    'offset',
    'position',
    'showShadow',
    'visible',
    'events',
    'template'
  ],
  data() {
    let self = this;
    return {
      propsRedirect: {
        template: 'content'
      },
      converters: {
        template(tpl) {
          let node = compile(tpl, self);
          return node;
        }
      },
      handlers: {
        visible(flag) {
          // fixed Amap info-window reopen
          let position = this.getPosition();
          if (position) {
            flag === false ? this.close() : this.open(self.$amap, [position.lng, position.lat]);
          }
        },
        template(node) {
          this.setContent(node);
        }
      }
    };
  },
  destroyed() {
    this.$amapComponent.close();
  },
  methods: {
    initComponent(options) {
      this.$amapComponent = new AMap.InfoWindow(options);
      if (this.visible !== false) this.$amapComponent.open(this.$amap, toLngLat(this.position));
    }
  }
};
</script>
