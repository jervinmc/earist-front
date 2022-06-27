<template>
  <div>
        <v-card class="pa-10 rounded-xl">
            <div align="center" class="pb-10 text-h6">
                <b>Floor Management({{building_name}})</b>
            </div>
            <div>
                <v-card v-for="index in parseInt(floor)" :key="index" height="100"  @click="viewRoom(index)">
                    <div>
                        <v-row>
                            <v-col>
                                
                            </v-col>
                            <v-spacer></v-spacer>
                            <v-col align-self="center">
                                {{index}} floor
                            </v-col>
                        </v-row>
                    </div>
                </v-card>
            </div>
        </v-card>
  </div>
</template>

<script>
export default {
    created(){
        this.floor = this.$route.query.floor
        this.building_name = this.$route.query.building_name
    },
    methods:{
        viewRoom(floor){
            window.location.href=`/room?building_name=${this.building_name}&floor=${floor}`
        }
    },
    data(){
        return{
            floor:1,
            building_name:''
        }
    }
}
</script>

<style>

</style>