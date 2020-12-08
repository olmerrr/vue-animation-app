<template>
  <ul>
    <li v-for="user in users" :key="user" @click="removeUser(user)">
      {{ user }}
    </li>
  </ul>
  <div>
    <input type="text" ref="userNameInput"/>
    <button @click="addUser">Add User</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: ['Den','Marina', 'Sam', 'Kristina', 'Nastya'],

    };
  },
  methods: {
    removeUser(user) {
      this.users = this.users.filter((usr) =>usr !== user);
      console.log('del', user)
    },
    addUser() {
      const enteredUserName = this.$refs.userNameInput.value;
      this.users.unshift(enteredUserName);
      console.log('add user')
    },
  },
}
</script>
<style scoped>
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  li {
    border: 1px solid #ccc;
    padding: 1rem;
    text-align: center;
  }
  input {
    padding: 5px;
    margin-right: 20px;
  }
</style>