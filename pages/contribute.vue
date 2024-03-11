<script setup lang="ts">
import { ref, onMounted } from 'vue';

// Define projectDetails as a reactive reference
const projectDetails = ref([]);

// Fetches the latest release version of the project
async function fetchProjectVersions() {
  const response = await fetch(`https://api.modrinth.com/v2/project/1KVo5zza/version?featured=true`);
  const versions = await response.json();
  return versions.find(version => version.version_type === "release");
}

// Fetches project details based on project IDs
async function fetchProjectDetails(projectIds) {
  const response = await fetch(`https://api.modrinth.com/v2/projects?ids=[${projectIds.map(id => `"${id}"`).join(',')}]`);
  return response.json();
}

// Displays projects by fetching their versions and details, then sorting and filtering them
async function displayProjects() {
  const version = await fetchProjectVersions();
  if (!version || !version.dependencies) return; // Exit if no version or dependencies found
  
  const projectIds = version.dependencies.map(dep => dep.project_id).filter(id => id);
  let projects = await fetchProjectDetails(projectIds);
  projects = projects
    .filter(project => project.donation_urls && project.donation_urls.length > 0) // Filter projects with at least one donation link
    .sort((a, b) => a.title.localeCompare(b.title)); // Sort projects by title

  // Transform project details for rendering
  projectDetails.value = projects.map(project => ({
    iconUrl: project.icon_url,
    title: project.title,
    donationUrls: project.donation_urls
  }));
}

// Use onMounted to ensure displayProjects is called after the component is mounted
onMounted(() => {
  displayProjects().catch(console.error);
});
</script>

<template>
<h1>{{ $t("content.contribute.title") }}</h1>
<p>{{ $t("content.contribute.supportMessage") }}</p>

<h2>{{ $t("content.contribute.modsTitle") }}</h2>
<p>{{ $t("content.contribute.modsDescription") }}</p>
<ul id="projects">
  <li v-for="project in projectDetails" :key="project.title">
    <img v-if="project.iconUrl" :src="project.iconUrl" :alt="`${project.title} icon`" />
    <a v-if="project.donationUrls.length === 1" :href="project.donationUrls[0].url" target="_blank">
      {{ project.title }}
    </a>
    <span v-else>
      {{ project.title }}:
      <a v-for="(donation, index) in project.donationUrls" :key="index" :href="donation.url" target="_blank">{{ donation.platform }}</a>
    </span>
  </li>
</ul>
<p><a href="https://download.fo/mods">{{ $t("content.contribute.modsLink") }}</a></p>
<p>{{ $t("content.contribute.modsAutomaticListFetch") }}</p>

<h2>{{ $t("content.contribute.modpackTitle") }}</h2>
<p>{{ $t("content.contribute.modpackSupportWays") }}</p>

<ul>
    <li>
        <i18n-t keypath="content.contribute.modpackLaunchersTitle" tag="strong">
          <a href="https://wiki.download.fo/readme/install-instructions#curseforge-app">{{ $t("content.contribute.modpackLaunchersTitle.curseforge") }}</a>
          <a href="https://wiki.download.fo/readme/install-instructions#modrinth-app">{{ $t("content.contribute.modpackLaunchersTitle.modrinth") }}</a>
          <a href="https://wiki.download.fo/readme/install-instructions#gdlauncher-carbon">{{ $t("content.contribute.modpackLaunchersTitle.gdlauncher") }}</a>
        </i18n-t>
        <ul>
            <li>{{ $t("content.contribute.modpackLaunchersMoneyInfo") }}</li>
        </ul>
    </li>
    <li>
        <strong><a href="https://download.fo/rate">{{ $t("content.contribute.modpackAltToHeartTitle") }}</a></strong>
        <ul>
            <li>{{ $t("content.contribute.modpackAltToHeartSignupInfo") }}</li>
        </ul>
    </li>
    <li>
        <strong><a href="https://wiki.download.fo/readme/free-cape">{{ $t("content.contribute.modpackFreeCapeTitle") }}</a></strong>
    </li>
    <li>
         <i18n-t keypath="content.contribute.modpackAdBlockerTitle" tag="strong">
          <a href="https://docs.modrinth.com/docs/details/ads/#browser-extensions">{{ $t("content.contribute.modpackAdBlockerTitle.adblocker") }}</a>
          <a href="https://modrinth.com/user/robotkoer">{{ $t("content.contribute.modpackAdBlockerTitle.projects") }}</a>
        </i18n-t>
    </li>
    <li>
        <strong><a href="https://download.fo/host">{{ $t("content.contribute.modpackMinecraftServer") }}</a></strong>
    </li>
    <li>
        <strong><a href="https://download.fo">{{ $t("content.contribute.modpackShare") }}</a></strong>
    </li>
</ul>

<h2>{{ $t("content.contribute.contributeTitle") }}</h2>
<p>{{ $t("content.contribute.contributeAdditionalSupportMessage") }}</p>
<ul>
    <li>
        <strong><a href="https://github.com/Fabulously-Optimized/fabulously-optimized/issues?q=is%3Aissue+is%3Aopen+label%3A%22feedback%2Ftesters+wanted%22">{{ $t("content.contribute.contributeTestModsTitle") }}</a></strong>
    </li>
    <li>
        <strong><a href="https://download.fo/translate">{{ $t("content.contribute.contributeTranslateTitle") }}</a></strong>
    </li>
    <li>
        <strong><a href="https://github.com/Fabulously-Optimized/wiki/issues">{{ $t("content.contribute.contributeWikiTitle") }}</a></strong>
    </li>
    <li>
        <strong><a href="https://github.com/Fabulously-Optimized/fabulously-optimized/labels/help%20wanted">{{ $t("content.contribute.contributeProgrammingTitle") }}</a></strong>
    </li>
    <li>
        <strong><a href="https://download.fo/discord">{{ $t("content.contribute.contributeDiscordTitle") }}</a></strong>
    </li>
</ul>
</template>


<style>
body { font-family: BlinkMacSystemFont,-apple-system,"Segoe UI",Roboto,Oxygen,Ubuntu,Cantarell,"Fira Sans","Droid Sans","Helvetica Neue",Helvetica,Arial,sans-serif; }
li img { width: 1em; height: 1em; margin-right: 0.5em; }
a { color: var(--color-brand); text-decoration: none; }
a:hover { text-decoration: underline; }
</style>
