<template>
<div style="text-align: center">
    <v-card
        class="mx-auto"
        max-width="344"
        style="padding: 10px"
    >
    <div>
        <div>
        <v-row>
          <v-col
            cols="6"
          >
            <v-select
              v-model="fromKey"
              :items="rates"
              item-text="key"
              item-value="key"
              label="From:"
              @change="exchange"
            ></v-select>
          </v-col>

          <v-col
            cols="6"
          >
            <v-select
              v-model="toKey"
              :items="rates"
              item-text="key"
              item-value="key"
              label="To:"
              @change="exchange"
            ></v-select>
          </v-col>
        </v-row>
        
        </div>
        <v-row>
          <v-col
            cols="6"
          >
          <v-text-field type="number" v-model="fromValue" @input="exchange" ></v-text-field>
          </v-col>
          <v-col
            cols="6"
          >
          <v-text-field type="number" v-model="toValue" @input="exchange" readonly ></v-text-field>
          
          </v-col>
        </v-row>
    </div>
    </v-card>
    
</div>
</template>

<script>
import axios from 'axios'
export default {
    name: "Currency",
    data() {
        return {
            currencyExchange: [],
            rates: [],
            ratesValue: [],
            fromKey: 'USD',
            toKey: 'MMK',
            fromValue: 1,
            toValue: 0,
            result: 0
        }
    },
    created() {
        this.getCurrencyExchangeRate();
    },
    
    methods: {
        exchange() {
            console.log(this.ratesValue[this.fromKey])
            this.toValue = (this.fromValue/this.ratesValue[this.fromKey]) * this.ratesValue[this.toKey]
        },
        getCurrencyExchangeRate(){
            axios.get('https://api.currencyfreaks.com/latest?apikey=32edd2b97bfc4a75b4dab2b3be4a418d')
            .then(response => {
            // JSON responses are automatically parsed.
                this.currencyExchange = response.data
                let data = response.data.rates;
                this.ratesValue = data;
                this.rates = Object.keys(data).map((key) => {
                    return {key: key, value: data[key]}
                });
                this.toValue = (this.fromValue/this.ratesValue[this.fromKey]) * this.ratesValue.MMK
                this.rates = this.rates.sort((a, b) => (a.key > b.key) ? 1 : -1);

                console.log('response '+this.toValue, this.rates)

            })
            .catch(e => {
                console.log('error', e)
            })
            
        }
    }
}
</script>>