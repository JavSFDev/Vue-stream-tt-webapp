<template>
    <div class="d-container" style="margin-top: 120px;" @click="navigationStateChange()">
        <iframe id="siberPlayer" name="siberPlayer" style="height: calc(87vh);width: 100vw;" :src=iframeSrc allowfullscreen
            frameborder="0" scrolling=false @load=postData>
        </iframe>
    </div>
</template>
     
<style scoped lang="scss">
@import '../../styles/home.scss';
</style>

<script>
import CardVod from '../../assets/img/card-vod.png';
import CardLive from '../../assets/img/card-live.png';
import PlayIcon from '../../assets/img/icon-play.png';
import { number } from '@intlify/core-base';

import { devMode } from '../../utils/devConfig';
import {PLAYER_URL} from '@/mainConfig';
// import iframeResizer from "iframe-resizer";

export default {
    name: 'EventPlayer',
    components: {
    },
    props: {
        categoryId: {
            type: number,
            required: true,
        },
        vodId: {
            type: Number
        }
    },
    data() {
        return {
            PlayIcon: PlayIcon,
            CardVod: CardVod,
            CardLive: CardLive,
            cardSpaceMargin: 5,
            slidesPerViewCnt: 7,
            devConfig: {},
            SVODs: [],
            isDev: devMode,
            iframeSrc: `${PLAYER_URL}player.php?stream=` + this.$route.params.eventId + "&about=1&streams=1&videos=1&channels=1&matches=1&svods=1&apps=0&chat=1&donation=1&epg=0"
        }
    },
    mounted() {
        window.addEventListener(
            "resize", function (event) {
                var parent_data = { "window_height": window.innerHeight };
                document.getElementById("siberPlayer").contentWindow.postMessage(parent_data, "*");
        },
            true
        );
    },
    methods: {
        getSVODs() {
            this.$store.dispatch("setSVODByCategoryId",
                {
                    CategoryId: this.categoryId
                });
        },
        resizeCardSpace(cnt) {
            this.cardSpaceMargin = cnt * 10;
        },
        postData() {
            var parent_data = { "window_height": window.innerHeight };
            document.getElementById("siberPlayer").contentWindow.postMessage(parent_data, "*");
            //    iFrameResize({ scrolling: false, heightCalculationMethod: "taggedElement", checkOrigin: false}, '#siberPlayer');
            // iframeResizer({ scrolling: false, heightCalculationMethod: "taggedElement", checkOrigin: false}, "#siberPlayer");
            // iFrameResize({ scrolling: false, heightCalculationMethod: "taggedElement", checkOrigin: false}, "#siberPlayer");
        },
        navigationStateChange() {
            this.$store.dispatch("changeNavigationState", false);
        }
    },
    computed: {
        devConfig() {
            return this.$store.state.SVODStore.SVODs[this.categoryId];
        },
        SVODs() {
            let svdCnt = this.$store.state.SVODStore.SVODs.length;
            if (svdCnt == 0) {
                this.getSVODs();
            }
            if (svdCnt < 7) {
                // this.slidesPerViewCnt = svdCnt;
                // this.resizeCardSpace(this.slidesPerViewCnt);
            }

            return this.$store.state.SVODStore.SVODs[this.categoryId];
        },
    },
    watch: {
        SVODs(newVal) {
            return newVal;
        },
        categoryId(newVal) {
            return newVal;
        }
    },
    setup() {
        return {
            modules: [],
        };
    },
} 
</script>
     