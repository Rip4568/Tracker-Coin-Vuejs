<template>
  <table class="table">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Codigo</th>
        <th>Maximo</th>
        <th>Minimo</th>
        <th>Variação</th>
        <th>ACTIONS</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(quote, key) in quotes" :key="key">
        <td>{{ key }}</td>
        <td>{{ quote.name }}</td>
        <td>{{ quote.high }}</td>
        <td>{{ quote.low }}</td>
        <td>
          <span
            class="label label-rounded text-small"
            :class="{
              'label-error': quote.pctChange < 0,
              'label-success': quote.pctChange > 0,
            }"
          >
            {{ quote.pctChange }} %
          </span>
        </td>
        <td>
          <a
            v-if="!listenQuotes.includes(key)"
            href="#clicked"
            class="btn btn-primary btn-sml"
            data-tooltip="Seguir"
            v-on:click="$emit('listen', key)"
            ><i class="icon icon-plus"></i>
          </a>
          <a
            v-else
            href="#"
            class="btn btn-error btn-sm"
            data-tooltip="remover"
            v-on:click="$emit('unlisten', key)"
          ><i class="icon icon-minus"></i>
          </a>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts">
export default {
  setup() {
    return {};
  },
  props: {
    quotes: { type: Object, required: true },
    listenQuotes: { type: Array, required: true },
  },
  emits: ["listen", "unlisten"],
};
</script>
