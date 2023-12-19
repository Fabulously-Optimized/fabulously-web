<script setup lang="ts">
import {
  Card,
  Avatar,
  Button,
  MoonIcon,
  SunIcon,
  LanguagesIcon,
  OverflowMenu,
} from "omorphia";

const colorMode = useColorMode();
const { locales, locale: currentLocale, setLocale } = useI18n();

const theme = computed({
  get() {
    return colorMode.value !== "dark";
  },
  set(newValue) {
    colorMode.preference = newValue ? "light" : "dark";
  },
});

const availableLocales = computed(() => {
  return locales.value;
});

const languageOptionFilter = computed(() => {
  return availableLocales.value.map((locale: any) => ({
    id: locale.code,
    color: locale.code === currentLocale.value ? "green" : "",
    action: () => {
      setLocale(locale.code);
    },
    hoverFilledOnly: false,
  }));
});

console.log(languageOptionFilter.value);
</script>

<template>
  <Card class="nav">
    <NuxtLink class="side__link side__profile" to="/">
      <Avatar src="/icon.png"></Avatar>
      <h2 class="username">Fabulously Optimized</h2>
    </NuxtLink>
    <div class="side no__mobile">
      <NuxtLink to="/" class="side__link">{{
        $t("navbar.links.home")
      }}</NuxtLink>
      <!-- <NuxtLink to="/changelog" class="side__link">Changelog</NuxtLink> -->
      <NuxtLink to="/github" :external="true" class="side__link"
        >GitHub</NuxtLink
      >
      <NuxtLink to="/wiki" :external="true" class="side__link">{{
        $t("navbar.links.wiki")
      }}</NuxtLink>
    </div>
    <div class="side__buttons no__mobile">
      <Button iconOnly class="button__rounded_icon" @click="theme = !theme"
        ><MoonIcon v-if="!theme" /><SunIcon v-else
      /></Button>
      <OverflowMenu
        class="btn icon-only button__rounded_icon"
        style="background: none; box-shadow: none !important"
        :options="[
          {
            id: 'language',
            color: 'primary',
            action: () => {
              navigateTo('/translate', {
                external: true,
                open: {
                  target: '_blank',
                },
              });
            },
            hoverFilledOnly: false,
          },
          { divider: true },
          ...languageOptionFilter,
        ]"
      >
        <LanguagesIcon />
        <template #language
          ><LanguagesIcon />{{ $t("navbar.action.language") }}</template
        >
        <template v-for="_locale of availableLocales" v-slot:[_locale.code]>{{
          _locale.name
        }}</template>
      </OverflowMenu>
    </div>
  </Card>

  <!-- Responsive version of the navbar for only mobile. -->
  <Card class="nav nav-body">
    <div class="side">
      <NuxtLink to="/" class="side__link">{{
        $t("navbar.links.home")
      }}</NuxtLink>
      <!-- <NuxtLink to="/changelog" class="side__link">Changelog</NuxtLink> -->
      <NuxtLink to="/github" class="side__link">GitHub</NuxtLink>
      <NuxtLink to="/wiki" class="side__link">{{
        $t("navbar.links.wiki")
      }}</NuxtLink>
    </div>
  </Card>
</template>

<style scoped lang="css">
/* Mobile Navbar */
@media only screen and (min-width: 600px) {
  .nav-body {
    display: none !important;
  }
}

/* Hide no__mobile */
@media only screen and (max-width: 600px) {
  .no__mobile {
    display: none !important;
  }

  .side__profile {
    margin: auto;
  }

  .side__profile > .username {
    /* Adjust font size to fit horizontally fully. */
    /* Using vw */
    margin: auto;
    padding-left: auto;
    font-size: 5cqw;
  }
}

.button__rounded_icon,
.popup-container {
  border-radius: 50% !important;
  box-shadow: none !important;
  background-color: transparent !important;
  transition: color 100ms ease-in-out !important;
}

.side__buttons {
  display: flex;
  flex-direction: row;
  margin-top: auto;
  margin-bottom: auto;
  margin-block-end: inherit !important;
}

.button__rounded_icon:hover,
.popup-container {
  color: var(--color-brand) !important;
}

.nav-body {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.nav {
  padding: var(--gap-md);
  margin: auto;
  width: 85%;
  margin-top: var(--gap-lg) !important;
  background-color: var(--color-raised-bg);
  box-shadow: var(--shadow-raised-lg);
  display: flex;
}

.username {
  text-align: center;
  vertical-align: middle;
}

@media only screen and (min-width: 600px) and (max-width: 800px) {
  .username {
    display: none;
  }
}

@media only screen and (max-width: 1200px) {
  .side {
    left: 0 !important;
    position: inherit !important;
  }
}

.side {
  position: relative;
  display: flex;
  margin: auto;
  margin-left: auto;
  left: calc(var(--gap-xl) * -4);
  flex-direction: row;
  gap: var(--gap-xl);
}

.side__profile {
  display: flex;
  flex-direction: row;
  gap: var(--gap-md);
}

.side__link {
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
  text-decoration: none;
  color: var(--color-base);
}

.side__link:hover > *,
.side__link:hover {
  color: var(--color-brand);
  transition: color 100ms ease-in-out;
}

.side__link > .username {
  margin: auto;
}
</style>
