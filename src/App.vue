<template>
  <v-app :dark="goDark">
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">THE</span>
        <span>LIST</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-dialog v-model="settings" max-width="300px">
        <template v-slot:activator="{ on }">
          <v-btn flat fab v-on="on">
            <v-icon>mdi-settings</v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-card-title>Settings</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-switch v-model="goDark" :label="`Dark Theme`"></v-switch>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn flat @click="settings = false">Ok</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>
    <v-container>
      <v-layout>
        <v-flex>
          <v-content>
            <ListEntry @add:item="addItem"></ListEntry>
            <Items
              :items="items"
              @delete:item="deleteItem"
              @edit:item="editItem"
            ></Items>
          </v-content>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import ListEntry from '@/components/ListEntry.vue'
import Items from '@/components/Items.vue'
export default {
  name: 'App',
  components: {
    ListEntry,
    Items,
  },
  data() {
    return {
      items: [],
      goDark: this.goDark,
      settings: false,
    }
  },
  watch: {
    goDark(value) {
      localStorage.goDark = value
    },
  },
  mounted() {
    if (localStorage.getItem('items')) {
      try {
        this.items = JSON.parse(localStorage.getItem('items'))
      } catch (e) {
        localStorage.removeItem('items')
      }
    }
    if (localStorage.goDark) {
      try {
        this.goDark = JSON.parse(localStorage.goDark)
      } catch (e) {
        localStorage.remove('goDark')
      }
    }
  },
  methods: {
    addItem(item) {
      const lastId =
        this.items.length > 0 ? this.items[this.items.length - 1].id : 0
      const id = lastId + 1
      const newItem = { ...item, id }
      this.items = [...this.items, newItem]
      this.updateLocalStorage()
    },
    deleteItem(id) {
      this.items = this.items.filter(item => item.id !== id)
      this.updateLocalStorage()
    },
    editItem(id, updatedItem) {
      this.items = this.items.map(item => (item.id === id ? updatedItem : item))
      this.updateLocalStorage()
    },
    updateLocalStorage() {
      localStorage.setItem('items', JSON.stringify(this.items))
    },
  },
}
</script>
<style lang="css" scoped></style>
