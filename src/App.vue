<template>
  <!-- TODO: Guardar as moedas tracked e as meodas da api em um gerenciador de estado global do Pinia e fazer o preservamento -->
  <div class="container grid-lg my-2 py-2">
    <!-- moedas que o usuario listou para acompanhar -->
    <div class="card mb-4" v-if="listenQuotes.length > 0">
      <div class="card-header">
        <div class="h4">Tracking Your Favorite(s) Coin(s)</div>
      </div>
      <div class="card-body">
        <WatchListQuotes :listenQuotes="listenQuotes"/>
      </div>
    </div>
    <div v-else>
      <div class="alert alert-danger text-center">Não há nenhuma moeda para ser rastreada</div>
    </div>
    
    <!-- todas as moedas buscadas pela api -->
    <div class="card">
      <div class="card-header">
        <div class="h4">All Coins...</div>
      </div>
      <div class="card-body">
        <list-quotes 
        :quotes="quotes" 
        :listenQuotes="listenQuotes"
        v-on:listen="onListen"
        v-on:unlisten="onUnListen"
        />
      </div>
    </div>
    <!-- usado apenas para testes da chamada de api -->
    <!-- {{ quotes }} -->
  </div>
</template>

<script lang="ts">
import { reactive, ref, toRefs } from "@vue/reactivity";
import api from './services/api';
import { onMounted } from '@vue/runtime-core';
import ListQuotes from './components/ListQuotes.vue';
import WatchListQuotes from "./components/WatchListQuotes.vue";

export default {
  components: { ListQuotes, WatchListQuotes },
  setup() {
    interface Quote {
      key: string;
      name: string;
      high: number;
      low: number;
      pctChange: number;
    }

    function onListen(code) {
      data.listenQuotes.push(code)
    }

    function onUnListen(code) {
      //alert(code)
      data.listenQuotes = data.listenQuotes.filter(key => key !== code)
      console.log(data.listenQuotes);
      
    }

    const data = reactive({
      quotes: {},
      listenQuotes: [],
    })

    onMounted(async () => {
      const response = await api.all();
      data.quotes = response.data;
    })

    return { ...toRefs(data), onUnListen, onListen };
  },
};
</script>

<style scoped></style>
