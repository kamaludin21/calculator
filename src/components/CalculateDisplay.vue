<template>
  <div class="flex h-2/5 w-full flex-col items-end justify-end rounded-md bg-slate-800 px-1 pt-1 text-right text-white">
    <div class="w-full overflow-y-auto" id="resultElement"	>
      <div class="space-y-3" >
				<div class="-space-y-1" v-for="history in histroryOperation" :key="history.operation">
					<p class="text-lg font-light">{{ history.operation }}</p>
					<p class="text-2xl font-medium">&#x3D; {{ history.result }}</p>
				</div>
			</div>
    </div>
    <div class="z-10 flex w-full flex-col items-end">
      <p class="text-2xl">{{ lastOperation != '0' ? lastOperation : '' }}</p>
      <div id="numberInput" ref="numberInput" class="scroll z-0 w-full overflow-x-auto overflow-y-hidden">
        <p class="text-5xl font-semibold">
          {{ lastOperation != '0' ? '&#x3D;' : '' }}{{ currentCalculate }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
	// import TrashIcon from './icons/TrashIcon.vue'
  export default {
    name: 'CalculateHistory',
		// components: { TrashIcon },
    props: {
      histroryOperation: Array,
      lastOperation: String,
      currentCalculate: String,
			deleteHistory: Function
    }
  }
</script>

<style lang="postcss" scoped>
/* ===== Scrollbar CSS ===== */
  /* Firefox */
  * {
    scrollbar-width: auto;
    scrollbar-color: #1e3a8a #ffffff;
  }

  /* Chrome, Edge, and Safari */
  *::-webkit-scrollbar {
    width: 11px;
  }

  *::-webkit-scrollbar-track {
    background: #1e293;
  }

  *::-webkit-scrollbar-thumb {
    background-color: #1e3a8a;
    border-radius: 10px;
    border: 3px solid #1e293b;
  }
</style>
