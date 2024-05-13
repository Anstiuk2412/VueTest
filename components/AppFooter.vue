<template>
  <FooterOverride
    :column="4"
    multiple
    class="footer"
  >
    <SfFooterColumn :title="$t('Information')">
      <SfList>
        <SfListItem
          v-for="item in information"
          :key="item"
        >
          <SfMenuItem
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Kundeservice')">
      <SfList>
        <SfListItem
          v-for="item in kundeservice"
          :key="item"
        >
          <SfMenuItem
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Guides')">
      <SfList>
        <SfListItem
          v-for="item in guides"
          :key="item"
        >
          <SfMenuItem
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Følg os')">
      <div class="footer__socials">
        <SfImage
          v-for="item in social"
          :key="item"
          class="footer__social-image"
          :src="addBasePath('/icons/'+item+'.svg')"
          :alt="item"
          width="50"
          height="50"
        />
      </div>
    </SfFooterColumn>
  </FooterOverride>
</template>

<script lang="ts">
import {
  SfFooter, SfList, SfImage, SfMenuItem,
} from '@storefront-ui/vue';
import { defineComponent } from '@nuxtjs/composition-api';
import { addBasePath } from '~/helpers/addBasePath';
import FooterOverride from "~/src/theme/FooterOverride.vue";

export default defineComponent({
  components: {
    FooterOverride,
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem,
  },
  setup() {
    return {
      addBasePath,
    };
  },
  data() {
    return {
      information: ['Handelsbetingelser', 'Persondatapolitik', 'Levering', 'Prøv din nye seng', 'Risikofri prøve - Gratis return', 'Om Sengetid', 'Blog', 'Udsalg hos Sengetid'],
      kundeservice: ['F.A.Q', 'Butik', 'Kundeudtalelser', 'Finansiering', 'Job', 'Kontakt', 'Book en sengeekspert'],
      guides: ['Sådan samles din seng', 'Guide til valg af fasthed', 'Guide til valg af topmadras', 'Guide til opbygning af Norhland seng'],
      social: ['facebook', 'instagram'],
      isMobile: false,
      desktopMin: 1024,
    };
  },
});
</script>

<style lang="scss">

.footer {
  --c-secondary: black;
  margin-bottom: 3.75rem;
  --c-white: #929292;
  margin-top: 50px;

  .sf-menu-item{
    .sf-button {
      margin-bottom: 10px;
    }
    &:hover{
      --menu-item-label-color: #333333;
    }
  }
  .copyrights {
    display: flex;
    flex-direction: row;
    font-size: 11px;
    color: #929292;
    justify-content: space-between;
    align-items: center;
    padding: 5px 0;
    a{
      color: #929292;
    }
  }
  .sf-heading__title {
    text-transform: capitalize;
  }
  span.sf-menu-item__label {
    font-size: 12px;
  }
  .footer__social-image {
    margin-right: 15px;
  }
  @include for-desktop {
    margin-bottom: 0;
  }
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 auto var(--spacer-lg);
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-start;
      padding: var(--spacer-xs) 0;
      margin: 0 auto;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-2xs) 0 0;
    background-color: black;
    border-radius: 100%;
  }
}
.sf-footer {
  padding: 30px 0 0;
  @include for-desktop {
    padding-bottom: 0;
    margin-top: var(--spacer-2xl);
  }
  &__container {
    margin: var(--spacer-sm);
    @include for-desktop {
      max-width: 80rem;
      margin: 0 auto;
    }
  }
}
</style>
