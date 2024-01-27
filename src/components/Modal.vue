<template>
    <div class="modal">
        <div class="modal__content">
            {{ users.length == 0 ? 'Укажите количество гостей' : 'Укажителей гостей' }}
            <div class="input__group" v-if="users.length == 0">
                <input type="number" v-model="usersCount" min="0">
                <button @click="setUsersCount()">ОК</button>
            </div>      
            <div class="modal__content__users" v-if="users.length">
                <div v-for="(user, index) in users" :key="index">                 
                    <input 
                        type="text" 
                        placeholder="Имя гостя" 
                        v-model="user.name"
                    >
                </div>
            </div>
            <button type="submit" @click="submitUsers()" @submit="submitUsers()">Подтвердить</button>

            <div class="modal__content__close">
                <button @click="$emit('close')">X</button>
            </div>     
            <div class="modal__content__back" v-if="users.length != 0">
                <button @click="users.length = 0">←</button>
            </div>      
        </div>        
       
    </div>
</template>

<script setup>
  import { reactive, ref } from "vue"
  
    defineProps({
        title: String
    })    
    const emits = defineEmits(["usersList", "close"]);
    const usersCount = ref(0)
    
    const users = reactive([])

    function setUsersCount() {
        for (let i = 0; i < usersCount.value; i++) {
            users.push({
                name: `Гость ${i+1}`, 
                pos: [],
                total: 0
            })          
        }
    }

    function submitUsers() {
        emits('usersList', users)
        emits('close')
    }

</script>

<style lang="scss" scoped>
    .modal {
        position: fixed;
        top: 0;
        left: 0;
        background: rgba($color: #000, $alpha: .5);
        width: 100%;
        height: 100%;

        &__content {
            position: absolute;
            border-radius: 30px;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            padding: 75px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            gap: 15px;
            background: #000;
            width: 50%;
            min-height: 50%;
            max-height: 85%;

            @media (max-width: 768px) {
                width: 90%;
                padding: 75px 0;
            }

            &__users {
                display: flex;
                flex-direction: column;
                gap: 20px;
                overflow: auto;
                width: 50%;
                align-items: center;

                @media (max-width: 768px) {
                    width: 90%;
                }
            }

            &__close {
                position: absolute;
                right: 15px;
                top: 15px;
            }

            &__back {
                position: absolute;
                left: 15px;
                top: 15px;
                display: flex;
                align-items: center;
                justify-content: center;
            }

            .input__group {
                display: flex;
                align-items: center;
                gap: 15px;
            }
        }
    }
</style>