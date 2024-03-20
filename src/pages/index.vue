<script lang="ts" setup>
import { Icon } from '@iconify/vue';
import {
  useDashboard,
  LoadingStatus,
  type ChainConfig,
} from '@/stores/useDashboard';
import ChainSummary from '@/components/ChainSummary.vue';
import { computed, ref } from 'vue';
import { useBlockchain } from '@/stores';

const dashboard = useDashboard();

const keywords = ref('');
const chains = computed(() => {
  if (keywords.value) {
    const lowercaseKeywords = keywords.value.toLowerCase();

    return Object.values(dashboard.chains).filter(
      (x: ChainConfig) => x.chainName.toLowerCase().indexOf(lowercaseKeywords) > -1 
      || x.prettyName.toLowerCase().indexOf(lowercaseKeywords) > -1
    );
  } else {
    return Object.values(dashboard.chains);
  }
});

const featured = computed(() => {
  const names = ["cosmos", "osmosis", "akash", "celestia", "evmos", "injective", "dydx", "noble"];
  return chains.value
    .filter(x => names.includes(x.chainName))
    .sort((a, b)=> (names.indexOf(a.chainName) - names.indexOf(b.chainName)))
})

const chainStore = useBlockchain()
</script>
<template>
  <div class="">
    <div class="flex md:!flex-row flex-col items-center justify-center mb-6 mt-14 gap-2">
      <div class="w-16 rounded-full">
        <!-- <svg version="1.0" xmlns="http://www.w3.org/2000/svg" 
          viewBox="0 0 150.000000 132.000000"
          preserveAspectRatio="xMidYMid meet">
          <g transform="translate(0.000000,132.000000) scale(0.100000,-0.100000)"
          :fill="chainStore.current?.themeColor||'#666CFF'" class=" dark:invert" stroke="none">
            <path d="M500 1310 l-125 -5 -182 -315 c-100 -173 -182 -321 -182 -329 -1 -7
            81 -159 181 -337 l183 -324 372 0 371 0 186 325 c102 179 186 330 186 337 0 7
            -82 157 -182 335 l-183 323 -250 -2 c-137 -1 -306 -5 -375 -8z m588 -454 c61
            -106 112 -197 112 -201 0 -4 -50 -95 -111 -201 l-112 -194 -231 0 -231 0 -105
            181 c-58 100 -109 190 -114 200 -6 14 17 63 104 213 l112 196 232 0 231 0 113
            -194z"/>
            <path d="M591 1001 l-54 -6 -87 -150 -88 -150 176 -3 c97 -1 181 -1 187 2 9 3
            165 267 183 308 4 9 -233 7 -317 -1z"/>
            <path d="M872 824 l-90 -159 36 -66 c113 -201 147 -258 153 -251 8 8 179 302
            179 307 0 2 -37 68 -83 147 -46 78 -88 151 -94 162 -9 16 -24 -5 -101 -140z"/>
            <path d="M360 625 c0 -7 148 -263 172 -297 l19 -28 186 0 c101 0 183 3 181 8
            -1 4 -43 78 -93 165 l-90 157 -187 0 c-104 0 -188 -2 -188 -5z"/>
          </g>
        </svg> -->
        <!-- <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="200px" height="200px" style="shape-rendering:geometricPrecision; text-rendering:geometricPrecision; image-rendering:optimizeQuality; fill-rule:evenodd; clip-rule:evenodd" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet">
<g><path style="opacity:1" fill="#0f0f0f" d="M -0.5,-0.5 C 66.1667,-0.5 132.833,-0.5 199.5,-0.5C 199.5,66.1667 199.5,132.833 199.5,199.5C 132.833,199.5 66.1667,199.5 -0.5,199.5C -0.5,132.833 -0.5,66.1667 -0.5,-0.5 Z"/></g>
<g><path style="opacity:1" fill="#fbfbfb" d="M 159.5,138.5 C 144.5,139.167 129.5,139.833 114.5,140.5C 109.929,148.638 105.096,156.638 100,164.5C 95.3034,156.329 90.4701,148.329 85.5,140.5C 70.1667,139.833 54.8333,139.167 39.5,138.5C 59.236,104.03 79.236,69.6969 99.5,35.5C 120.265,69.361 140.265,103.694 159.5,138.5 Z"/></g>
<g><path style="opacity:1" fill="#303030" d="M 100.5,108.5 C 97.4155,102.661 94.0821,96.9942 90.5,91.5C 87.7801,85.7288 84.7801,80.0621 81.5,74.5C 88.1667,73.1667 94.8333,73.1667 101.5,74.5C 107.581,85.6633 113.914,96.6633 120.5,107.5C 113.866,108.497 107.2,108.83 100.5,108.5 Z"/></g>
<g><path style="opacity:1" fill="#060606" d="M 90.5,91.5 C 94.0821,96.9942 97.4155,102.661 100.5,108.5C 93.825,108.666 87.1583,108.5 80.5,108C 84.059,102.614 87.3924,97.1142 90.5,91.5 Z"/></g>
<g><path style="opacity:1" fill="#515151" d="M 119.5,112.5 C 106.833,111.167 94.1667,111.167 81.5,112.5C 80.9569,112.44 80.6236,112.107 80.5,111.5C 93.8333,110.167 107.167,110.167 120.5,111.5C 120.376,112.107 120.043,112.44 119.5,112.5 Z"/></g>
<g><path style="opacity:1" fill="#040404" d="M 119.5,112.5 C 113.667,123.505 107.334,134.172 100.5,144.5C 93.6663,134.172 87.3329,123.505 81.5,112.5C 94.1667,111.167 106.833,111.167 119.5,112.5 Z"/></g>
<g><path style="opacity:1" fill="#949494" d="M 39.5,138.5 C 54.8333,139.167 70.1667,139.833 85.5,140.5C 69.8192,140.833 54.1525,140.499 38.5,139.5C 38.6236,138.893 38.9569,138.56 39.5,138.5 Z"/></g>
<g><path style="opacity:1" fill="#969696" d="M 159.5,138.5 C 160.043,138.56 160.376,138.893 160.5,139.5C 145.181,140.499 129.848,140.833 114.5,140.5C 129.5,139.833 144.5,139.167 159.5,138.5 Z"/></g>
</svg> -->

      </div>
      <h1 class="text-primary dark:invert text-3xl md:!text-6xl font-bold">
        {{ $t('pages.title') }}
      </h1>
    </div>
    <div class="text-center text-base">
      <p class="mb-1">
        {{ $t('pages.slogan') }}
      </p>
    </div>
    <div
      v-if="dashboard.status !== LoadingStatus.Loaded"
      class="flex justify-center"
    >
      <progress class="progress progress-info w-80 h-1"></progress>
    </div>

    <div v-if="featured.length>0" class="text-center text-base mt-6 text-primary">
      <h2 class="mb-6"> Featured Blockchains 🔥 </h2>
    </div>

    <div v-if="featured.length>0"
      class="grid grid-cols-1 gap-4 mt-6 md:!grid-cols-3 lg:!grid-cols-4 2xl:!grid-cols-5"
    >
    <ChainSummary
        v-for="(chain, index) in featured"
        :key="index"
        :name="chain.chainName"
      />
    </div>

    <div class="text-center text-base mt-6 text-primary">
      <h2 class="mb-6">{{ $t('pages.description') }}</h2>
    </div>

    <div class="flex items-center rounded-lg bg-base-100  border border-gray-200 dark:border-gray-700 mt-10">
      <Icon icon="mdi:magnify" class="text-2xl text-gray-400 ml-3"/>
      <input :placeholder="$t('pages.search_placeholder')" class="px-4 h-10 bg-transparent flex-1 outline-none text-base" v-model="keywords" />
      <div class="px-4 text-base hidden md:!block">{{ chains.length }}/{{ dashboard.length }}</div>
    </div>

    <div
      class="grid grid-cols-1 gap-4 mt-6 md:!grid-cols-3 lg:!grid-cols-4 2xl:!grid-cols-5"
    >
      <ChainSummary
        v-for="(chain, index) in chains"
        :key="index"
        :name="chain.chainName"
      />
    </div>
  </div>
</template>

<style scoped>
 .logo path{
  fill: #171d30;
}
</style>
