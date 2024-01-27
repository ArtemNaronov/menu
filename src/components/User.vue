<template>
    <div>
        <span>{{ name }}</span>
        <div class="positions" v-for="(pos, i) in positions" :key="i">
            <input type="text" v-model="pos.name" placeholder="Название блюда">
            <input type="number" v-model="pos.price" placeholder="Стоимость блюда">
            <button @click="removeItem(i)">
                <svg xmlns="http://www.w3.org/2000/svg" width="800px" height="800px" viewBox="0 0 24 24" fill="none">
                    <path d="M9.1709 4C9.58273 2.83481 10.694 2 12.0002 2C13.3064 2 14.4177 2.83481 14.8295 4" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
                    <path d="M20.5001 6H3.5" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
                    <path d="M18.8332 8.5L18.3732 15.3991C18.1962 18.054 18.1077 19.3815 17.2427 20.1907C16.3777 21 15.0473 21 12.3865 21H11.6132C8.95235 21 7.62195 21 6.75694 20.1907C5.89194 19.3815 5.80344 18.054 5.62644 15.3991L5.1665 8.5" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
                    <path d="M9.5 11L10 16" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
                    <path d="M14.5 11L14 16" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
                </svg>
            </button>
        </div>
        <div>Общая сумма на человека: {{ total }}</div>
        <button @click="addPos()">Добавить позицию</button>
    </div>
</template>

<script setup>
import { reactive, ref, watch } from "vue"

    const props = defineProps({
        name: String,
        positions: Array,
        total: Number
    })  
    const emits = defineEmits(["setTotal"]);
    const posList = reactive(props.positions)
   
    watch(posList, (newValue) => {
        let price = newValue.map(item => item.price).reduce((sum, current) => sum + current); 
        setTotal(price);
    })

    function setTotal(price) {
        emits('setTotal', price)
    }

    function addPos() {
        posList.push(
            {
                name: "",
                price: 0
            }
        )
    }

    function removeItem(i) {
        posList.splice(i, 1)
    }

</script>

<style scoped lang="scss">
    span {
        font-weight: 700;
        font-size: 24px;
    }

    svg {
        width: 24px;
        height: auto;
    }

    input,
    button {
        height: 35px;
    }

    .positions {
        display: flex;
        align-items: center;
        gap: 10px;

        [type="text"] {
            width: 75%;
        }

        [type="number"] {
            width: 25%;
        }
    }
</style>