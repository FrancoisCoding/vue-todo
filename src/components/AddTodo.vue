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
import firebase from "firebase";

// Your web app's Firebase configuration
var firebaseConfig = {
  apiKey: "AIzaSyCV35nsPS3WvMoyp6BYyrq9hhq4pGhheig",
  authDomain: "francoisgamescontact.firebaseapp.com",
  databaseURL: "https://francoisgamescontact.firebaseio.com",
  projectId: "francoisgamescontact",
  storageBucket: "",
  messagingSenderId: "1037851501346",
  appId: "1:1037851501346:web:42882627ea226811"
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

var messagesRef = firebase.database().ref("vue todos");

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

      var newMessagesRef = messagesRef.push();
      newMessagesRef.set({
        todos: {
          title: this.title,
          description: this.description
        }
      });

      // Clear form
      this.title = "";
      this.description = "";
    },

    saveMessage(title, description) {
      var newMessageRef = messagesRef.push();
      newMessageRef.set({
        title: this.title,
        description: this.description
      });
      console.log("newMessageRef", newMessageRef);
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
