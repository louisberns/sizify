<template>
  <div class="container-card container align-items-start" :class="{ wrap : config.wrap }">
    <div v-for="(card, index) in data" url="url" class="card" :style="{ minWidth: card.width * config.size + 'px'}" v-show="config.show === card.category || config.show === ''">

      <div class="card--head container column align-center" :style="{width: card.width * config.size + 'px'}">

        <div class="container align-center">
          <strong @click="checkIframeWidth()">{{card.name}}</strong>
          <div class="container align-items-center margin-left-10">
            <button type="button" name="button" class="btn btn-sm tooltip-top" title="Delete size" @click="removeSize(index)" v-if="card.category === 'default'"><i class="fas fa-trash-alt"></i></button>
            <button type="button" name="button" class="btn btn-sm tooltip-top" title="Expanded mode" @click="expand(card)"><i class="fas fa-expand-arrows-alt"></i></button>
          </div>
        </div>

        <div class="container sizes-box">
          <div class="flex-grow-1 container align-items-center">
            <span>W</span>
            <input type="number" v-model="card.width" name="" value="">
          </div>
          <div class="" :class="{hidden : config.size === .5}" v-show="!config.defaultHeight">
            x
          </div>
          <div class="flex-grow-1 container align-items-center" v-show="!config.defaultHeight">
            <span>H</span>
            <input type="number" v-model="card.height" name="" value="">
          </div>
        </div>
      </div>

      <div class="card--content" :class="{'default-height' : config.defaultHeight}" :style="{transform: 'scale(' + config.size + ',' + config.size + ')', transformOrigin: 'left top 0px', height: card.height * config.size + 'px', width: card.width * config.size + 'px'}">
        <iframe scrolling="yes" :id="'iframe-' + index" :src="config.url" :width="card.width" :height="card.height" :style="{ minWidth: card.width + 'px', height: card.height + 'px'}"></iframe>
        <div class="card--content--keyboard" v-show="config.keyboard">
          <img src="~/assets/images/teclado-horizontal.png" alt="" v-if="config.landscape">
          <img src="~/assets/images/teclado-vertical.png" alt="" v-else>
        </div>
      </div>

    </div>

  </div>
</template>

<script>
  import {
    mapState
  }
  from 'vuex'
  import tippy from 'tippy.js';

  export default {
    name: 'card',
    data() {
      return {
        default: false
      }
    },
    props: {
      data: {
        type: Array
      },
      url: {
        type: String
      }
    },
    mounted() {
      tippy(document.querySelectorAll('.tooltip-top'), {
        placement: 'top',
        delay: 200,
      })
    },
    methods: {
      removeSize: function(index) {
        console.log(index)
        var r = confirm("Do you want to delete?")
        if (r == true) {
          this.sizes.splice(index, 1);
        }
      },

      expand: function(card) {
        this.expandSize.name = card.name
        this.expandSize.width = card.width
        this.expandSize.height = card.height
      },

      checkIframeWidth: function() {
        var iframeTeste = document.querySelector("#iframe-0")
        console.log(iframeTeste)
        var hasHorizontalScrollbar = iframeTeste.scrollWidth > iframeTeste.clientWidth
        var hasVerticalScrollbar = iframeTeste.scrollHeight > iframeTeste.clientHeight
        console.log(hasHorizontalScrollbar)
        console.log(hasVerticalScrollbar)
      }

    },
    computed: {
      ...mapState(['config', 'sizes', 'expandSize'])
    }
  }
</script>

<style lang="scss" scoped>

</style>
