<template>
  <div
    class="searchpanel fixed mw-100 bg-cl-primary cl-accent"
    :class="{ active: isOpen }"
    data-testid="searchPanel"
  >
    <div class="row">
      <div class="col-md-12 end-xs">
        <i
          class="material-icons p15 pointer cl-accent"
          @click="closeSearchpanel"
          data-testid="closeSearchPanel"
        >
          close
        </i>
      </div>
    </div>
    <div class="col-md-12 end-xs">
      <label for="search" class="visually-hidden">
        {{ $t('Search') }}
      </label>
      <input
        ref="search"
        id="search"
        v-model="search"
        @input="makeSearch"
        class="mr20 py10 brdr-none brdr-bottom-1 brdr-cl-primary no-outline h4"
        :placeholder="$t('Type what you are looking for...')"
        type="text"
      >
    </div>
    <div class="col-md-12 product-listing pl35 pt20 row">
      <product-tile @click.native="closeSearchpanel" :key="product.id" v-for="product in products" :product="product"/>
      <transition name="fade">
        <div v-if="emptyResults" class="no-results relative center-xs h4">
          {{ $t('No results were found.') }}
        </div>
      </transition>
    </div>
    <div v-show="OnlineOnly" v-if="products.length >= 18" class="buttons-set align-center py35 mt20 px40">
      <button @click="seeMore" v-if="readMore"
              class="no-outline brdr-none py15 px20 bg-cl-mine-shaft :bg-cl-th-secondary cl-white fs-medium-small"
              type="button">
        {{ $t('Load more') }}
      </button>
      <button @click="closeSearchpanel"
              class="no-outline brdr-none p15 fs-medium-small close-button"
              type="button">
        {{ $t('Close') }}
      </button>
    </div>
  </div>
</template>

<script>
import SearchPanel from '@vue-storefront/core/components/blocks/SearchPanel/SearchPanel'
import ProductTile from 'theme/components/core/ProductTile'
import VueOfflineMixin from 'vue-offline/mixin'

export default {
  components: {
    ProductTile
  },
  mixins: [SearchPanel, VueOfflineMixin],
  mounted () {
    this.$bus.$on('focusSearchInput', () => {
      if (!this.$store.state.ui.searchpanel) {
        this.$refs.search.focus()
      }
    })
  }
}
</script>

<style lang="scss" scoped>
@import "~theme/css/animations/transitions";

.searchpanel {
  height: 100vh;
  width: 800px;
  top: 0;
  right: 0;
  z-index: 3;
  transform: translateX(100%);
  transition: transform 300ms $motion-main;
  overflow-y: auto;
  overflow-x: hidden;

  &.active {
    transform: translateX(0)
  }

  .product {
    width: 30%;
    padding: 10px;
  }

  input {
    width: calc(100% - 40px);
  }

  .no-results {
    top: 80px;
    width: calc(100% - 40px);
  }
}

i {
  opacity: 0.6;
}

i:hover {
  opacity: 1;
}

.close-button {
  background: #fff;
}

@media only screen and (max-width:50em) {
  .searchpanel .product {
    width: 50%;
    box-sizing: border-box;
  }
  button {
    width: 100%;
    margin-bottom: 15px;
  }
}
</style>
