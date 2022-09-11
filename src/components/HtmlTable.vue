<template>
  <b-table-simple
    v-if="data !== null"
    :bordered="true"
    class="table-height"
    responsive
    hover
    small
  >
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

<style scoped>
.table-height {
  overflow-y: auto;
  height: calc(100vh - 462px);
}
</style>
