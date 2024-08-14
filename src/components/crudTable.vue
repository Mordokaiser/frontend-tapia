<template>
    <div class="relative overflow-x-auto shadow-md sm:rounded-lg ">
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 ">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 ">
                <tr>
                    <th scope="col" class="px-6 py-3">
                        Modelo
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Color
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Precio
                    </th>
                    <th scope="col" class="px-6 py-3">
                        stock
                    </th>
                    <th scope="col" class="px-6 py-3">
                        tienda
                    </th>
                    <th scope="col" class="px-6 py-3">
                        acciones
                    </th>
                </tr>
            </thead>
            <tbody>
                <!-- recorrer  y crear filas-->
                <tr v-for="phone in phonesList" :key="phone.id" class="odd:bg-white even:bg-gray-50 border-b ">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap ">
                        {{ phone.model }}
                    </th>
                    <td class="px-6 py-4">{{ phone.color }}</td>
                    <td class="px-6 py-4">{{ phone.price }}</td>
                    <td class="px-6 py-4">{{ phone.stock }}</td>
                    <td class="px-6 py-4">{{ phone.store }}</td>
                    <td class="flex px-6 py-4 gap-2">
                        <button class="font-medium text-blue-600 hover:underline" @click="openEditModal(phone)">Edit</button>
                        <button class="font-medium text-red-600 hover:underline" @click="deletePhone(phone.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <!--modal de edicion-->
    <div class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center" :class="{ 'hidden': !activeEdit }" id="modal">
        <div class="bg-white p-8 rounded-md shadow-md">
            <h2 class="text-2xl font-bold mb-4">Editar telefono</h2>
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
            <div>
                <label>Tienda: </label>
                <select id="store" name="store" v-model="storeRef" class="rounded-md borde max-w-20 lg:max-20 xl2:max-w-40 whitespace-nowrap ">
                    <option value="abcdin">abcdin</option>
                    <option value="paris">paris</option>
                    <option value="falabella">falabella</option>
                    <option value="lider">lider</option>
                </select>
            </div>
            <div class="flex w-full justify-center mt-4 mb-2">
                <button class="bg-slate-100 border border-orange-600 rounded-md px-4 py-1 flex items-center uppercase hover:shadow-sm hover:shadow-orange-600" @click="editPhone">editar</button>
                <button class="bg-slate-100 border border-red-600 rounded-md px-4 py-1 flex items-center uppercase hover:shadow-sm hover:shadow-red-600 ml-2" @click="closeEditModal">cancelar</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

// Variables que contendrán los teléfonos
const phonesList = ref([]);

// Variables para activar el modal de edición
const activeEdit = ref(false);

// Variable para almacenar el ID del teléfono actual
const currentPhoneId = ref(null);

// Variables para los campos del formulario de edición
const modelRef = ref('');
const colorRef = ref('');
const priceRef = ref(0);
const stockRef = ref(0);
const storeRef = ref('');

// URL base del backend
const base_url = 'http://app.backend-tapia/api';

// Obtener teléfonos cuando se monta el componente
onMounted(() => {
    getPhones();
});

// Función para obtener los teléfonos desde el backend
const getPhones = async () => {
    try {
        const response = await axios.get(`${base_url}/show-phones`);
        // Asignamos la respuesta a la variable phonesList
        phonesList.value = response.data.phones;
        // Los modelos vienen con "_" y queremos espacios
        phonesList.value.forEach(phone => phone.model = phone.model.replace(/_/g, ' '));
    } catch (error) {
        console.error(error);
    }
};

// Función para abrir el modal de edición
const openEditModal = (phone) => {
    currentPhoneId.value = phone.id;
    modelRef.value = phone.model.replace(/ /g, '_'); // Asegúrate de que el valor coincida con las opciones del select
    colorRef.value = phone.color;
    priceRef.value = phone.price;
    stockRef.value = phone.stock;
    storeRef.value = phone.store;
    activeEdit.value = true;
};

// Función para cerrar el modal de edición
const closeEditModal = () => {
    activeEdit.value = false;
};

// Función para editar el teléfono
const editPhone = async () => {
    // Lógica para editar el teléfono por axios put a edit-phones/{id}
    const phoneData = {
        model: modelRef.value,
        color: colorRef.value,
        price: priceRef.value,
        stock: stockRef.value,
        store: storeRef.value
    };
    try {
        const response = await axios.put(`${base_url}/edit-phones/${currentPhoneId.value}`, phoneData, {
            headers: {
                'Content-Type': 'application/json'
            }
        });
        console.log('Teléfono editado exitosamente:', response.data);
        // Actualizar la lista de teléfonos
        getPhones();
    } catch (error) {
        console.error('Error al editar el teléfono:', error);
    }
    // Cerrar el modal
    closeEditModal();
};

// Función para eliminar el teléfono
const deletePhone = async (phoneId) => {
    try {
        const response = await axios.delete(`${base_url}/delete-phones/${phoneId}`);
        console.log('Teléfono eliminado exitosamente:', response.data);
        // Actualizar la lista de teléfonos
        getPhones();
    } catch (error) {
        console.error('Error al eliminar el teléfono:', error);
    }
};
</script>