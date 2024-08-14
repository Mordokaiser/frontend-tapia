<template>
    <div class="">
        <h1>Xiaomi phones</h1>
        <p>Sistema gestor de teléfonos en stock</p>
        <!-- opciones del crud -->
        <div class="grid grid-cols-5">
            <div class="grid-row">
                <label for="model">modelo</label>
                <select id="model" name="model" v-model="modelRef">
                    <option value="Redmi_Note_10">Redmi Note 10</option>
                    <option value="Redmi_Note10_Pro">Redmi Note 10 Pro</option>
                    <option value="Redmi_Note_10_Pro_Max">Redmi Note 10 Pro Max</option>
                    <option value="Redmi_Note_10_5G">Redmi Note 10 5G</option>
                </select>
            </div>
            <div>
                <label>color</label>
                <select id="color" name="color" v-model="colorRef">
                    <option value="gray">Onyx Gray</option>
                    <option value="white">Pebble White</option>
                    <option value="green">Lake Green</option>
                    <option value="gold">Champagne Gold</option>
                </select>
            </div>
            <div>
                <label for="price">precio</label>
                <input type="number" id="price" name="price" min="0" max="9999" v-model.number="priceRef">
            </div>
            <div>
                <label for="stock">stock</label>
                <input type="number" id="stock" name="stock" min="0" max="9999" v-model.number="stockRef">
            </div>
            <div>
                <label>tienda</label>
                <select id="store" name="store" v-model="storeRef">
                    <option value="abcdin">abcdin</option>
                    <option value="paris">paris</option>
                    <option value="falabella">falabella</option>
                    <option value="lider">lider</option>
                </select>
            </div>
        </div>
        <div class="flex w-full justify-center mt-4 mb-2">
            <button
                class="bg-sky-600 rounded-md px-4 py-1 flex items-center uppercase hover:ring-4 hover:ring-green-400"
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

const addPhone = async () => {
    console.log('Agregando teléfono...');

    // Datos del teléfono que se enviarán
    const phoneData = {
        model: modelRef.value,
        color: colorRef.value,
        price: priceRef.value,
        stock: stockRef.value,
        store: storeRef.value
    };

    // Convertir los datos a una cadena de consulta
    const queryString = new URLSearchParams(phoneData).toString();

    const base_url = 'http://app.backend-tapia';
    try {
        // Construir la URL completa con los parámetros de consulta
        const url = `${base_url}/add-phones?${queryString}`;

        // Envío de los datos al servidor usando fetch
        const response = await fetch(url, {
            method: 'GET', // Método GET
            headers: {
                'Content-Type': 'application/json'
            }
        });

        // Verificar si la respuesta es exitosa
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        // Parsear la respuesta JSON
        const data = await response.json();
        console.log('Teléfono agregado exitosamente:', data);
    } catch (error) {
        console.error('Error al agregar el teléfono:', error);
    }
};

</script>
