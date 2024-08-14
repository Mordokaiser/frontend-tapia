<template>
    <div class=" bg-slate-100 rounded-md px-4 py-4 mb-8 shadow-sm font-mono">
        

        <p class="mb-8">Sistema gestor de teléfonos en stock, este sistema esta pensado para que pueda mantener
            actualizado la informacion de stock que tiene en sus tiendas.</p>
        <!-- opciones del crud -->
        <div class="grid md:grid-cols-5">
            <div>
                <label for="model">Modelo: </label>
                <select id="model" name="model" v-model="modelRef" class="rounded-md border whitespace-nowrap max-w-20 lg:max-20 xl2:max-w-40">
                    <option value="Redmi_Note_10">Redmi Note 10</option>
                    <option value="Redmi_Note_10_Pro">Redmi Note 10 Pro</option>
                    <option value="Redmi_Note_10_Pro_Max">Redmi Note 10 Pro Max</option>
                    <option value="Redmi_Note_10_5G">Redmi Note 10 5G</option>
                </select>
            </div>
            <div>
                <label>Color: </label>
                <select id="color" name="color" v-model="colorRef" class="rounded-md borde max-w-20 lg:max-20 xl2:max-w-40 whitespace-nowrap ">
                    <option value="gray">Onyx Gray</option>
                    <option value="white">Pebble White</option>
                    <option value="green">Lake Green</option>
                    <option value="gold">Champagne Gold</option>
                </select>
            </div>
            <div>
                <label for="price">Precio: </label>
                <input type="number" id="price" name="price" min="0" max="9999" v-model.number="priceRef" class="rounded-md borde">
            </div>
            <div>
                <label for="stock">Stock: </label>
                <input type="number" id="stock" name="stock" min="0" max="9999" v-model.number="stockRef" class="rounded-md borde">
            </div>
            <div >
                <label>Tienda: </label>
                <select id="store" name="store" v-model="storeRef" class="rounded-md borde max-w-20 lg:max-20 xl2:max-w-40 whitespace-nowrap ">
                    <option value="abcdin">abcdin</option>
                    <option value="paris">paris</option>
                    <option value="falabella">falabella</option>
                    <option value="lider">lider</option>
                </select>
            </div>
        </div>
        <div class="flex w-full justify-center mt-4 mb-2">
            <button
                class="bg-slate-100 border border-orange-600 rounded-md px-4 py-1 flex items-center uppercase hover:shadow-sm hover:shadow-orange-600"
                @click="addPhone">agregar</button>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';

const modelRef = ref('');
const colorRef = ref('');
const priceRef = ref(990);
const stockRef = ref(10);
const storeRef = ref('');

const base_url = 'http://app.backend-tapia/api';
const addPhone = async () => {
    //enviar por axios
    const phoneData = new FormData();
    phoneData.append('model', modelRef.value);
    phoneData.append('color', colorRef.value);
    phoneData.append('price', priceRef.value);
    phoneData.append('stock', stockRef.value);
    phoneData.append('store', storeRef.value);

    try {
        const response = await axios.post(`${base_url}/add-phones`, phoneData, {
            headers: {
                'Content-Type': 'multipart/form-data'
            }
        });
        console.log('Teléfono agregado exitosamente:', response.data);
    } catch (error) {
        console.error('Error al agregar el teléfono:', error);
    }

}
</script>
<style scoped></style>
