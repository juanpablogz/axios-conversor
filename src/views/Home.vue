<template>

  <div>
      <v-layout>
        <v-flex xs12>

          <v-card>
            <v-date-picker 
            v-model="fecha"
            full-width
            locale= "es-cl"
            :min ="minimo"
            :max ="maximo"
            @change="getDolar(fecha)"
            >

            </v-date-picker>
          </v-card>

        <v-card color="error" dark>
         <v-card-text class="display-1 text-center">
           {{valor}}
         </v-card-text>
        </v-card>
        </v-flex>
       
       </v-layout>

       
  </div>
 
</template>

<script>


import  axios  from "axios";


export default {
data() {
  return {
    fecha: new Date().toISOString().substr(0, 10),
    minimo: "1984",
    maximo: new Date().toISOString().substr(0, 10),
    valor: null
  }
},
methods: {


  async getDolar(dia){
    console.log(dia);
     let arrayFecha = dia.split(['-'])
     console.log (arrayFecha);
     let ddmmyy = arrayFecha[2] + '-' + arrayFecha[1] + '-' + arrayFecha[0]

    try {

    
        let datos = await axios.get(`https://mindicador.cl/api/dolar/${ddmmyy}`)

        if (datos.data.serie > 0) {
          this.valor = await datos.data.serie[0].valor
        } else {
          this.valor = 'sin resultado'
        }
      console.log (datos.data.serie);
      
      this.valor = await datos.data.serie[0].valor
    } catch (error) {
      
    }

    finally{


    }

    
  }
},
created() {
  this.getDolar(this.fecha)
}
}
</script>
