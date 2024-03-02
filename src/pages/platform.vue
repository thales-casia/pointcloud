<template>
  <div class="platform">
    <loading-surface v-if="isLoading" class="msg">{{loadingMsg}}</loading-surface>
    <canvas ref="canvas" width="500" height="500" style="width:100%" @click="onCast"></canvas>
  </div>
</template>
<script setup lang="ts">
import LoadingSurface from '@/components/loading-surface.vue';
import { usePlatform } from '@/store';
import { computed, onMounted, ref, watch } from 'vue';
const props = defineProps({
  id: String
});
const store = usePlatform();
const canvas = ref<HTMLCanvasElement>();
function onCast(e:MouseEvent) {
  const x = ( e.clientX / window.innerWidth ) * 2 - 1;
	const y = - ( e.clientY / window.innerHeight ) * 2 + 1;
  store.cast(x, y);
}
const isLoading = computed(() => store.isLoading);
const loadingMsg = computed(() => store.loadingMsg);


function toggle() {
  store.toggleFold();
}

onMounted(() => {
  if(canvas.value) {
    store.freight(canvas.value); // 装载canvas
    store.start(props.id||''); // 按照config开始执行
  }
});
watch(() => props.id, (val, old) => {
  store.start(val||''); // config修改
});
</script>
