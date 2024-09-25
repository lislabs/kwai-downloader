<script setup lang="ts">
const videoLink = ref('')

const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

const pasteFromClipboard = () => {
  navigator.clipboard.readText().then((text) => {
    videoLink.value = text
  }).catch((err) => {
    console.error('Failed to read clipboard contents: ', err)
  })
}

useSeoMeta({
  ogTitle: 'Kwai Video Downloader',
  description: 'Kwai Video Downloader is a tool that allows you to download videos from Kwai.',
  ogDescription: 'Kwai Video Downloader is a tool that allows you to download videos from Kwai.'
})
</script>

<template>
  <div v-if="page">
    <div class="absolute inset-0 landing-grid z-[-1] [mask-image:radial-gradient(100%_100%_at_top_right,white,transparent)]" />

    <div class="flex flex-col items-center justify-center text-center mt-20 mx-6">
      <h1 class="text-4xl font-black">
        Online Kwai Video Downloader
      </h1>
      <h2 class="text-xl text-gray-500 dark:text-gray-400 mt-4">
        Fast, free, and high-quality. Easy to save videos from Kwai App. Without any watermark.
      </h2>
      <div class="flex items-center justify-center mt-8 gap-2.5">
        <UInput
          v-model="videoLink"
          color="white"
          variant="outline"
          placeholder="Paste Kwai video link here"
          icon="i-heroicons-link"
          size="xl"
        />
        <UButton
          color="white"
          icon="i-heroicons-clipboard"
          size="xl"
          @click="pasteFromClipboard"
        />
        <UButton
          color="white"
          label="Download"
          icon="i-heroicons-arrow-down-tray-solid"
          size="xl"
        />
      </div>

      <ULandingFAQ
        :items="page.faq.items"
        multiple
        default-open
        class="max-w-4xl mx-auto mt-10 text-left"
      />
    </div>
  </div>
</template>

<style scoped>
.landing-grid {
  background-size: 100px 100px;
  background-image:
    linear-gradient(to right, rgb(var(--color-gray-200)) 1px, transparent 1px),
    linear-gradient(to bottom, rgb(var(--color-gray-200)) 1px, transparent 1px);
}
.dark {
  .landing-grid {
    background-image:
      linear-gradient(to right, rgb(var(--color-gray-800)) 1px, transparent 1px),
      linear-gradient(to bottom, rgb(var(--color-gray-800)) 1px, transparent 1px);
  }
}
</style>
