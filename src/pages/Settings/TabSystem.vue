<template>
  <div>
    <TwoColumnForm
      v-if="doc"
      :doc="doc"
      :fields="fields"
      :autosave="true"
      :emit-change="true"
      @change="forwardChangeEvent"
    />
    <div class="flex flex-row justify-between my-4">
      <LanguageSelector class="text-sm" input-class="px-4 py-1.5"/>
      <button
        class="text-gray-900 text-sm hover:bg-gray-200 rounded-md px-4 py-1.5"
        @click="checkForUpdates(true)"
      >
        Check for Updates
      </button>
    </div>
  </div>
</template>

<script>
import frappe from 'frappe';
import TwoColumnForm from '@/components/TwoColumnForm';
import { checkForUpdates } from '@/utils';
import LanguageSelector from '@/components/Controls/LanguageSelector.vue';

export default {
  name: 'TabSystem',
  components: {
    TwoColumnForm,
    LanguageSelector,
  },
  emits: ['change'],
  data() {
    return {
      doc: null,
    };
  },
  async mounted() {
    this.doc = frappe.SystemSettings;
    this.companyName = frappe.AccountingSettings.companyName;
  },
  computed: {
    fields() {
      let meta = frappe.getMeta('SystemSettings');
      return meta.getQuickEditFields();
    },
  },
  methods: {
    checkForUpdates,
    forwardChangeEvent(...args) {
      this.$emit('change', ...args);
    },
  },
};
</script>
