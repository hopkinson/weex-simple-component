<!-- iconfont 封装 -->
<style lang="css" scoped>
.cs-icon {
  font-size: 36;
  font-family:iconfont;
  color: #666;
}
</style>
<template>
<text @click="handleClick" :style="getStyle" class="cs-icon">{{getIcon}}</text>
</template>
<script>
/**
 * Name
 * icon-font的使用
 *
 * prop
 * value      iconfont的unicode
 * size       字体图标大小
 * color      字体图标颜色
 *
 * event
 * click      点击事件
 *
 */
const dom = weex.requireModule('dom')
const he = require('he')
export default {
    beforeCreate() {
        dom.addRule('fontFace', {
            'fontFamily': 'iconfont',
            'src': `url('//at.alicdn.com/t/font_622685_glpred4gztqbmx6r.ttf')`
        })
    },
    props: {
        value: {
            type: String,
            default: ''
        },
        color: {
            type: String,
            color: '#333'
        },
        size: {
            type: String,
            default: '48px'
        }
    },
    methods: {
        handleClick(e) {
            this.$emit('click', e)
        }
    },
    computed: {
        getIcon() {
            const {
                value
            } = this
            return he.decode(value)
        },
        getStyle() {
            return {
                fontSize: this.size,
                color: this.color
            }
        }
    }
}
</script>
