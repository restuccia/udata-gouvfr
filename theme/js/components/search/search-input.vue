<!--
---
name: Input
category: Suggest
---

# Search input

It's an input that calls the passed `onChange` function on each change.
You can also pass it a `value` prop that will populate the field.
-->

<template>
  <section class="search-input">
    <form action="" @submit.prevent="onSubmit" class="row-inline">
      <span class="icon" :class="{ active: queryString }" v-html="icon" />
      <input
        type="text"
        :value="queryString"
        @input="_onChange"
        @keydown.delete="onDelete"
        ref="input"
        :aria-label="placeholder || $t('Search...')"
        :placeholder="placeholder || $t('Search...')"
      />
      <span
        class="close-icon ml-sm self-center"
        @click.prevent="stop"
        v-html="closeIcon"
        v-if="stop"
      />
    </form>
  </section>
</template>

<script>
import Icon from "svg/search.svg";
import CloseIcon from "svg/close.svg";

export default {
  created() {
    this.icon = Icon;
    this.closeIcon = CloseIcon;
    this.queryString = this.value;
  },
  watch: {
    value: function (val) {
      this.queryString = val;
    },
  },
  async mounted() {
    //this.$nextTick doesn't work because of browsers®
    setTimeout(() => this.focus(), 100);
  },
  props: {
    onChange: Function,
    stop: Function,
    value: String,
    placeholder: String,
  },
  data() {
    return {
      queryString: "",
    };
  },
  methods: {
    _onChange(e) {
      if (this.onChange) this.onChange(e.target.value);
    },
    onDelete() {
      if (this.queryString === "") this.onChange(this.queryString);
    },
    focus() {
      this.$refs.input.focus({
        preventScroll: true,
      });
    },
  },
};
</script>
