<template>
  <div class="row text-left">
    <b-card>
      <div class="col-12">
        <b-form-textarea
          v-model="text"
          placeholder="Past Excel data here..."
          rows="10"
          max-rows="6"
        ></b-form-textarea>
        <div class="row mt-3">
          <div class="col-6">
            <b-form-checkbox v-model="hasHeader">
              Consider with column titles
            </b-form-checkbox>
          </div>
          <div
            v-if="text !== '' && !isExcel"
            class="col-6 text-danger text-right"
          >
            This is format cannot be parsed, please try again
          </div>
        </div>
      </div>

      <div class="col-12 mt-3">
        <b-card no-body class="dynamic-height">
          <b-tabs card>
            <b-tab title="JSON" active>
              <b-card-text>
                <JsonEditor :code="jsonString" />
              </b-card-text>
            </b-tab>
            <b-tab title="HTML Table">
              <b-card-text>
                <HtmlTable :data="json" />
              </b-card-text>
            </b-tab>
          </b-tabs>
        </b-card>
      </div>
    </b-card>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";
import JsonEditor from "@/components/JsonEditor.vue";
import HtmlTable from "@/components/HtmlTable.vue";

export default defineComponent({
  components: { JsonEditor, HtmlTable },
  setup() {
    let text = ref("");
    let json = ref(null);
    let hasHeader = ref(false);
    let columns = ref([]);

    return {
      text,
      json,
      hasHeader,
      columns,
    };
  },
  computed: {
    isExcel() {
      if (this.text !== null && this.text !== "") {
        const matches = this.text.match(/(\t).*(\n)/);
        if (matches && matches.length > 0) {
          return true;
        }
      }

      return false;
    },
    jsonString() {
      return JSON.stringify(this.json, null, 2);
    },
  },
  watch: {
    text() {
      if (this.isExcel) {
        this.parseExcelData();
      } else {
        this.json = null;
      }
    },
    hasHeader() {
      if (this.isExcel) {
        this.parseExcelData();
      } else {
        this.json = null;
      }
    },
  },
  methods: {
    parseExcelData() {
      if (this.hasHeader) {
        this.parseWithHeader();
      } else {
        this.parseWithoutHeader();
      }
    },
    parseWithoutHeader() {
      const rows = this.text.split("\n").filter((row) => row.length > 0);
      const data = rows.map((item) => {
        return item.split("\t");
      });

      this.json = data;
    },
    parseWithHeader() {
      const rows = this.text.split("\n").filter((row) => row.length > 0);

      let data = rows.map((item) => {
        return item.split("\t");
      });

      const columns = data[0];
      data.splice(0, 1);

      const dataObject = [];

      for (const index in data) {
        const obj = {};
        for (const colIndex in columns) {
          obj[this.camelize(columns[colIndex])] = data[index][colIndex];
        }

        dataObject.push(obj);
      }

      this.json = dataObject;
    },
    camelize(str) {
      return str
        .replace(/(?:^\w|[A-Z]|\b\w)/g, function (word, index) {
          return index === 0 ? word.toLowerCase() : word.toUpperCase();
        })
        .replace(/\s+/g, "");
    },
  },
});
</script>

<style scoped>
.dynamic-height {
  height: calc(100vh - 378px);
}
.text-right {
  text-align: right;
}
</style>
