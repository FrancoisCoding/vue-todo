<template>
  <div>
    <form @submit="addTodo">
      <input type="text" v-model="title" name="title" placeholder="Add Todo..." required />
      <input type="text" v-model="description" name="description" placeholder="Add Description..." />
      <input type="submit" class="btn" />
    </form>
  </div>
</template>

<script>
import uuid from "uuid";

export default {
  name: "AddTodo",
  data() {
    return {
      title: "",
      description: ""
    };
  },
  methods: {
    addTodo(e) {
      e.preventDefault();
      const newTodo = {
        id: uuid.v4(),
        title: this.title,
        description: this.description,
        completed: false
      };
      // Send up to parent
      this.$emit("add-todo", newTodo);

      // Clear form
      this.title = "";
      this.description = "";
    }
  }
};
</script>

<style scoped>
form {
  display: flex;
}

input[type="text"] {
  flex: 10;
  padding: 5px;
}

input[type="submit"] {
  flex: 2;
}
</style>
