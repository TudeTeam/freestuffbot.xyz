<template>
  <header :page="this.$route.name">
    <div class="header desktopHeader">
      <div class="headerInner">
        <div class="headerLogo">
          <nuxt-link
            to="/"
            @click="$ga.event({ eventAction: 'navigate', eventCategory: 'header', eventLabel: 'home' })"
          >
            <img src="@/assets/img/icon_round.webp" alt="FreeStuff Logo">
            <span class="name">FreeStuff</span>
          </nuxt-link>
        </div>
        <ul class="headerNav">
          <HeaderCategory
            v-for="category in pages"
            :key="category.name"
            :name="category.name"
            :elements="category.elements"
            :url="category.url"
            :color="category.color"
          />
        </ul>
        <a
          :href="startUrl"
          class="headerButton"
          @click="$ga.event({ eventAction: 'start', eventCategory: 'header', eventLabel: 'discord' })"
        >
          Get Started
        </a>
      </div>
    </div>
    <!-- <div class="header mobileHeader">
      <div class="headerInner">
        <div class="headerLogo">
          <nuxt-link to="/">
            <img src="@/assets/img/icon_round.webp" alt="FreeStuff Logo">
          </nuxt-link>
        </div>
        <div class="headerNav">
          <HeaderCategory
            :name="pagename"
            :elements="pages.map(c => c.elements).flat()"
            :url="null"
            :color="color"
          />
        </div>
      </div>
    </div> -->
  </header>
</template>

<script lang="ts">
import Vue from 'vue'
import HeaderCategory from './HeaderCategory.vue'

export default Vue.extend({
  components: {
    HeaderCategory
  },
  props: {
    pagename: { type: String, default: '' },
    color: { type: String, default: '' }
  },
  data () {
    return {
      audience: 'discord'
    }
  },
  computed: {
    pages () {
      switch (this.$route.name) {
        case 'telegram': return [
          {
            name: 'Guide',
            url: '/guide',
            elements: []
          },
          {
            name: 'Themes',
            url: '/themes?platform=telegram',
            elements: []
          },
          {
            name: 'FAQ',
            url: '/faq',
            elements: []
          }
        ]

        default: return [
          {
            name: 'Guide',
            url: '/guide',
            elements: []
          },
          {
            name: 'Themes',
            url: '/themes',
            elements: []
          },
          {
            name: 'FAQ',
            url: '/support/faq',
            elements: []
          }
        ];
      }
    },
    startUrl() {
      switch (this.$route.name) {
        case 'telegram': return '/start-telegram';
        default: return '/start';
      }
    }
  },
  mounted () {
    const url = new URL(window.location.href)
    if (url.searchParams.has('audience')) {
      const target = url.searchParams.get('audience') || ''
      // @ts-ignore
      this.audience = target
    }
  },
  methods: {
    login () {
      location.href = 'https://idp.tude.ga/login/'
    }
  }
})
</script>

<style scoped lang="scss">
@import '~/assets/style/all.scss';

header {
  background: transparent;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 110;
  width: 100%;
  border: 0;
  font-family: $font-header;
  font-size: 18px;
  font-style: inherit;
  font-weight: inherit;
  margin: 0;
  outline: 0;
  padding: 0;
  vertical-align: baseline;
  transition: opacity 0.1s ease-in;

  &[page="index"] {
    opacity: 0;
    animation: 1s fadein ease 3s;
    animation-fill-mode: forwards;
  }

  &[hide] {
    opacity: 0;
    pointer-events: none;
  }
}

@keyframes fadein {
  0% { opacity: 0; }
  100% { opacity: 1; }
}

.desktopHeader {
  visibility: visible;
  // @media (max-width: 848px) { visibility: hidden; }
}

.mobileHeader {
  visibility: hidden;
  // @media (max-width: 848px) { visibility: visible; }
}

.header {
  display: block;
  box-sizing: border-box;
  color: #fff;
  font-size: 14px;
  font-weight: 400;
  height: 72px;
  padding: 8px 0;
  position: absolute;
  user-select: none;
  width: 100%;
  z-index: 560;

  a { text-decoration: none !important; }
  * { font-family: $font-regular; }
  
  .headerInner {
    align-items: center;
    display: grid;
    height: 100%;
    margin: 0 auto;
    max-width: 1300px;
    padding: 5px 20px;
    position: relative;
    grid-template-columns: 1fr auto 1fr;

    .headerLogo {
      flex: 0 0 auto;
      margin-right: 12px;
      display: flex;
      align-items: center;

      a {
        display: flex;
        align-items: center;
        height: 40px;
      }

      img {
        height: 40px;
        margin-right: 12px;
      }

      .name {
        font-family: $font-header;
        color: $color-header;
        font-size: 18pt;
        position: relative;
      }
    }

    .headerNav {
      display: flex;
      align-items: center;
      justify-content: center;
      flex: 1 0 auto;
      margin: 0;
      min-height: 56px;
      padding: 0;

      & > li {
        font-size: 18px;
        -webkit-font-smoothing: subpixel-antialiased;
        -webkit-transition: opacity 0.125s;
        display: inline-block;
        position: relative;
        transition: opacity 0.125s;
        will-change: opacity;
      }
    }

    .headerButton {
      display: inline-block;
      margin-left: auto;
      font-family: $font-major;
      font-size: 11pt;
      color: $color-header;
      background-image: linear-gradient(30deg, #ba38be, #7b7fe1);
      box-shadow: 0 0 15px #00000088;
      padding: 8pt 20pt;
      border-radius: 99px;
      cursor: pointer;

      &:hover {
        background-image: linear-gradient(30deg, #9e35a1, #6b6eb9);
      }
    }
  }

  @media (max-width: 800px) {
    animation: none !important;
    
    .headerInner {
      grid-template-columns: 1fr 1fr;

      .headerNav {
        display: none !important;
      }
    }
  }

  @media (max-width: 400px) {
    .headerLogo {
      img { height: 30px !important; }

      .name { font-size: 12pt !important; }
    }

    .headerButton { padding: 6pt 10pt !important; }
  }
}
</style>
