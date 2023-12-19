<script setup lang="ts">
import { Avatar } from "omorphia";
</script>

<template>
  <footer class="footer">
    <div class="content has-text-centered">
      <p>
        {{ $t("footer.legal_notice") }}
      </p>
      <div class="link-rows">
        <div class="link-row">
          <h4>{{ $t("footer.header.contribute") }}</h4>
          <NuxtLink to="/github" :external="true">GitHub</NuxtLink>
          <NuxtLink to="/translate" :external="true">{{
            $t("footer.links.translate")
          }}</NuxtLink>
        </div>
        <div class="link-row">
          <h4>{{ $t("footer.header.discuss") }}</h4>
          <NuxtLink to="/discord" :external="true">Discord</NuxtLink>
          <NuxtLink to="/mastodon" :external="true">Mastodon</NuxtLink>
          <NuxtLink to="/wiki" :external="true">{{
            $t("navbar.links.wiki")
          }}</NuxtLink>
        </div>
        <div class="link-row">
          <h4>{{ $t("footer.header.download") }}</h4>
          <NuxtLink to="/modrinth" :external="true">Modrinth</NuxtLink>
          <NuxtLink to="/curseforge" :external="true">CurseForge</NuxtLink>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped lang="scss">
.footer {
  border-top-right-radius: var(--gap-xl);
  border-top-left-radius: var(--gap-xl);
  text-align: center;
  background-color: var(--color-raised-bg);
  color: var(--color-base);
  padding: 3rem;
  margin-top: 5rem;
  margin-bottom: 0rem;
  border-top: 1px solid var(--color-border);
  border-bottom: 1px solid var(--color-border);
}

.link-rows {
  display: flex;
  flex-direction: row;
  gap: var(--gap-xl);
  justify-content: space-evenly;
  margin-top: var(--gap-xl);

  // Align all link rows together.
  .link-row {
    display: flex;
    flex-direction: column;

    a {
      color: var(--color-brand);
    }
  }
}

.image {
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: row;
  gap: var(--gap-md);

  .username {
    margin-top: auto;
    margin-bottom: auto;
  }
}
</style>
