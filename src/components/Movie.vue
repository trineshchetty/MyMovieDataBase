<template>


  <v-container v-if="loading">
    <div class="text-xs-center">
        <v-progress-circular
          indeterminate
          :size="150"
          :width="8"
          color="green">
        </v-progress-circular>
      </div>
  </v-container>


    <v-container v-else>
        <v-layout wrap>
      <v-flex xs12 mr-1 ml-1>
        <v-card>
          <v-img
            :src="singleMovie.Poster"
            aspect-ratio="2"
          ></v-img>


                    <v-card-title primary-title>
            <div>
              <h2 class="headline mb-0">{{singleMovie.Title}}-{{singleMovie.Year}}</h2>
              <p>{{ singleMovie.Plot}} </p>
              <h3>Actors:</h3>{{singleMovie.Actors}}
               <h4>Awards:</h4> {{singleMovie.Awards}}
               <p>Genre: {{singleMovie.Genre}}</p>
            </div>
          </v-card-title>

                    <v-card-actions>
            <v-btn flat color="green" @click="back">back</v-btn>
          </v-card-actions>

              </v-card>
      </v-flex>
    </v-layout>
       
    
        
        <v-layout row wrap>
            <v-flex xs12>
                <div class="text-xs-center">
                    <v-dialog v-model="dialog" width="500">
                        <v-btn slot="activator" color="green" dark>View Ratings</v-btn>
                        

                        <v-card>
                            <v-card-title class="headline grey lighten-2" primary-title>Ratings</v-card-title>
                            <v-card-text>
                                <table style="width:100%" border="1" >
                                    <tr>
                                        <th>Source</th>
                                        <th>Ratings</th>
                                    </tr>
                                    
                                    <tr v-for="(rating,index) in this.ratings" :key="index">
                                        <td align="center">{{ratings[index].Source}}</td>
                                        <td align="center">{{ratings[index].Value}}</td>
                                    </tr>
                                </table>
                            </v-card-text>
                        <v-divider></v-divider>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="secondary" flat @click="dialog = false">OK</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </v-flex>
</v-layout>



</v-container>


    



    

</template>
<script>
import axios from 'axios'

export default {
    props: ['id'],
    data () {
        return {
            singleMovie: '',
            ratings: [],
            dialog: false,
            loading: true
            
        }
    },

    methods: {
          back () {
        this.$router.push('/')
    }

    },


    mounted () {
        axios.get(`http://www.omdbapi.com/?apikey=52a951f1&i=${this.id}&Content-Type=application/json`)
        .then(response => {
            this.singleMovie = response.data
            this.ratings = response.data.Ratings
            this.loading = false
        
        })
        .catch(error => {
            console.log(error)
        })
    }
}


</script>
<style scoped>

</style>
