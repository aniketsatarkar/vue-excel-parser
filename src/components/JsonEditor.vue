<template>
  <div v-if="code !== 'null'">
    <CodeEditor
      :value="code"
      :read_only="true"
      :language_selector="false"
      :wrap_code="true"
      :display_language="false"
      height="400px"
      width="100%"
      border_radius="0px"
    />
  </div>
  <div v-else>Nothing has been parsed...</div>
</template>

<script>
import { defineComponent } from "vue";
import CodeEditor from "simple-code-editor";

export default defineComponent({
  components: {
    CodeEditor,
  },
  props: {
    code: {
      default: "",
      type: String,
    },
  },
  setup() {},
});
</script>
