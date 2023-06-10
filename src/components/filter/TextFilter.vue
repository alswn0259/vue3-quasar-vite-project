<template>
  <div>
    <input ref="inputRef" v-model="text" />
  </div>
</template>

<!-- <script setup lang="ts">
import { ref, toRef, watch } from 'vue';
import { IFilterParams } from 'ag-grid-community';

const props = defineProps({ params: Object });
const params = toRef(props, 'params');
debugger;
const inputRef = ref();
const textFilter = ref();

function isFilterActive() {
  return (
    textFilter.value !== null &&
    textFilter.value !== undefined &&
    textFilter.value !== ''
  );
}

function doesFilterPass(params: any) {
  const { api, colDef, column, columnApi, context, valueGetter } = params.value;
  const { node } = params;
  const value = valueGetter({
    api,
    colDef,
    column,
    columnApi,
    context,
    data: node.data,
    getValue: (field: any) => node.data[field],
    node,
  })
    .toString()
    .toLowerCase();

  return (
    !textFilter.value ||
    textFilter.value
      .toLowerCase()
      .split(' ')
      .every((filterWord: any) => {
        return value.indexOf(filterWord) >= 0;
      })
  );
}

function getModel() {
  if (!isFilterActive()) {
    return null;
  }

  return { value: textFilter.value };
}

function setModel(model: any) {
  textFilter.value = model == null ? null : model.value;
}

function afterGuiAttached() {
  inputRef.value.focus();
}

function componentMethod(message: any) {
  alert(`Alert from PartialMatchFilterComponent ${message}`);
}

watch(textFilter, (val: any, oldVal: any) => {
  if (val !== oldVal) {
    params.value?.filterChangedCallback();
  }
});

defineExpose({
  isFilterActive,
});
</script> -->

<script>
export default {
  data() {
    return {
      text: '',
    };
  },
  methods: {
    isFilterActive() {
      return this.text !== null && this.text !== undefined && this.text !== '';
    },

    doesFilterPass(params) {
      const { api, colDef, column, columnApi, context, valueGetter } =
        this.params;
      const { node } = params;
      const value = valueGetter({
        api,
        colDef,
        column,
        columnApi,
        context,
        data: node.data,
        getValue: (field) => node.data[field],
        node,
      })
        .toString()
        .toLowerCase();

      return (
        !this.text ||
        this.text
          .toLowerCase()
          .split(' ')
          .every((filterWord) => {
            return value.indexOf(filterWord) >= 0;
          })
      );
    },

    getModel() {
      if (!this.isFilterActive()) {
        return null;
      }

      return { value: this.text };
    },

    setModel(model) {
      this.text = model == null ? null : model.value;
    },

    afterGuiAttached() {
      this.$refs.inputRef.focus();
    },

    componentMethod(message) {
      alert(`Alert from PartialMatchFilterComponent ${message}`);
    },
  },
  watch: {
    text: function (val, oldVal) {
      if (val !== oldVal) {
        this.params.filterChangedCallback();
      }
    },
  },
};
</script>
<style scoped></style>
