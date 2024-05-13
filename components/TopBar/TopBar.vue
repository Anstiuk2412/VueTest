<template>
  <SfTopBar class="topbar">
    <template #left>
      <SfButton class="sf-button--text">
        <img src="@/static/icons/flag.svg" alt="" class="topbar-icons">
        <div>{{ $t('Dansk design') }}</div>
      </SfButton>

      <SfButton class="sf-button--text">
        <img src="@/static/icons/shipping.svg" alt="" class="topbar-icons">
        <div>{{ $t('Gratis fragt') }}</div>
      </SfButton>

      <SfButton class="sf-button--text">
        <img src="@/static/icons/101-night.svg" alt="" class="topbar-icons">
        <div>{{ $t('101 nætters risikofri prøve') }}</div>
      </SfButton>
    </template>

    <template #center>
      <SfButton class="topbar__button sf-button--text ml" @click="isOpen = true">
        <div class="flex flex-col rating">
          <SfRating size="lg" class="star-rating" :score="5"/>
        </div>
        <div>{{ $t('Kunderne elsker os') }}</div>
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
    </template>
    <template #right>
<!--      <CurrencySelector v-if="hasCurrencyToSelect" />
      <StoreSwitcher v-if="hasStoresToSelect" />-->
      <TopBarRight/>
    </template>
  </SfTopBar>
</template>

<script lang="ts">
import {defineComponent, ref} from '@nuxtjs/composition-api';
import { SfButton, SfTopBar } from '@storefront-ui/vue';
import { useTopBar } from './useTopBar';
import { SfRating } from '@storefront-ui/vue';
import { SfModal } from '@storefront-ui/vue';
import TopBarRight from "~/src/theme/TopBarRight.vue";

export default defineComponent({
  components: {
    TopBarRight,
    CurrencySelector: () => import('../CurrencySelector.vue'),
    StoreSwitcher: () => import('../StoreSwitcher.vue'),
    SfTopBar,
    SfButton,
    SfRating,
    SfModal,
  },
  setup() {
    const { hasCurrencyToSelect, hasStoresToSelect } = useTopBar();
    const isOpen = ref(false);

    return {
      hasCurrencyToSelect,
      hasStoresToSelect,
      isOpen,
    };
  },
});

</script>
<style lang="scss" scoped>
.topbar {

  z-index: 2;
  &__button {
    margin: 0 0 0 var(--spacer-xs);
  }
}
.sf-button--text{
  font-size: 10px;
  text-decoration: none;
  margin-right:40px;
  display: flex;
}
.topbar-icons{
  width: 15px;
  margin: 0 4px 2px 0;
}
.sf-modal {
  --modal-width: 800px;
}
.footer-modal {
  display: flex;
  justify-content: flex-end;
  border-top: 1px solid #c1c1c1;
}
.ml{
  margin: 0;
}
.modal-content{
  padding: 10px 20px;
}
.sf-rating{
  --icon-color: #F18912;
  justify-content: center;
}
.button-close{
  margin-top: 10px;
}
.sf-modal__content {
  padding: 0!important;
  --modal-content-padding: 0!important;
}
.modal-header{
  text-align: center;
}
::v-deep {
  .sf-top-bar__container {
    justify-content: space-between;
    max-width: 100%!important;
    padding: 0 30px;
    & > * {
      width: calc(100% / 3);
      justify-content: center;
    }
    & > :first-child {
      justify-content: flex-start;
    }
    & > :last-child {
      justify-content: flex-end;
    }
  }
}
</style>
