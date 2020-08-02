<!-- =========================================================================================
	File Name: ItemGridView.vue
	Description: Item Component - Grid VIew
	----------------------------------------------------------------------------------------
	Item Name: Vuexy - Vuejs, HTML & Laravel Admin Dashboard Template
	Author: Pixinvent
	Author URL: http://www.themeforest.net/user/pixinvent
========================================================================================== -->

<template>
    <!-- <div class="item-grid-view vx-row match-height">
        <div class="vx-col" :class="gridColumnWidth" v-for="item in items" :key="item.objectID"> -->
            <vx-card class="grid-view-item mb-base overflow-hidden" v-on="$listeners">
                <template slot="no-body">

                  <div class="flex mb-4 ">
                    <div class="w-4/12 ">
                      <!-- ITEM IMAGE -->
                      <div class="item-img-container bg-white flex items-center justify-center mt-5 cursor-pointer" @click="navigate_to_detail_view">
                        <img :src="item.image" :alt="item.name" class="imagem-quadrada grid-view-img px-4">
                      </div>
                    </div>
                    <div class="w-8/12 ">
                      <div class="item-details px-4">



                        <!-- TITLE & DESCRIPTION -->
                        <div class="my-4">
                          <h6 class="truncate font-semibold mb-1 hover:text-primary cursor-pointer" @click="navigate_to_detail_view">{{ item.name }}</h6>
                          <p class="item-description truncate text-sm">{{ item.description }}</p>
                        </div>
                        <!-- RATING & PRICE -->
                        <div class="flex justify-between items-center">
                          <h6 class="font-bold">${{ item.price }}</h6>
                          <div class="text-warning border border-solid border-warning flex py-1 px-2 rounded">
                            <span class="text-sm mr-1">{{ item.rating }}</span>
                            <feather-icon icon="StarIcon" svgClasses="h-4 w-4" />
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>



                </template>
            </vx-card>
       <!--  </div>
    </div> -->
</template>

<script>
export default{
    props: {
        item: {
            type: Object,
            required: true
        },
    },
    methods: {
      navigate_to_detail_view() {
        this.$router.push({name: 'ecommerce-item-detail-view', params: {item_id: this.item.objectID }})
          .catch(() => {})
      }
    }
}
</script>

<style lang="scss" scoped>
.grid-view-item {
    .grid-view-img {
        max-width: 100%;
        max-height: 100%;
        width: auto;
        transition: .35s;
    }

    &:hover {
        transform: translateY(-5px);
        box-shadow: 0px 4px 25px 0px rgba(0,0,0,.25);

        .grid-view-img{
            opacity: 0.9;
        }
    }
}

  .imagem-quadrada{
    width: 100px;
    height: 100px;
  }
</style>
