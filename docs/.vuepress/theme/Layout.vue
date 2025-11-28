<template>
  <div class="main-container" :class="{ 'has-top-banner': showTopBanner }">
    <div id="v3-banner">
      <span class="hidden-sm"
        >Vue Router 3 has reached EOL and is no longer actively
        maintained.</span
      >
      <a href="https://router.vuejs.org/">Upgrade to Vue Router 4</a>
    </div>

    <BannerTop v-if="showTopBanner" @close="closeBannerTop" />
    <ParentLayout>
      <template #page-top>
        <CarbonAds
          v-if="$site.themeConfig.carbonAds"
          :key="'ca:' + $page.path"
          :code="$site.themeConfig.carbonAds.carbon"
          :placement="$site.themeConfig.carbonAds.placement"
        />
      </template>
      <template #page-bottom>
        <BuySellAds
          v-if="$site.themeConfig.carbonAds"
          :key="'bsa:' + $page.path"
          :code="$site.themeConfig.carbonAds.custom"
          :placement="$site.themeConfig.carbonAds.placement"
        />
      </template>

      <template #sidebar-top>
        <div class="sponsors sponsors-top">
          <span>Platinum Sponsors</span>

          <template v-if="sponsors.platinum.length">
            <a
              v-for="sponsor in sponsors.platinum"
              :href="sponsor.href"
              :key="sponsor.href"
              target="_blank"
              rel="noopener"
            >
              <img :src="sponsor.imgSrcLight" :alt="sponsor.alt" />
            </a>
          </template>
          <a
            v-else
            class="become-sponsor"
            href="https://github.com/sponsors/posva"
            target="_blank"
            rel="noopener"
            alt="Your logo here"
            >Become a Sponsor!</a
          >
        </div>
      </template>

      <template #sidebar-bottom>
        <div class="sponsors">
          <span>Sponsors</span>

          <a
            v-for="sponsor in sponsors.gold"
            :href="sponsor.href"
            :key="sponsor.href"
            target="_blank"
            rel="noopener"
          >
            <img :src="sponsor.imgSrcLight" :alt="sponsor.alt" />
          </a>
        </div>
      </template>
    </ParentLayout>
  </div>
</template>

<script>
import ParentLayout from '@parent-theme/layouts/Layout.vue'
import CarbonAds from './components/CarbonAds.vue'
import BuySellAds from './components/BuySellAds.vue'
import sponsors from '../components/sponsors.json'

export default {
  name: 'Layout',
  components: {
    ParentLayout,
    CarbonAds,
    BuySellAds,
    BannerTop: () => import('./components/VueSchool/BannerTop.vue'),
  },
  data() {
    return {
      sponsors,
      showTopBanner: false,
    }
  },
  mounted() {
    const now = new Date()
    const end = new Date('2022-05-04T00:00:00+02:00')
    this.showTopBanner =
      !localStorage.getItem('VS_FW_22_BANNER_CLOSED') && now < end
  },
  methods: {
    closeBannerTop() {
      this.showTopBanner = false
      localStorage.setItem('VS_FW_22_BANNER_CLOSED', 1)
    },
  },
}
</script>

<style>
@media screen and (max-width: 1300px) {
  .content__default::before {
    content: '';
    /* background-color: red; */
    position: relative;
    display: block;
    /* top: 87px; */
    /* right: -12px; */
    float: right;
    height: 221px;
    /* width: 0; */
    padding: 0 0 20px 30px;
    margin-top: 20px;
    margin-right: -24px;
  }
}

@media screen and (max-width: 900px) {
  #v3-banner .hidden-sm {
    display: none;
  }
}

img {
  max-width: 100%;
}

#v3-banner {
  background-color: #ffb731;
  width: 100%;
  min-height: 40px;
  padding: 10px 60px;
  z-index: 19;
  box-sizing: border-box;
  text-align: center;
  color: #333;

  top: 0;
  position: fixed;
}

#v3-banner a {
  color: #34495e;
  font-weight: bold;
}

header.navbar,
aside.sidebar,
main.page,
main.home {
  margin-top: 40px;
}
</style>

<style scoped>
.sponsors {
  margin: 0 0 1rem 1.35rem;
}

.sponsors-top {
  margin-top: 1rem;
  /* workaround padding in vitepress */
  margin-bottom: -2rem;
}

.sponsors > span {
  /* margin: 1.25rem 0; */
  display: block;
  color: #999;
  font-size: 0.8rem;
}

.sponsors a:last-child {
  margin-bottom: 20px;
}
.sponsors a:first-child {
  margin-top: 18px;
}

.sponsors a {
  margin-top: 10px;
  width: 125px;
  display: block;
}
</style>
