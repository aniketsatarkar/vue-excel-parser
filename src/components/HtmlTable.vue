<template>
  <b-table-simple responsive hover small :bordered="true" v-if="data !== null">
    <b-tbody>
      <b-tr v-for="(row, rowIndex) in data" :key="rowIndex">
        <b-td v-for="(col, colIndex) in row" :key="colIndex">
          {{ col }}
        </b-td>
      </b-tr>
    </b-tbody>
  </b-table-simple>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    data: {
      default: null,
      type: Array,
    },
  },
  setup() {},
});
</script>
