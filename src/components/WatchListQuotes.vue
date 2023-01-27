<template>
  <!-- passar props como argumentos e mensagens para quando não houver coins -->
  <list-quotes :quotes="quotes" :listenQuotes="this.listenQuotes"/>
  <div class="text-right mt-2">
    <cite class="text-small">
      Atualizará a cada <b>{{ nextUpdateTime }} segundos</b>.
    </cite>
  </div>
</template>

<script lang="ts">
import { reactive, ref, toRefs } from "@vue/reactivity";
import ListQuotes from './ListQuotes.vue';
import api from '../services/api';
import { onMounted } from '@vue/runtime-core';

const UPDATE_TIME = (seconds:number):number => seconds * 1e3

export default {
  components: { ListQuotes },
  props: {
    listenQuotes: {type: Array, required: true},
  },
  setup(props) {
    const interval = ref(null)
    const quotes = ref({});
    const nextUpdateTime = ref(30)

    onMounted(() => {
      methods.refresh();
    })

    const methods = reactive({
      async refresh() {
        const { data } = await api.listen(props.listenQuotes);
        quotes.value = data;
      }
    })
    return {
      quotes,
      nextUpdateTime,
      ...toRefs(methods),
    }
  },
};
</script>