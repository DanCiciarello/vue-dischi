<template>
    <div class="cdDisksListsContainer py-5">
        <div class="container">

            <!-- Sezione filtri -->
            <div class="row pb-5">
                <div class="col-3">
                    <p class="text-white">Filtra per genere:</p>
                    <div class="input">
                        <select class="form-select" @click="getMusicGenres" v-model="genreSelect">
                            <option>All</option>
                            <option v-for="genre in genreList" :key="genre">{{genre}}</option>
                        </select>
                    </div>
                </div>
            </div>

            <!-- Lista dischi -->
            <div class="row row-cols-5 gx-5 gy-4">
                <div class="col" v-for="disk in getDisksFiltered" :key="disk.title">
                    <DiskItem :disk-obj="disk" />
                </div>
            </div>
            
        </div>
    </div>
</template>

<!-- --------------------------------------------------------------------------- -->

<script>

import axios from "axios";
import DiskItem from "./DiskItem.vue";

export default {
    name: "DisksList",
    data() {
        return {
            apiUrl: "https://flynn.boolean.careers/exercises/api/array/music",
            disksArray: null,
            genreList: [],
            genreSelect: "All",
        };
    },
    computed: {
        getDisksFiltered(){
            // Se è selezionato All
            if(this.genreSelect === "All"){
                // Ritorno l'array completo
                return this.disksArray;
            } 
            // Altrimenti se selezionato un genere
            else {
                // Filtro l'array originale
                return this.disksArray.filter(disk => {
                    return disk.genre.includes(this.genreSelect);
                })
            }
        }
    },
    methods: {
        fetchDisksList() {
            // Chiamo l'API e valorizzo l'array con i risultati
            axios
                .get(this.apiUrl)
                .then((result) => {
                    this.disksArray = result.data.response;
            });
        },
        getMusicGenres(){
            this.disksArray.forEach((disk) => {
                if(!this.genreList.includes(disk.genre)) {
                    this.genreList.push(disk.genre);
                }
            }
            )
        },
    },
    mounted() {
        this.fetchDisksList();
    },
    components: { DiskItem }
}
</script>

<!-- --------------------------------------------------------------------------- -->

<style lang="scss">

</style>