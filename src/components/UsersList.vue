<template>
  <div>
      <transition-group tag="ul" name="user-list">
        <li v-for="user in users" :key="user" @click="removeUser(user)">
          {{ user }}
        </li>
      </transition-group>
    <p v-if="users.length === 0">On this moment no have users</p>
    <div>
      <input type="text" ref="userNameInput" />
      <button @click="addUser">Add User</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: ['Max', 'Mendy', 'Hulk', 'Bruse'],
    };
  },
  methods: {
    removeUser(user) {
      this.users = this.users.filter((usr) => usr !== user);
    },
    addUser() {
      const enteredUserName = this.$refs.userNameInput.value.trim();
      if (enteredUserName === '') return false;
      this.users.unshift(enteredUserName);
    },
  },
};
</script>
<style scoped>
ul {
  list-style: none;
}
li {
  cursor: pointer;
  text-align: center;
  border: 1px solid #ccc;
  padding: 10px;
}
input {
  margin-right: 20px;
}
.user-list-enter-from,
.user-list-leave-to{
  opacity: 0;
  transform: translateX(-30px);
}
.user-list-enter-active{
  transition: all 1s ease-out;
}
.user-list-enter-to,
.user-list-leave-from{
    opacity: 1;
  transform: translateX(0);
}
.user-list-leave-active {
  /* position: absolute; */
  transition: all 1s ease-in;
}
.user-list-move {
  transition: transform 0.8s ease;
}
</style>