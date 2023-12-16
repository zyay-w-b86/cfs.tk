<template>
    <div>
        <v-row>
            <v-col>
                <v-text-field v-model="radius" label="Radius" type="number" min="0" required></v-text-field>
            </v-col>
            <v-col>
                <v-text-field v-model="diameter" label="Diameter" type="number" min="0" required></v-text-field>
            </v-col>
            <v-col>
                <v-text-field v-model="circumference" label="Circumference" type="number" min="0" required></v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field v-model="target" label="Target" type="number" min="0" required></v-text-field>
            </v-col>

        </v-row>
        <v-row>
            <v-col class="d-flex justify-center">
                <v-progress-circular class="wrap" :rotate="-90" :size="360" :width="40" :value="value" color="warning">

                    <div>
                        <div>Angle:
                            <v-chip class="ma-2" color="warning" large text-color="black" label>
                                {{ angle }}°
                            </v-chip>
                        </div>
                        <div>Clock position:
                            <v-chip class="ma-2" color="warning" large text-color="black" label>
                                {{ clock }}
                            </v-chip>
                        </div>
                    </div>

                </v-progress-circular>
            </v-col>
        </v-row>
    </div>
</template>
<script setup>
import { Big } from 'big.js';
</script>

<script>
export default {
    data: () => ({
        radius: 1,
        pi: 3.14159265358979323846,
        target: 1,
    }),
    computed: {
        diameter() {
            return this.radius * 2;
        },
        circumference() {
            return this.radius ? Big(this.pi).times(this.diameter) : 0;
        },
        angle() {
            if (!this.target || !this.circumference) {
                return 0;
            } else {
                return Big(this.target).div(this.circumference).times(360).round(2);
            }
        },
        value() {
            if (!this.target || !this.circumference) {
                return 0;
            } else {
                return Big(this.target).div(this.circumference).times(100).mod(100).round(2);
            }
        },
        clock() {
            if (!this.target || !this.circumference) {
                return 0;
            } else {
                let mins = Big(this.angle).times(2);
                return mins.div(60).round(0, Big.roundDown) + ':' + mins.mod(60).round(0, Big.roundDown) + ':' + mins.mod(60).mod(60).round();
            }
        },
    },

}
</script>