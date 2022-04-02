<template>
    <div id="mFacultate">
        <div class="box">
            <div class="text">Introduceți mediile:</div>
            <div v-for="(element, index) in data" :key="index">
                <div class="hidd">
                    <div class="div">
                        <input
                            v-model="data[index]"
                            placeholder="Materie"
                            class="input"
                            type="text"
                        />
                    </div>
                    <div class="div">
                        <input
                            v-model="data2[index]"
                            placeholder="Medie"
                            :class="
                                parseFloat(data2[index]) < 0 || parseFloat(data2[index]) > 10
                                    ? 'input red'
                                    : 'input'
                            "
                            type="number"
                            min="0"
                            max="10"
                        />
                    </div>
                    <div class="div">
                        <input
                            v-model="data3[index]"
                            placeholder="Număr de credite"
                            class="input"
                            type="number"
                        />
                    </div>
                </div>
                <hr />
            </div>
        </div>
        <hr />
        <div class="media" v-if="calculated != false">
            <b>Media ta finală este : {{ medieF.toFixed(2) }}</b>
            <br />
            <small>Media ponderată: {{ medieP.toFixed(2) }}</small>
            <br />
            <small>Media aritmetică: {{ medieA.toFixed(2) }}</small>
            <br />
        </div>
    </div>
</template>

<script>
export default {
    name: 'Medie Facultate',
    components: {},
    methods: {
        AddOne() {
            this.data.push(null);
        }
    },
    data: function() {
        return {
            data: [null],
            data2: [null],
            data3: [null],
            medieF: 0,
            medieA: 0,
            medieP: 0,
            calculated: false
        };
    },

    mounted() {
        setInterval(() => {
            if (this.data2[this.data.length - 1] != null) {
                this.AddOne();
            }
            //Adaugă input în momentul în care ultimul input este completat

            this.medieA = 0;
            this.medieP = 0;
            let k = 0;
            let crediteTotale = 0;
            for (let i = 0; i < this.data2.length; i++) {
                if (parseFloat(this.data2[i])) {
                    k++;
                    this.medieA += parseFloat(this.data2[i]);
                }
                if (parseFloat(this.data3[i])) {
                    crediteTotale += parseFloat(this.data3[i]);
                    this.medieP += parseFloat(this.data2[i]) * parseFloat(this.data3[i]);
                }
            }
            this.medieA /= k;
            this.medieP /= crediteTotale;
            this.medieF = this.medieA + this.medieP;
            this.medieF /= 2;
            if (parseInt(this.medieF) != 0) this.calculated = true;
        }, 1);
    }
};
</script>

<style lang="scss" scoped>
#mFacultate {
    font-family: 'Montserrat', sans-serif;
    font-weight: 300;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: white;
    display: flex;
    flex-flow: column;
}
.box {
    display: flex;
    flex-direction: column;

    .hidd {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        .div {
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 5px;
            .text {
                font-family: 'Montserrat', sans-serif;
                font-weight: 300;
                font-size: 15px;
                margin: 5px;
            }
            .input {
                font-family: 'Montserrat', sans-serif;
                font-weight: 500;
                width: 35vh;
                text-align: center;
                font-size: 25px;
                margin: 5px auto;
                background: #2c3e50;
                padding: 5px;
                border: solid 1px white;
                border-radius: 5px;
                color: white;
                &.red {
                    animation: wrong 0.8s infinite;
                }
            }
        }
    }
    @media (max-width: 1000px) {
        .hidd {
            flex-direction: column;
            .div {
                display: block;
                margin: 0px;
            }
        }
    }
    .text {
        font-weight: 300;
        font-size: 35px;
    }
}

.media {
    padding: 25px;
    font-size: 35px;
    font-weight: 400;
    small {
        padding: 15px;
        font-weight: 300;
        font-size: 20px;
    }
}
@keyframes wrong {
    0% {
        border: solid 2px white;
    }
    50% {
        border: solid 2px red;
    }
    100% {
        border: solid 2px white;
    }
}
</style>
