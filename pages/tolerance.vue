<template>
    <div>
        <v-row>
            <v-col>
                <h1>Engineering tolerance</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col>

            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <h3>Report</h3>
            </v-col>
        </v-row>

        <v-form ref="form" v-model="valid" lazy-validation>
            <v-row>
                <v-col>
                    <v-text-field v-model="name" :counter="20" :rules="nameRules" label="Title"></v-text-field>
                </v-col>
            </v-row>
            <v-row>
                <v-col>
                    <v-text-field v-model="deviation" :rules="deviationRules" label="Deviation (%)" type="number" min="0"
                        max="100" required></v-text-field>
                </v-col>
                <v-col>
                    <v-btn-toggle v-model="deviation" @change="noNull($event)">
                        <v-btn value="1">
                            1
                        </v-btn>
                        <v-btn value="2">
                            2
                        </v-btn>
                        <v-btn value="3">
                            3
                        </v-btn>
                        <v-btn value="5">
                            5
                        </v-btn>
                        <v-btn value="10">
                            10
                        </v-btn>
                    </v-btn-toggle>

                </v-col>
                <v-spacer></v-spacer>
            </v-row>
            <v-row>
                <v-col>
                    <v-text-field v-model="length1" :rules="lengthRules" label="Length" type="number" min="0" step="0.01"
                        required></v-text-field>
                </v-col>
                <v-col>
                    <v-chip class="ma-2" label>
                        ± {{ length1Range }}
                    </v-chip>
                    <span class="text-no-wrap font-weight-medium">
                        {{ length1UpperLimit }} ~ {{ length1LowerLimit }}
                    </span>
                </v-col>
                <v-spacer></v-spacer>
            </v-row>
            <v-row>
                <v-col>
                    <v-text-field v-model="length2" :rules="lengthRules" label="Length" type="number" min="0" step="0.01"
                        required></v-text-field>
                </v-col>
                <v-col>
                    <v-chip class="ma-2" label>
                        ± {{ length2Range }}
                    </v-chip>
                    <span class="text-no-wrap font-weight-medium">
                        {{ length2UpperLimit }} ~ {{ length2LowerLimit }}
                    </span>
                </v-col>
                <v-spacer></v-spacer>
            </v-row>
            <v-row>
                <v-col>
                    <v-text-field v-model="length3" :rules="lengthRules" label="Length" type="number" min="0" step="0.01"
                        required></v-text-field>
                </v-col>
                <v-col>
                    <v-chip class="ma-2" label>
                        ± {{ length3Range }}
                    </v-chip>
                    <span class="text-no-wrap font-weight-medium">
                        {{ length3UpperLimit }} ~ {{ length3LowerLimit }}
                    </span>


                </v-col>
                <v-spacer></v-spacer>
            </v-row>

            <v-row>
                <v-btn :disabled="!valid" color="primary" class="mr-4" @click="validate">
                    Save
                </v-btn>

                <v-btn color="error" class="mr-4" @click="reset">
                    Reset
                </v-btn>

            </v-row>


        </v-form>

    </div>
</template>
<script setup>
import { Big } from 'big.js';
</script>
<script>
export default {
    data: () => ({
        valid: true,
        name: '',
        nameRules: [
            // v => !!v || 'Name is required',
            v => (v.length <= 20) || 'Name must be less than 20 characters',
        ],
        deviation: 5,
        deviationRules: [
            v => !!v || 'Deviation is required',
            v => (v && v > 0 && v < 100) || 'Deviation must be number over 0 up to 100',
        ],
        length1: 0,
        length2: 0,
        length3: 0,
        lengthRules: [
            // v => (v && v > 0) || 'Deviation must be number over 0',
        ],

    }),

    methods: {
        validate() {
            this.$refs.form.validate()
        },
        reset() {
            // this.$refs.form.reset()
            this.name = '';
            this.deviation = 5;
            this.length1 = 0;
            this.length2 = 0;
            this.length3 = 0;

        },
        resetValidation() {
            this.$refs.form.resetValidation()
        },
        // computeRange(num) {
        //     Big(parseFloat(num)).times(this.deviation * 0.01)
        // },
        noNull(e) {
            console.log(e)
            if (e === null || e === undefined) {
                this.deviation = 0
            }
        }
    },
    computed: {
        num1() {
            let value = parseFloat(this.length1);
            return value === NaN ? 0 : Big(value);
        },
        num2() {
            let value = parseFloat(this.length2);
            return value === NaN ? 0 : Big(value);
        },
        num3() {
            let value = parseFloat(this.length3);
            return value === NaN ? 0 : Big(value);
        },
        length1Range() {
            return this.num1.times(this.deviation * 0.01)
        },
        length1UpperLimit() {
            return this.num1.plus(this.length1Range)
        },
        length1LowerLimit() {
            return this.num1.minus(this.length1Range)
        },
        length2Range() {
            return this.num2.times(this.deviation * 0.01)
        },
        length2UpperLimit() {
            return this.num2.plus(this.length2Range)
        },
        length2LowerLimit() {
            return this.num2.minus(this.length2Range)
        },
        length3Range() {
            return this.num3.times(this.deviation * 0.01)
        },
        length3UpperLimit() {
            return this.num3.plus(this.length3Range)
        },
        length3LowerLimit() {
            return this.num3.minus(this.length3Range)
        },
    },
}
</script>