<template>
    <div id="cerori">
        <div class="box">
            <div class="text">Introduceți valorile măsurate:</div>
            <div v-for="(element, index) in data" :key="index">
                x [ <small>{{ index }}</small> ] =
                <input
                    v-model="data[index]"
                    placeholder="Valoare măsurată"
                    class="input"
                    type="number"
                />
            </div>
        </div>
        <hr />
        <div class="media" v-if="data != [null]">
            Rezultate:<br />
            X<small>o</small>: {{ x0 }}<br />Ea <small>(eroare absoluta)</small>: {{ e_absoluta
            }}<br />

            ε<small>r (eroarea relativa procentuala)</small>:
            <span v-if="e_relativa < 5" style="color:green">{{ e_relativa }}</span
            ><span v-if="e_relativa > 5" style="color:red"
                >{{ e_relativa }} %<br />
                <b
                    >Se consideră că măsurătorile au condus la un rezultat ce poate fi acceptat
                    d.p.d.v. al erorilor de măsură, dacă este îndeplinită condiția ε<small>r</small>
                    ≤ 5%</b
                >
            </span>

            <br />v ∈ [ {{ media }} ± {{ e_absoluta }} ]
        </div>
    </div>
</template>

<script>
export default {
    name: 'CErori',
    components: {},
    methods: {
        AddOne() {
            this.data.push(null);
        }
    },
    data: function() {
        return {
            x0: 0,
            data: [null],
            media: 0,
            e_absoluta: 0,
            e_relativa: 0,
            m2: 0
        };
    },

    mounted() {
        setInterval(() => {
            if (this.data[this.data.length - 1] != null) {
                this.AddOne();
            }
            //Adaugă input în momentul în care ultimul input este completat

            this.media = 0;
            let k = 0;
            for (let i = 0; i < this.data.length; i++) {
                if (parseFloat(this.data[i])) {
                    k++;
                    this.media += parseFloat(this.data[i]);
                }
            }
            this.media /= k;
            this.x0 = this.media;
            this.m2 = 0;
            for (let h = 0; h < this.data.length; h++) {
                if (parseFloat(this.data[h])) {
                    let deltaxi = this.x0 - parseFloat(this.data[h]);
                    this.m2 += deltaxi * deltaxi;
                }
            }

            this.e_absoluta = Math.sqrt(this.m2 / (k * (k - 1)));
            this.e_relativa = (this.e_absoluta / this.x0) * 100;
        }, 1);
    }
};
</script>

<style lang="scss" scoped>
#cerori {
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
    .text {
        font-weight: 300;
        font-size: 35px;
    }
    .input {
        font-weight: 500;
        width: 45vh;
        text-align: center;
        font-size: 25px;
        margin: 5px auto;
        background: #2c3e50;
        padding: 5px;
        border: solid 1px white;
        border-radius: 5px;
        color: white;
    }
}
@media (max-width: 1000px) {
    .box {
        .input {
            width: 70%;
        }
    }
}
.media {
    padding: 25px;
    font-size: 35px;
    font-weight: 400;
}
</style>
