<template>
  <div class="top_bar_right">
    <SfButton class="topbar__button sf-button--text margin-right" @click="isOpen = true">
      <div>{{ $t('Rentefri finansiering') }}</div>
    </SfButton>
    <SfModal
      v-model="isOpen"
      class="max-w-[90%] md:max-w-lg"
      tag="section"
      role="alertdialog"
      aria-labelledby="promoModalTitle"
      aria-describedby="promoModalDesc"
    >
      <header class="modal-header">
        <div class="flex flex-col rating">
          <SfRating size="2xl" class="star-rating" :score="5"/>
        </div>
        <h3 id="promoModalTitle" class="typography-headline-4 md:typography-headline-3">
          {{ $t('KUNDERNE ELSKER OS') }}
        </h3>
      </header>
      <div class="modal-content">
        <p id="promoModalDesc" class="mt-2">
          {{ $t('There are special offers for some of the items on your wishlist. Do you want to see these deals before proceeding to checkout page?') }}
        </p>
      </div>
      <footer class="footer-modal">
        <SfButton @click="isOpen = false" class="button-close bg-black">Okay</SfButton>
      </footer>
    </SfModal>

    <SfButton class="topbar__button sf-button--text">
      <div>{{ $t('Om Sengetid') }}</div>
    </SfButton>
  </div>
</template>

<script lang="ts">
import { ref} from '@nuxtjs/composition-api';
import {SfButton, SfModal} from "@storefront-ui/vue";
export default {
  name: 'TopBarRight',
  components: {
    SfButton,
    SfModal,
  },
  setup() {
    const isOpen = ref(false);

    return {
      isOpen,
    };
  },
};
</script>

<style scoped lang="scss">
.top_bar_right {
  display: flex;
  .sf-button--text {
    display: flex;
    font-size: 10px;
    text-decoration: unset;
  }
  .margin-right {
    margin-right: 40px;
  }
}
</style>
