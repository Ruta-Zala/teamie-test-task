<template>
  <div class="flex justify-center rounded flex-col">
    <label for="">Sort By</label>
    <div class="flex-col sm:flex-row inline-flex rounded-md shadow-sm" role="group">
      <button v-for="(section, index) in sections" :key="index" :class="{ 'bg-green-700 text-white border-green-700' : currentIndex === index }"
              :value="section"
              @click="fetch(section,index)" type="button"
              class="py-2 px-4 text-sm font-medium text-gray-900 bg-white border border-gray-200
              hover:bg-green-900 hover:text-white hover:border-green-700 focus:outline-none
             flex items-center">
          <span class="flex-1">{{ capitalize(section) }}</span>
          <div v-if="currentIndex === index" class="ml-2">
              <span v-if="desc">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                     class="bi bi-arrow-up" viewBox="0 0 16 16">
                    <path fill-rule="evenodd"
                        d="M8 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L7.5 2.707V14.5a.5.5 0 0 0 .5.5z"/>
                </svg>
              </span>
              <span v-else>
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-arrow-down" viewBox="0 0 16 16">
                    <path fill-rule="evenodd"
                        d="M8 1a.5.5 0 0 1 .5.5v11.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L7.5 13.293V1.5A.5.5 0 0 1 8 1z"/>
                </svg>
              </span>
          </div>
      </button>
    </div>
  </div>
</template>

<script>
import {computed} from "vue"
import sectionsData from "./sections"

export default {
  props: {
    modelValue: String,
    fetch: Function,
    isActive: Boolean,
    desc: Boolean,
    currentIndex: Number,
  },
  setup(props, {emit}) {
    const section = computed({
      get: () => props.modelValue,
      set: value => emit("update:modelValue", value),
    });

    return {
      section,
    }
  },
  data() {
    return {
      sections: sectionsData,
    }
  },
  methods: {
    capitalize(value) {
      if (!value) return "";
      value = value.toString().replace('_', " ");
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
  },
}
</script>
