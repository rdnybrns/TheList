<template>
  <v-list v-if="items.length > 0" class="pa-0">
    <template v-for="item in items">
      <v-hover :key="item.id">
        <v-list-tile
          slot-scope="{ hover }"
          :class="`elevation-${hover ? 2 : 0}`"
        >
          <v-list-tile-action>
            <v-checkbox
              @change="itemCheckChange(item, $event)"
              v-model="item.isChecked"
            ></v-checkbox>
          </v-list-tile-action>
          <template v-if="editing === item.id">
            <v-text-field
              autofocus
              v-model="item.description"
              v-on:keyup.enter="editing = null"
              v-on:blur="editing = null"
            ></v-text-field>
          </template>
          <template v-else>
            <v-list-tile-content @click="editMode(item.id)">
              <v-list-tile-title
                v-text="item.description"
                :class="{ checked: item.isChecked }"
              ></v-list-tile-title>
            </v-list-tile-content>
          </template>
          <template v-if="hover">
            <v-hover>
              <v-btn
                fab
                flat
                small
                slot-scope="{ hover }"
                :class="`elevation-${hover ? 5 : 0}`"
                ripple
                @click="$emit('delete:item', item.id)"
              >
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </v-hover>
          </template>
        </v-list-tile>
      </v-hover>
    </template>
  </v-list>
</template>

<script>
export default {
  name: 'items',
  props: {
    items: Array,
  },
  data() {
    return {
      editing: null,
    }
  },
  methods: {
    isItemValid(item) {
      return item.description.trim().length > 0 && item.description !== ''
    },
    editMode(id) {
      this.editing = id
    },
    editItem(item) {
      if (!this.isItemValid(item)) return
      this.$emit('edit:item', item.id, item)
      this.editing = null
    },
    itemCheckChange(item, event) {
      item.isChecked = event
      this.editItem(item)
    },
  },
}
</script>
<style scoped>
.checked {
  text-decoration: line-through;
  color: lightgray;
}
</style>
