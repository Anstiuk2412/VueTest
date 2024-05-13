<template>
  <div>
    <SfHeaderOverride
      class="sf-header--has-mobile-search"
      :class="{ 'header-on-top': isSearchOpen }"
    >
      <template #logo>
        <div class="logo-wrapper">
          <a class="logo-conteiner">
            <img src="@/static/logo-dark.svg" alt="" class="logo-dark">
            <!--          <img src="@/static/logo-light.svg" alt="" class="logo-light">-->
          </a>
        </div>
      </template>
      <template #search>
<!--        <SfButton
          :is-search-open="isSearchOpen"
          @click="isSearchOpen = true"
          @set-is-open="isSearchOpen = $event"
          @set-search-results="productSearchResults = $event"
        class="button_search">
        <img src="@/static/icons/search.svg"
             alt="" class="topbar-icons">
        </SfButton>-->

      </template>
      <template #navigation>
          <HeaderNavigation :category-tree="categoryTree"/>
      </template>
      <template #aside>
        <div class="sf-header__switchers">
          <CurrencySelector
            v-if="hasCurrencyToSelect"
            class="smartphone-only"
          />
          <StoreSwitcher
            v-if="hasStoresToSelect"
            class="smartphone-only"
          />
        </div>
      </template>
      <template #before_search>

      </template>
      <template #header-icons="{ activeIcon }">
        <div class="sf-header__icons">
          <div class="text">
            <div>{{ $t('Eksperthjælp 09 - 20') }}</div>
            <img src="@/static/icons/phone-a.svg" alt="" class="topbar-icons">
            <div>+45 70 60 55 53</div>
          </div>
            <SearchBar
              :is-search-open="isSearchOpen"
              @set-is-open="isSearchOpen = $event"
              @set-search-results="productSearchResults = $event"
            />
<!--          <SfButton
            v-e2e="'app-header-account'"
            class="sf-button&#45;&#45;pure sf-header__action"
            data-testid="accountIcon"
            aria-label="Account"
            @click="handleAccountClick"
          >
            <SvgImage
              :icon="accountIcon"
              :label="$t('Account')"
              width="1.25rem"
              height="1.25rem"
              :class="{
                'sf-header__icon is-active': activeIcon === 'account',
              }"
            />
          </SfButton>-->
<!--          <SfButton
            v-if="isAuthenticated"
            class="sf-button&#45;&#45;pure sf-header__action"
            data-testid="wishlistIcon"
            aria-label="Wishlist"
            @click="toggleWishlistSidebar"
          >
            <SvgImage
              :icon="wishlistHasProducts ? 'heart_fill' : 'heart'"
              :label="$t('Wishlist')"
              width="1.25rem"
              height="1.25rem"
              class="sf-header__icon"
              :class="{
                'sf-header__icon is-active': activeIcon === 'wishlist',
              }"
            />
            <SfBadge
              v-if="wishlistHasProducts"
              class="sf-badge&#45;&#45;number cart-badge"
            >
              {{ wishlistItemsQty }}
            </SfBadge>
          </SfButton>-->
          <div class="cart-wrapper">
            <SfButton
              v-e2e="'app-header-cart'"
              class="sf-button--pure sf-header__action"
              aria-label="Toggle cart sidebar"
              @click="toggleCartSidebar"
            >
<!--            <SvgImage
              icon="cart_empty"
              :label="$t('Cart')"
              width="20"
              height="20"
              class="sf-header__icon"
              :class="{
                'sf-header__icon is-active': activeIcon === 'cart',
              }"
            />-->
              <img src="@/static/icons/cart.svg" alt="" class="topbar-icons">
              <SfBadge
                v-if="cartTotalItems"
                class="sf-badge--number cart-badge"
              >
                {{ cartTotalItems }}
              </SfBadge>
            </SfButton>
          </div>
        </div>
      </template>

    </SfHeaderOverride>
    <SearchResults
      v-if="isSearchOpen"
      :visible="isSearchOpen"
      :search-results="productSearchResults"
      @close="isSearchOpen = false"
    />
    <SfOverlay :visible="isSearchOpen" />
  </div>
</template>

<script lang="ts">
import {
  SfOverlay, SfHeader, SfButton, SfBadge,
} from '@storefront-ui/vue';

import {
  computed,
  ref,
  defineComponent,
  useRouter,
  useContext,
  onMounted,
  useFetch,
} from '@nuxtjs/composition-api';
import HeaderNavigation from '~/components/Header/Navigation/HeaderNavigation.vue';
import { useCategory } from '~/modules/catalog/category/composables/useCategory';
import {
  useUiHelpers,
  useUiState,
} from '~/composables';
import { useCart } from '~/modules/checkout/composables/useCart';
import { useWishlist } from '~/modules/wishlist/composables/useWishlist';
import { useUser } from '~/modules/customer/composables/useUser';
import { useWishlistStore } from '~/modules/wishlist/store/wishlistStore';
import type { CategoryTree, ProductInterface } from '~/modules/GraphQL/types';
import HeaderLogo from '~/components/HeaderLogo.vue';
import SvgImage from '~/components/General/SvgImage.vue';
import { useTopBar } from './TopBar/useTopBar';
import Marquee from './Header/Navigation/TopMessage.vue';
import SfHeaderOverride from "~/src/theme/CustomHeader.vue";

export default defineComponent({
  components: {
    SfHeaderOverride,
    HeaderNavigation,
    SfHeader,
    SfOverlay,
    HeaderLogo,
    SvgImage,
    SfButton,
    SfBadge,
    Marquee,
    CurrencySelector: () => import('~/components/CurrencySelector.vue'),
    StoreSwitcher: () => import('~/components/StoreSwitcher.vue'),
    SearchBar: () => import('~/components/Header/SearchBar/SearchBar.vue'),
    SearchResults: () => import(
      /* webpackPrefetch: true */ '~/components/Header/SearchBar/SearchResults.vue'
    ),
  },
  data() {
    return {
      iconPath: '../static/icons/cart.svg'
    }
  },
  setup() {
    const router = useRouter();
    const { app } = useContext();
    const { toggleCartSidebar, toggleWishlistSidebar, toggleLoginModal } = useUiState();
    const { setTermForUrl, getCatLink } = useUiHelpers();
    const { isAuthenticated } = useUser();
    const { loadTotalQty: loadCartTotalQty, cart } = useCart();
    const { loadItemsCount: loadWishlistItemsCount } = useWishlist();
    const { categories: categoryList, load: categoriesListLoad } = useCategory();

    const { hasCurrencyToSelect, hasStoresToSelect } = useTopBar();

    const isSearchOpen = ref(false);
    const productSearchResults = ref<ProductInterface[] | null>(null);

    const wishlistStore = useWishlistStore();
    const wishlistItemsQty = computed(() => wishlistStore.wishlist?.items_count ?? 0);

    const wishlistHasProducts = computed(() => wishlistItemsQty.value > 0);
    const accountIcon = computed(() => (isAuthenticated.value ? 'profile_fill' : 'profile'));
    const categoryTree = ref<CategoryTree[]>([]);

    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        await router.push(app.localeRoute({ name: 'customer-my-profile' }));
      } else {
        toggleLoginModal();
      }
    };

    useFetch(async () => {
      await categoriesListLoad({ pageSize: 20 });

      categoryTree.value = categoryList.value?.[0]?.children
        .filter((category) => category.include_in_menu);
    });

    onMounted(async () => {
      if (app.$device.isDesktop) {
        await loadCartTotalQty();
        await loadWishlistItemsCount();
      }
    });


    return {
      accountIcon,
      cartTotalItems: computed(() => cart.value?.total_quantity ?? 0),
      categoryTree,
      getCatLink,
      handleAccountClick,
      isAuthenticated,
      isSearchOpen,
      productSearchResults,
      setTermForUrl,
      toggleCartSidebar,
      toggleWishlistSidebar,
      wishlistHasProducts,
      wishlistItemsQty,
      hasCurrencyToSelect,
      hasStoresToSelect,
    };
  },
});
</script>

<style lang="scss" scoped>
.sf-header {
  --header-padding: var(--spacer-sm);
  @include for-desktop {
    --header-padding: 0 var(--spacer-sm);
  }
  &__header{

  }

  &__switchers {
    display: flex;
  }
}
.text {
  display: flex;
  align-content: center;
  flex-wrap: nowrap;
  align-items: center;
  margin: 0 10px;
}
.text div {
  margin: 0 10px;
}
.cart-wrapper {
  .sf-button {
    margin: 0;
    padding: 12px 5px;
  }
}
.logo-conteiner {
  max-width: 250px;
  width: 100%;
  text-align: center;
  img{
    height: 25px;
  }
}
.sf-header__icons {
  .sf-header__search{
    margin-top: 5px;
  }
}
.sf-input.sf-search-bar.sf-header__search {
  border-bottom: 0;
  max-width: 45px;
}
.sf-input.sf-search-bar.sf-header__search>div {
  margin: 10px 0px 0;
}
.button_search {
  background: transparent;
}
.sf-link.sf-header-navigation-item__link.nav-item {
  padding-left: 0;
}
.sf-header__wrapper {
  background: unset!important;
}
.sf-header__header{
  max-width: 100%!important;
}
.header-on-top {
  z-index: 2;
}

.cart-badge {
  position: absolute;
  bottom: 40%;
  left: 40%;
}
</style>
