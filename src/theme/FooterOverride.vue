<template>
  <footer class="sf-footer">
    <div
      class="sf-footer__container"
      :style="{ 'grid-template-columns': ' 1fr'.repeat(column) }"
    >
      <slot />
    </div>
    <div class="sf-footer__container copyrights">
      <div class="copyright-item">
        <p>©2024 Sengetid ApS | Industrigrenen 15, 2635 Ishøj (Kontoradresse) | CVR: 38311697 | TLF: <a href="tel:70605553">70 60 55 53</a> | <a href="mailto:hej@sengetid.dk">hej@sengetid.dk</a></p>
      </div>
      <div class="copyright-item">
        <img src="@/static/kortikoner.png" alt="">
      </div>
    </div>
  </footer>
</template>
<script>
import Vue from "vue";
import SfFooterColumn from "@storefront-ui/vue";
Vue.component("SfFooterColumn", SfFooterColumn);
export default {
  name: "FooterOverride",
  props: {
    column: {
      type: Number,
      default: 4,
    },
    multiple: {
      type: Boolean,
      default: true,
    },
    open: {
      type: [String, Array],
      default: () => [],
    },
  },
  provide() {
    return {
      items: this.items,
    };
  },
  data() {
    return {
      isOpen: this.open,
      items: [],
    };
  },
  methods: {
    toggle(payload) {
      if (!this.multiple) {
        this.isOpen = [payload];
      } else if (this.isOpen.includes(payload)) {
        this.isOpen = this.isOpen.filter((item) => item !== payload);
      } else {
        this.isOpen.push(payload);
      }
      this.$emit("change", this.isOpen);
    },
  },
};
</script>
<style lang="scss">
@import "~@storefront-ui/shared/styles/components/organisms/SfFooter.scss";
</style>
