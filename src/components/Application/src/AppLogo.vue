<!--
 * @Author: Vben
 * @Description: logo component
-->
<template>
  <div
    class="anticon"
    :class="[prefixCls, theme, { 'collapsed-show-title': getCollapsedShowTitle }]"
    @click="handleGoHome"
  >
    <img src="../../../assets/images/logo.png" />
    <div
      class="ml-2 truncate md:opacity-100"
      :class="[
        `${prefixCls}__title`,
        {
          'xs:opacity-0': !alwaysShowTitle,
        },
      ]"
      v-show="showTitle"
    >
      {{ title }}
    </div>
  </div>
</template>
<script lang="ts">
  import { defineComponent } from 'vue';

  import { useGlobSetting } from '/@/hooks/setting';
  import { useGo } from '/@/hooks/web/usePage';
  import { useMenuSetting } from '/@/hooks/setting/useMenuSetting';
  import { useDesign } from '/@/hooks/web/useDesign';

  import { PageEnum } from '/@/enums/pageEnum';
  import { propTypes } from '/@/utils/propTypes';

  export default defineComponent({
    name: 'AppLogo',
    props: {
      /**
       * The theme of the current parent component
       */
      theme: propTypes.oneOf(['light', 'dark']),
      // Whether to show title
      showTitle: propTypes.bool.def(true),
      alwaysShowTitle: propTypes.bool.def(false),
    },
    setup() {
      const { prefixCls } = useDesign('app-logo');
      const { getCollapsedShowTitle } = useMenuSetting();
      const { title } = useGlobSetting();
      const go = useGo();

      function handleGoHome(): void {
        go(PageEnum.BASE_HOME);
      }

      return {
        handleGoHome,
        title,
        prefixCls,
        getCollapsedShowTitle,
      };
    },
  });
</script>
<style lang="less" scoped>
  @prefix-cls: ~'@{namespace}-app-logo';

  .@{prefix-cls} {
    display: flex;
    align-items: center;
    padding-left: 7px;
    cursor: pointer;
    transition: all 0.2s ease;

    &.light {
      border-bottom: 1px solid @border-color-base;
    }

    &.collapsed-show-title {
      padding-left: 20px;
    }

    &.light &__title {
      color: @primary-color;
    }

    &.dark &__title {
      color: @white;
    }

    &__title {
      font-size: 16px;
      font-weight: 700;
      transition: all 0.5s;
    }
  }
</style>
