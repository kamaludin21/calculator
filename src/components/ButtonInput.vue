<template>
  <button type="button" class="btn" >
    {{ symbols }}
  </button>
</template>

<script>
  export default {
    name: 'ButtonInput',
    props: {
      symbols: String
    }
  }
</script>

<style scoped lang="postcss">
  .btn {
    @apply focus:outline-none w-full rounded-md flex justify-center items-center text-slate-300 hover:ring-1 ring-blue-700 ring-inset duration-200;
  }
</style>