<script setup>
    import axios from 'axios';
    import { onMounted, ref } from 'vue';
    
    const barber = ref({
        'barber_name':''
    });
    const barbers = ref([]);
    
    const uploadBarber = async () => {
        try {
            const response = await axios.post('http://localhost:8000/api/barbers',barber.value);
            if (response.data.success) {
                console.log(response.data.message);
                alert(response.data.message);

            }
            barber.value.barber_name = '';
            getBarbers();
        } catch (error) {
            console.log(error);
        }
    }
    const getBarbers = async () => {
        try {
            const response = await axios.get('http://localhost:8000/api/barbers');
            barbers.value = response.data;
        } catch (error) {
            console.log(error);
        }
    }
    const deleteBarber = async (id) => {
        try {
            const response = await axios.delete('http://localhost:8000/api/barbers',{
                params:{
                    id:id
                }
            });
            if (response.data.success) alert(response.data.message);
            getBarbers();
        } catch (error) {
            console.log(error);
        }
    }
    onMounted(()=>{
        getBarbers();
    })
</script>

<template>
    <div class="mb-4">
        <label for="barber_name" class="form-label">Név</label>
        <input type="text" name="" id="barber_name" class="form-control" v-model="barber.barber_name">
    </div>
    <button class="btn btn-info mb-4" @click="uploadBarber">Hozzáadás</button>
    <hr>
    <div class="mt-4">
        <h3>Fodrászok</h3>
        <ul class="list-group" v-if="barbers.length>0">
            <li class="list-group-item d-flex justify-content-between" v-for="(item,index) in barbers"><span>{{ item.barber_name }}</span> <button class="btn btn-danger" @click="deleteBarber(item.id)">Törlés</button></li>
        </ul>
        <p v-else>Nincsenek fodrászok</p>
    </div>
</template>

<style scoped>

</style>
