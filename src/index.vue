<template>
  <div :class="[mode]" class="component">
    <img v-if="cUrl" :src="cUrl" />
  </div>
</template>

<script>
  import {VueExtend} from 'godspen-lib'

  export default {
    mixins: [VueExtend.mixin],
    name: 'image',
    label: process.env.LABEL,
    style: process.env.STYLE,
    props: {
      url: {
        type: String,
        editer: {
          type: 'image',
          label: '图片',
          desc: '图片地址信息'
        },
        default: 'https://ymmtest.oss-cn-hangzhou.aliyuncs.com/ymmfile/explore-biz/ymm_1528188057605.png'
      },
      compress: {
        type: Boolean,
        default: true,
        editer: {
          type: 'checkbox',
          desc: '按元素实际大小动态访问对应尺寸的图片（只支持阿里oss图片资源），如果关闭会加载原图',
          label: '图片尺寸优化 '
        }
      },
      mode: {
        type: String,
        default: 'widthFix',
        editer: {
          type: 'enum',
          label: '填充模式',
          defaultList: {
            scaleToFill: '拉伸',
            aspectFit: '适应',
            widthFix: '固定宽度',
            heightFix: '固定高度'
          }
        }
      }
    },
    computed: {
      cUrl () {
        var url = this.scopeGet('url')
        if (!this.compress) return url
        var viewMeta = this.$attrs['view-meta'] || {}
        var width = /\d+px$/.test(viewMeta.width) ? parseFloat(viewMeta.width) * 2 | 0 : 750
        var height = /\d+px$/.test(viewMeta.height) ? parseFloat(viewMeta.height) * 2 | 0 : 750
        var resize = this.mode == 'heightFix' ? `h_${height}` : `w_${width}`
        url = url.replace(/\.(?:png|jpe?g)$/, `$&?x-oss-process=image/resize,${resize}`)
        return url
      }
    },
    editorMethods: {
    },
    methods: {
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component {
    vertical-align: bottom;
    display: block;
    width: 100%;
    height: 100%;

    > img {
      vertical-align: top;
    }

    &.scaleToFill {
      > img {
        width: 100%;
        height: 100%;
      }
    }

    &.aspectFit {
      > img {
        max-width: 100%;
        max-height: 100%;
      }
    }

    &.widthFix {
      overflow: hidden;

      > img {
        width: 100%;
      }
    }

    &.heightFix {
      overflow: hidden;

      > img {
        height: 100%;
      }
    }
  }
</style>
