<template>
  <v-text-field
    solo
    autofocus
    label="Type here"
    v-model="item.description"
    v-on:keyup.enter="enterItem"
  ></v-text-field>
</template>

<script>
export default {
  name: 'ListEntry',
  data() {
    return {
      item: {
        description: '',
      },
    }
  },
  methods: {
    isItemValid() {
      return (
        this.item.description.trim().length > 0 && this.item.description !== ''
      )
    },
    enterItem() {
      if (this.isItemValid()) {
        this.$emit('add:item', this.item)
      }
      this.item = { description: '' }
    },
  },
}
</script>
