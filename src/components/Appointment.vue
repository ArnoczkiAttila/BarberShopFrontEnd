<script setup>
    import axios from 'axios';
    import { onMounted, ref } from 'vue';
    const barbers = ref([]);
    const appointments = ref([]);
    const appointment = ref({
        'name':'',
        'barber_id':null,
        'appointment':''
    });
    const getBarbers = async () => {
        try {
            const response = await axios.get('http://localhost:8000/api/barbers');
            barbers.value = response.data;
        } catch (error) {
            console.log(error);
        }
    }
    const deleteAppointment = async (id) => {
        try {
            const response = await axios.delete('http://localhost:8000/api/appointments',{
                params:{
                    id:id
                }
            });
            getAppointments();
            if (response.data.success) alert(response.data.message);
        } catch (error) {
            console.log(error);
        }
    }
    const getAppointments = async () => {
        try {
            const response = await axios.get('http://localhost:8000/api/appointments');
            appointment.value = [];
            console.log(response.data);
            response.data.forEach(element => {
                element.showDetails = false;
                appointments.value.push(element);
            });
        } catch (error) {
            console.log(error);
        }
    }
    const uploadAppointment = async () => {
        try {
            const response = await axios.post('http://localhost:8000/api/appointments',appointment.value);
            console.log(response);
            
            if (response.data.success) {
                console.log(response.data.message);
                alert(response.data.message);
            }
            appointment.value.name = '';
            appointment.value.barber_id = null;
            appointment.value.appointment = '';
            getAppointments();
            getBarbers();
        } catch (error) {
            console.log(error);
        }
    }
    onMounted(()=>{
        getBarbers();
        getAppointments();
    })
</script>

<template>
    <div class="mb-4">
        <label for="name" class="form-label">Név</label>
        <input type="text" name="" id="name" class="form-control" v-model="appointment.name">
    </div>
    <div class="mb-4">
        <label for="barber_name" class="form-label">Fodrász</label>
        <select name="" id="barber_name" class="form-control" v-model="appointment.barber_id">
            <option  v-for="barber in barbers" :value="barber.id">{{ barber.barber_name }}</option>
        </select>
    </div>
    <div class="mb-4">
        <label for="idopont" class="form-label">Időpont</label>
        <input type="datetime-local" name="" id="idopont" class="form-control" v-model="appointment.appointment">
    </div>
    
    <button class="btn btn-info mb-4" @click="uploadAppointment">Hozzáadás</button>
    <hr>
    <div class="mt-4">
        <h3>Foglalások</h3>
        <ul class="list-group" v-if="appointments.length>0">
            <li class="list-group-item" v-for="(item,index) in appointments">
                <div class="d-flex justify-content-between">
                    <span>{{ item.name }} - {{ item.appointment }}</span> 
                    <div>
                        <button class="btn btn-warning me-3" @click="appointments[index].showDetails =!appointments[index].showDetails ">Részletek</button>
                        <button class="btn btn-danger" @click="deleteAppointment(item.id)">Törlés</button>
                    </div>
                </div>
                <div class="d-block mt-3" v-if="item.showDetails">
                    <ul class="list-group">
                        <li class="list-group-item">
                            {{ item.barber.barber_name }}
                        </li>
                    </ul>
                </div>
            </li>
        </ul>
        <p v-else>Nincsenek foglalások</p>
    </div>

</template>

<style scoped>

</style>
