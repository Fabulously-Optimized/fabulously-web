<script setup lang="ts">
import {
  Button,
  DownloadIcon,
  ClientIcon,
  UsersIcon,
  ChartIcon,
  LightBulbIcon,
  VersionIcon,
  PaintBrushIcon,
  renderHighlightedString,
  UpdatedIcon,
  DiscordIcon,
} from "omorphia";

import FabricIcon from "../components/FabricIcon.vue";

const featureLinks =
  ref(`\n\n[1]: https://github.com/Fabulously-Optimized/fabulously-optimized/blob/main/INCLUDED-MODS.md#smooth
[2]: https://wiki.download.fo/readme/give-up-optifine
[3]: https://wiki.download.fo/readme/free-cape
[4]: https://github.com/Fabulously-Optimized/fabulously-optimized/blob/main/INCLUDED-MODS.md#functional
[5]: https://github.com/Fabulously-Optimized/fabulously-optimized#downloads
[6]: https://download.fo/changelog
[7]: https://wiki.download.fo/readme/update-instructions
[8]: https://wiki.download.fo/readme/adding-more-mods
[9]: https://download.fo/github
[10]: https://download.fo/translate
[11]: https://download.fo/discord
[12]: https://wiki.download.fo
[13]: https://download.fo/thanks
[14]: https://wiki.download.fo/readme/server-setup
[15]: https://download.fo/host
[16]: https://download.fo/terms`);

useSeoMeta({
  title: "Fabulously Optimized",
  ogTitle: "Fabulously Optimized",
  description: `A simple Minecraft modpack focusing on performance and graphics enhancements.`,
  ogDescription: `A simple Minecraft modpack focusing on performance and graphics enhancements.`,
  ogImage: "/icon.png",
  twitterCard: "summary",
  twitterImage: "/icon.png",
  themeColor: "#d19321",
});

interface FeatureItem {
  id: string;
  icon: any;
}

const features: any = ref([
  {
    id: "performance",
    icon: ChartIcon,
  },
  {
    id: "optifine-parity",
    icon: PaintBrushIcon,
  },
  {
    id: "feels-familiar",
    icon: LightBulbIcon,
  },
  {
    id: "works-anywhere",
    icon: ClientIcon,
  },
  {
    id: "up-to-date",
    icon: UpdatedIcon,
  },
  {
    id: "built-on-fabric",
    icon: FabricIcon,
  },
  {
    id: "open-development",
    icon: VersionIcon,
    button: {
      color: "black",
      href: "/contributors",
      external: false,
    },
  },
  {
    id: "helpful-community",
    icon: UsersIcon,
  },
]);
</script>

<template>
  <!-- <Modal class="download_modal" header="Download" ref="download_modal"
    ><DownloadSection
  /></Modal> -->
  <div class="columned-hero">
    <div class="column">
      <h1>
        <span class="supercharge__gradient">{{
          $t("content.home.columned-hero.supercharge")
        }}</span>
        {{ $t("content.home.columned-hero.title") }}
      </h1>
      <p class="subtitle">{{ $t("content.home.columned-hero.subtitle") }}</p>
      <br />
      <div class="buttons">
        <!-- <Button color="primary" @click="$refs.download_modal.show()"
          ><DownloadIcon /> Download</Button
        >
        <Button
          color="green"
          @click="
            navigateTo(`https://modrinth.com/modpack/fabulously-optimized`, {
              external: true,
              open: {
                target: '_blank',
              },
            })
          "
          >View on Modrinth</Button
        > -->
        <Button
          color="green"
          @click="navigateTo(`/modrinth`, { external: true })"
          ><DownloadIcon />Modrinth</Button
        >
        <Button
          color="orange"
          @click="navigateTo(`/curseforge`, { external: true })"
          ><DownloadIcon />CurseForge</Button
        >
        <Button @click="navigateTo(`/vanilla`, { external: true })"
          ><DownloadIcon /> Vanilla</Button
        >
      </div>
    </div>
    <div class="column graph__container">
      <!-- <img class="dramatic-screenshot" src="/dramatic-screenshot.webp" /> -->
      <div class="graph">
        <div class="item">
          <p class="pretitle">FO</p>
          <div class="bar" style="background-color: #f5a50f !important">
            <p>259fps</p>
          </div>
        </div>
        <div class="item">
          <p class="pretitle">OptiFine</p>
          <div class="bar"><p>56fps</p></div>
        </div>
        <div class="item">
          <p class="pretitle">{{ $t("content.home.graph.vanilla") }}</p>
          <div class="bar"><p>49fps</p></div>
        </div>
      </div>
      <br />
      <div>
        <h3>
          Fabulously Optimized {{ $t("content.home.graph.title.is") }}
          <span class="smaller__gradient"
            >4x {{ $t("content.home.graph.title.quicker") }}</span
          >{{ $t("content.home.graph.title.than") }} OptiFine.<br />
        </h3>
      </div>
      <small
        >AMD Ryzen 5 2600 (12) @ 3.400GH and NVIDIA GeForce GTX 1060 3GB running
        Minecraft 1.20.1, OptiFine for 1.20.1, Fabric Loader with Fabulously
        Optimized 5.4.1 RD:8, Singleplayer, PLd</small
      >
    </div>
  </div>
  <div class="centered-hero">
    <h1>{{ $t("content.home.features.title") }}</h1>
    <iframe
      width="560"
      height="315"
      :src="`https://www.youtube.com/embed/${$t(
        'content.home.features.videoID'
      )}`"
      title="YouTube video player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
      allowfullscreen
    ></iframe>
    <div class="features">
      <div v-for="feature in features" class="feature-block">
        <div>
          <h1>
            <component class="feature-icon" :is="feature.icon"></component
            ><span>{{ $t(`feature.${feature.id}.title`) }}</span>
          </h1>
          <div
            class="markdown-body"
            v-html="
              renderHighlightedString(
                $t(`feature.${feature.id}.desc`) + featureLinks
              )
            "
          ></div>
          <br />
          <Button
            v-if="feature.button"
            :large="true"
            :color="feature.button.color"
            @click="
              navigateTo(feature.button.href, {
                external: feature.button.external,
              })
            "
            ><component
              :is="feature.button.icon"
              v-if="feature.button.icon != undefined"
            ></component>
            {{ $t(`feature.${feature.id}.btn`) }}</Button
          >
        </div>
        <!-- <img
      v-if="features.indexOf(feature) % 2 === 0"
      :src="`/features/${feature.id}.webp`"
      class="dramatic-screenshot"
    /> -->
      </div>
    </div>
  </div>
  <div class="centered-hero" style="margin-top: 5rem">
    <DownloadIcon />
    <h1>{{ $t("content.home.download.title") }}</h1>
    <p class="subtitle">
      {{ $t("content.home.download.subtitle") }}
    </p>
    <div class="buttons">
      <Button
        :large="true"
        color="green"
        @click="navigateTo(`/modrinth`, { external: true })"
        >Modrinth</Button
      >
      <Button
        :large="true"
        color="orange"
        @click="navigateTo(`/curseforge`, { external: true })"
        >CurseForge</Button
      >
    </div>
    <!-- <DownloadSection class="download-card-fix" /> -->
  </div>
</template>

<style scoped lang="scss">
.download-card-fix {
  border-radius: var(--gap-xl) !important;
}

h3 {
  color: var(--color-contrast) !important;
}

.features {
  // Two columned grid.
  padding: 9%;
  padding-top: 0;

  // make sure text isn't long widthwise
  max-width: 75vw;

  // align everything center
  margin-left: auto;
  margin-right: auto;

  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr;
  gap: 0;
}

.feature-block {
  gap: var(--gap-xl);
  align-items: center;
  padding-right: 2%;

  // img {
  //   width: 85%;
  //   height: 85%;
  //   margin-left: auto;
  //   margin-right: auto;
  //   object-fit: cover;

  //   border-radius: var(--gap-xl);

  //   // Drop Shadow
  //   box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.75);
  // }

  div {
    .feature-icon {
      width: 2.5rem;
      height: 2.5rem;
    }

    h1 {
      display: flex;
      gap: var(--gap-md);
      span {
        margin-top: auto;
        margin-bottom: auto;
      }
    }
  }
}

.columned-hero {
  display: grid;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: 1fr;
  gap: var(--gap-xl);
  align-items: center;
  margin-bottom: 8.75rem;
}

// On mobile, hide graph
@media (max-width: 999px) {
  .columned-hero {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr;
    margin-bottom: 0 !important;
  }

  .graph__container {
    display: none !important;
  }

  .features {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr;
    padding: 0 !important;
  }

  // Hide video on mobile.
  .centered-hero {
    margin-top: 5rem !important;
    // iframe {
    //   display: none !important;
    // }
  }
}

.column {
  display: block;
}

.buttons {
  display: flex;
  flex-direction: row;
  gap: var(--gap-md);
}
</style>
