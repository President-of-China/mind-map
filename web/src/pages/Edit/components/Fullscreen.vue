<template>
  <div class="fullscreenContainer" :class="{ isDark: isDark }">
    <el-tooltip
      class="item"
      effect="dark"
      :content="$t('fullscreen.fullscreenShow')"
      placement="top"
    >
      <div class="btn iconfont iconquanping" @click="toFullscreenShow"></div>
    </el-tooltip>
    <el-tooltip
      class="item"
      effect="dark"
      :content="$t('fullscreen.fullscreenEdit')"
      placement="top"
      v-if="!isReadonly"
    >
      <div class="btn iconfont iconquanping1" @click="toFullscreenEdit"></div>
    </el-tooltip>
  </div>
</template>

<script>
import { fullscrrenEvent, fullScreen } from '@/utils'
import { mapState, mapMutations } from 'vuex'

// 全屏
export default {
  props: {
    mindMap: {
      type: Object
    },
    isDark: {
      type: Boolean
    }
  },
  data() {
    return {
      isSetReadOnly: false
    }
  },
  computed: {
    ...mapState({
      isReadonly: state => state.isReadonly
    })
  },
  created() {
    document[fullscrrenEvent] = () => {
      if (!document.fullscreenElement && this.isSetReadOnly) {
        this.isSetReadOnly = false
        this.setIsReadonly(false)
        this.mindMap.setMode('edit')
      }
      setTimeout(() => {
        this.mindMap.resize()
      }, 1000)
    }
  },
  methods: {
    ...mapMutations(['setIsReadonly']),

    // 全屏查看
    toFullscreenShow() {
      // 如果当前是非只读，那么先切换成只读模式
      if (!this.isReadonly) {
        this.isSetReadOnly = true
        this.setIsReadonly(true)
        this.mindMap.setMode('readonly')
      }
      fullScreen(this.mindMap.el)
    },

    // 全屏编辑
    toFullscreenEdit() {
      fullScreen(document.body)
    }
  }
}
</script>

<style lang="less" scoped>
.fullscreenContainer {
  display: flex;
  align-items: center;

  &.isDark {
    .btn {
      color: hsla(0, 0%, 100%, 0.6);
    }
  }

  .item {
    margin-right: 12px;

    &:last-of-type {
      margin-right: 0;
    }
  }

  .btn {
    cursor: pointer;
  }
}
</style>
