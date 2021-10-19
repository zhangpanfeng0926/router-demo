<template>
  <div class="tab-bar-item" @click="itemClick">
    <slot v-if="!isSelected" name="item-img"></slot>
    <slot v-else name="item-img-selected"></slot>
    <!--<div :class="{selected:isSelected}">-->
      <div :style="selectedStyle">
      <slot name="item-title"></slot>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";
import { useRouter, useRoute } from "vue-router";

export default {
  name: "TabItem",
  props: {
    path: String,
    //如果不传值也会默认为默认颜色
    textColor: {
      type: String,
      default: "red",
    },
  },
  setup(props) {
    //setup获取参数传值
    const router = useRouter(); //获取整个路由对象
    const itemClick = () => {
      router.replace(props.path);
    };
    //设置当前选中项
    const route = useRoute(); //当前路由对象
    const isSelected = computed(() => {
      return route.path.includes(props.path);
    });
    //处理选中文字的颜色
    const selectedStyle = computed(() => {
      return isSelected.value ? { color: props.textColor } : {}; //注意computed属性的值获取使用value
    });

    return {
      itemClick,
      isSelected,
      selectedStyle,
    };
  },
};
</script>

<style>
.tab-bar .tab-bar-item {
  flex: 1; /*均匀分布 */
  height: 49px; /*移动端的下部推荐设计高度 */
  display: flex;
  flex-direction: column; /*纵向布局 */
  justify-items: center; /*主轴居中 */
  align-items: center; /*侧轴居中 */
}
/*图标大小 */
.tab-bar-item img {
  width: 24px;
  height: 24px;
}
.selected{
  color:red;
}
</style>