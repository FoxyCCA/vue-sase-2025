<script lang="ts" setup>
import type { AirlineModel } from '@/models/airline.model';
import { formatTime } from '@/utils'
import { AirlineService } from '@/services/airline.service';
import { AuthService } from '@/services/auth.service';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter()
const airlines = ref<AirlineModel[]>()
AirlineService.getAirlines()
    .then(rsp => airlines.value = rsp.data)
    .catch(e=> {
        AuthService.removeAuth()
        router.push('/login')
    })

function deleteObject(airline: AirlineModel) {

}
</script>

<template>
    <div>
        <h3>Airlines</h3>
        <RouterLink to="/airline/new" class="butn btn-sm btn-primary">
            <i class="fa-solid fa-plus"></i> Add Airline
        </RouterLink>
    </div>
    <table class="table table-stripped table-hover" v-if="airlines">
        <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Website</th>
                <th scope="col">updated At</th>
                <th scope="col">Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="a of airlines" :key="a.airlineId">
                <th scope="row">{{ a.airlineId }}</th>
                <td>{{ a.name }}</td>
                <td>
                    <a :href="a.website"> {{ a.website }}</a>
                </td>
                <td>{{ formatTime(a.updatedAt ?? a.createdAt) }}</td>
                <div class="btn-group">
                    <RouterLink :to="`/airline/${a.airlineId}`" class="butn btn-sm btn-success">
                        <i class="fa-solid fa-pen-to-square"></i>
                    </RouterLink>
                    <button class="btn btn-sm btn-danger" @click="deleteObject(a)">
                        <i class="fa-solid fa-trash"></i>
                    </button>
                </div>
            </tr>
        </tbody>
    </table>
</template>