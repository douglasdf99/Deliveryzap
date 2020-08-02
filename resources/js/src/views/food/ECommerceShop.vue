<!-- =========================================================================================
  File Name: ECommerceShop.vue
  Description: eCommerce Shop Page
  ----------------------------------------------------------------------------------------
  Item Name: Vuexy - Vuejs, HTML & Laravel Admin Dashboard Template
    Author: Pixinvent
  Author URL: http://www.themeforest.net/user/pixinvent
========================================================================================== -->

<template>
    <div>

      <div id="algolia-content-container" class="relative clearfix">
        <!-- RIGHT COL -->
        <div >
          <!-- SEARCH BAR -->
          <div class="relative mb-8">

            <!-- SEARCH INPUT -->
            <vs-input class="w-full vs-input-shadow-drop vs-input-no-border d-theme-input-dark-bg" placeholder="Search here" @input="refine($event)" @keyup.esc="refine('')" size="large" />

            <!-- SEARCH LOADING -->
            <p  class="mt-4 text-grey">
              <feather-icon icon="ClockIcon" svgClasses="w-4 h-4" class="mr-2 align-middle" />
              <span>Loading...</span>
            </p>

            <!-- SEARCH ICON -->
            <div slot="submit-icon" class="absolute top-0 right-0 py-4 px-6" >
              <feather-icon icon="SearchIcon" svgClasses="h-6 w-6" />
            </div>

            <!-- CLEAR INPUT ICON -->
            <div slot="reset-icon" class="absolute top-0 right-0 py-4 px-6" >
              <feather-icon icon="XIcon" svgClasses="h-6 w-6 cursor-pointer" @click="refine('')" />
            </div>
          </div>
            <!-- GRID VIEW -->

              <div class="items-grid-view vx-row match-height">
                <div class="vx-col lg:w-1/4 sm:w-1/3 w-full" v-for="item in items2" :key="item.objectID">

                  <item-grid-view :item="item">

                    <!-- SLOT: ACTION BUTTONS -->
                    <template slot="action-buttons">
                      <div class="flex flex-wrap">

                        <!-- PRIMARY BUTTON: ADD TO WISH LIST -->
                        <div
                          class="item-view-primary-action-btn p-3 flex flex-grow items-center justify-center cursor-pointer"
                          @click="toggleItemInWishList(item)">
                          <feather-icon icon="HeartIcon" :svgClasses="[{'text-danger fill-current' : isInWishList(item.objectID)}, 'h-4 w-4']" />

                          <span class="text-sm font-semibold ml-2">WISHLIST</span>
                        </div>

                        <!-- SECONDARY BUTTON: ADD-TO/VIEW-IN CART -->
                        <div
                          class="item-view-secondary-action-btn bg-primary p-3 flex flex-grow items-center justify-center text-white cursor-pointer"
                          @click="cartButtonClicked(item)">
                          <feather-icon icon="ShoppingBagIcon" svgClasses="h-4 w-4" />

                          <span class="text-sm font-semibold ml-2" v-if="isInCart(item.objectID)">VIEW IN CART</span>
                          <span class="text-sm font-semibold ml-2" v-else>ADD TO CART</span>
                        </div>
                      </div>
                    </template>
                  </item-grid-view>

                </div>
              </div>


        </div>
      </div>
    </div>
</template>

<script>

export default {
  components: {
    ItemGridView: () => import("./components/ItemGridView.vue"),
    ItemListView: () => import("./components/ItemListView.vue"),
  },
  data() {
    return {
      searchClient: algoliasearch(
        'latency',
        '6be0576ff61c053d5f9a3225e2a90f76'
      ),
      // Filter Sidebar
      isFilterSidebarActive: true,
      clickNotClose: true,
      currentItemView: 'item-grid-view',

    }
  },
  computed: {
    toValue() {
      return (value, range) => [
        value.min !== null ? value.min : range.min,
        value.max !== null ? value.max : range.max,
      ]
    },

    // GRID VIEW
    isInCart() {
      return (itemId) => this.$store.getters['eCommerce/isInCart'](itemId)
    },
    isInWishList() {
      return (itemId) => this.$store.getters['eCommerce/isInWishList'](itemId)
    },
    items2() {
      return this.$store.state.eCommerce.items;
    },
    windowWidth() { return this.$store.state.windowWidth }
  },
  watch: {
    windowWidth() {
      this.setSidebarWidth()
    }
  },
  methods: {
    setSidebarWidth() {
      if (this.windowWidth < 992) {
        this.isFilterSidebarActive = this.clickNotClose = false
      } else {
        this.isFilterSidebarActive = this.clickNotClose = true
      }
    },

    // GRID VIEW - ACTIONS
    toggleFilterSidebar() {
      if (this.clickNotClose) return
      this.isFilterSidebarActive = !this.isFilterSidebarActive
    },
    toggleItemInWishList(item) {
      this.$store.dispatch('eCommerce/toggleItemInWishList', item)
    },
    additemInCart(item) {
      this.$store.dispatch('eCommerce/additemInCart', item)
    },
    cartButtonClicked(item) {
      this.isInCart(item.objectID) ? this.$router.push('/apps/eCommerce/checkout').catch(() => {}) : this.additemInCart(item)
    }
  },
  created() {
    this.setSidebarWidth()
  }
}

</script>


<style lang="scss">
#algolia-instant-search-demo {
  .algolia-header {
    .algolia-filters-label {
      width: calc(260px + 2.4rem);
    }
  }

  #algolia-content-container {

    .vs-sidebar {
      position: relative;
      float: left;
    }
  }

  .algolia-search-input-right-aligned-icon {
    padding: 1rem 1.5rem;
  }

  .algolia-price-slider {
    min-width: unset;
  }

  .item-view-primary-action-btn {
    color: #2c2c2c !important;
    background-color: #f6f6f6;
    min-width: 50%;
  }

  .item-view-secondary-action-btn {
    min-width: 50%;
  }
}

.theme-dark {
  #algolia-instant-search-demo {
    #algolia-content-container {
      .vs-sidebar {
        background-color: #10163a;
      }
    }
  }
}

@media (min-width: 992px) {
  .vs-sidebar-rounded {
    .vs-sidebar {
      border-radius: .5rem;
    }

    .vs-sidebar--items {
      border-radius: .5rem;
    }
  }
}

@media (max-width: 992px) {
  #algolia-content-container {
    .vs-sidebar {
      position: absolute !important;
      float: none !important;
    }
  }
}

</style>

