<template>
    <HeaderSection/>
    <main class="container flex-grow px-4 mx-auto my-8">
        <slot/>
    </main>
    <FooterSection/>
</template>

<script>
  import HeaderSection from "./HeaderSection.vue"
  import FooterSection from "./FooterSection.vue"

  export default {
    components: {
      HeaderSection,
      FooterSection,
    },
  }
</script>
