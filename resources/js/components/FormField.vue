<template>
  <DefaultField
    :field="currentField"
    :errors="errors"
    :show-help-text="showHelpText"
    :full-width-content="fullWidthContent"
  >
    <template #field>
      <Vue3Signature :sigOption="{penColor: color, backgroundColor: bgColor}" :w="width" :h="height"
                     :disabled="disabled"  ref="signature" class="form-input-bordered rounded" @begin="onBegin" @end="onEnd" />

      <div class="flex flex-row align-middle md:items-center justify-center md:justify-end space-x-1 space-y-0 md:space-x-3 my-2">
        <button type="button"
                class="shadow bg-primary-500 hover:bg-primary-400 text-white dark:text-gray-900 cursor-pointer rounded text-sm inline-block font-bold w-1/2 md:w-32 h-9 px-3"
                @click="undo">
          <component
            :is="`heroicons-outline-reply`"
            height="18"
            width="18"
            class="inline"
          /> {{__('novaSignature.undo') }}
        </button>
        <button type="button"
                class="shadow bg-primary-500 hover:bg-primary-400 text-white dark:text-gray-900 cursor-pointer rounded text-sm inline-block font-bold w-1/2 md:w-32 h-9 px-3"
                @click="clear">
          <component
            :is="`heroicons-outline-x`"
            height="18"
            width="18"
            class="inline"
          /> {{__('novaSignature.clear') }}
        </button>
      </div>

      <textarea
        :id="currentField.attribute"
        type="text"
        class="hidden"
        v-model="value"
        rows="10"
      >
        {{value}}
      </textarea>

    </template>
  </DefaultField>
</template>

<script>
import { DependentFormField, HandlesValidationErrors } from 'laravel-nova'

export default {
  mixins: [DependentFormField, HandlesValidationErrors],
  props: ['resourceName', 'resourceId', 'field'],

  watch: {
    currentlyIsVisible(current, previous) {
      if (current === true && previous === false) {
        this.$nextTick(() => this.handleShow())
      } else if (current === false && previous === true) {}
    },
  },

  data() {
    return {
      height: '300px',
      width: '100%',
      color: 'black',
      bgColor: 'white',
      disabled: false,
      format: 'image/png'
    }
  },

  beforeMount() {
    this.width = this.field.width || this.width;
    this.height = this.field.height || this.height;
    this.color = this.field.color || this.color;
    this.bgColor = this.field.bgColor || this.bgColor;
    this.format = this.field.format || this.format;
  },

  mounted() {
    this.$nextTick().then(() => {
      if (this.currentField.value && this.currentField.visible) {
        this.value = this.currentField.value;
        this.$refs.signature?.fromDataURL(this.value);
      }
    });
  },

  methods: {
    handleShow() {
      this.value = this.currentField.value;
      this.$refs.signature.fromDataURL(this.value);
    },

    clear() {
      this.$refs.signature.clear()
      this.value = null
    },

    undo() {
      this.$refs.signature.undo()
      this.save();
    },

    onBegin() {
    },

    onEnd() {
      this.save();
    },

    save() {
      if (this.$refs.signature.isEmpty()) {
        this.value = null;
      } else {
        this.value = this.$refs.signature.save(this.format);
      }
    },

    /*
     * Set the initial, internal value for the field.
     */
    setInitialValue() {
      this.value = this.currentField.value || '';
    },

    /**
     * Fill the given FormData object with the field's internal value.
     */
    fill(formData) {
      this.fillIfVisible(formData, this.fieldAttribute, this.value || '')
    },
  },
}
</script>
