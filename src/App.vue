<template>
  <div>    
    <div>
      <Modal 
        v-if="showModal" 
        @close="showModal=false" 
        @usersList="(users) => userList = [... users]"
      />

      <div class="users">
          <User 
            v-for="(user, i) in userList" :key="i" class="users__item"
            :name="user.name" 
            :positions="user.pos" 
            :total="user.total" 
            @setTotal="(setTotal) => user.total = setTotal"
          />
          <span>Общая сумма заказа: {{ userList.length ? userList.map(item => item.total).reduce((sum, current) => sum + current) : 0 }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { reactive, ref, computed } from "vue";
  import Modal from "./components/Modal.vue";
  import User from "./components/User.vue";

  const showModal = ref(true)
  const userList = ref([])

</script>

<style scoped lang="scss">
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
.users {
  display: flex;
  flex-direction: column;
  gap: 20px;
  
  &__item {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-bottom: 20px;
    gap: 10px;
    
    border-bottom: 2px solid #fff;
  }
}
</style>
