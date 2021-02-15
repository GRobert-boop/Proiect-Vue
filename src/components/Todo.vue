<template>
  <li class="d-flex align-items-center list-group-item">
    <button
        class="btn border-0 flex-grow-1 text-left shadow-none"
        :class="{ completed }"
        @click="$emit('on-toggle')"
        v-if="!isEditing"
    >
      <span>{{ description }}</span>
    </button>
    <form v-else class="flex-grow-1" @submit.prevent="finishEditing()">
      <input
          type="text"
          class="form-control"
          v-model="newTodoDescription"
          @blur="finishEditing()"
          ref="newTodo"
      />
    </form>
    <v-menu offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
        >
          <v-icon>fa-list</v-icon>
        </v-btn>
      </template>
      <v-list>
        <v-list-item
            v-for="(item, index) in items"
            :key="index"

        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    <button
        @click="startEditing()"
        class="btn btn-outline-primary border-0 ml-2"
    >
      <span class="fa fa-edit"></span>
    </button>
    <button @click="$emit('on-delete')" class="btn btn-outline-danger border-0">
      <span class="fa fa-trash"></span>
    </button>
  </li>
</template>
<script>
export default {
  data() {
    return {
      autoselectMenu: false,
      isEditing: false,
      newTodoDescription: "",
      items: [
        { title: 'Click Me' },
        { title: 'Click Me' },
        { title: 'Click Me' },
        { title: 'Click Me 2' },
      ],
    };
  },
  props: {
    description: String,
    completed: Boolean
  },
  methods: {
    startEditing() {
      if (this.isEditing) {
        this.finishEditing();
      } else {
        this.newTodoDescription = this.description;
        this.isEditing = true;
        this.$nextTick(() => this.$refs.newTodo.focus());
      }
    },
    finishEditing() {
      this.isEditing = false;
      this.$emit("on-edit", this.newTodoDescription);
    },
    toggle() {
      this.autoselectMenu = !this.autoselectMenu
    }
  }
};
</script>

<style lang="scss" scoped>
.completed {
  text-decoration: line-through;
}
</style>