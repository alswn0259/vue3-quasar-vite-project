<template>
  <div style="padding: 4px; width: 200px">
    <div style="font-weight: bold">Custom Athlete Filter</div>
    <div>
      <input
        style="margin: 4px 0 4px 0"
        type="text"
        v-model="filterText"
        v-on:keyup="updateFilter($event)"
        placeholder="Full name search..."
      />
    </div>
    <div style="margin-top: 20px">
      This filter does partial word search on multiple words, eg "mich phel"
      still brings back Michael Phelps.
    </div>
    <div style="margin-top: 20px">
      Just to emphasise that anything can go in here, here is an image!!
    </div>
    <div>
      <img
        src="https://www.ag-grid.com/images/ag-Grid2-200.png"
        style="
          width: 150px;
          text-align: center;
          padding: 10px;
          margin: 10px;
          border: 1px solid lightgrey;
        "
      />
    </div>
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
  data: function () {
    return {
      filterText: null,
    };
  },
  methods: {
    updateFilter() {
      this.params.filterChangedCallback();
    },

    doesFilterPass(params) {
      const { api, colDef, column, columnApi, context } = this.params;
      const { node } = params;

      // make sure each word passes separately, ie search for firstname, lastname
      let passed = true;
      this.filterText
        .toLowerCase()
        .split(' ')
        .forEach((filterWord) => {
          const value = this.params.valueGetter({
            api,
            colDef,
            column,
            columnApi,
            context,
            data: node.data,
            getValue: (field) => node.data[field],
            node,
          });

          if (value.toString().toLowerCase().indexOf(filterWord) < 0) {
            passed = false;
          }
        });

      return passed;
    },

    isFilterActive() {
      return this.filterText != null && this.filterText !== '';
    },

    getModel() {
      if (!this.isFilterActive()) {
        return null;
      }

      return { value: this.filterText };
    },

    setModel(model) {
      this.filterText = model == null ? null : model.value;
    },
  },
};
</script>
<style scoped></style>
