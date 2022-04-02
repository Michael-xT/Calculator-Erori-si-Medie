<template>
    <div id="abx">
        <div class="box">
            <div class="text">Introduceți valorile măsurate:</div>
            <div v-for="(element, index) in data" :key="index">
                <div class="hidd">
                    <div class="div">
                        <div class="text">
                            x [ <small>{{ index }}</small> ] =
                        </div>
                        <input
                            v-model="data[index]"
                            placeholder="Valoare măsurată"
                            class="input"
                            type="number"
                        />
                    </div>
                    <div class="div">
                        <div class="text">
                            y [ <small>{{ index }}</small> ] =
                        </div>

                        <input
                            v-model="data2[index]"
                            placeholder="Valoare măsurată"
                            class="input"
                            type="number"
                        />
                    </div>
                </div>
                <hr />
            </div>
        </div>

        <div class="media">
            a = {{ a }}
            <br />
            b = {{ b }}
        </div>
        <hr />
        <div class="media">
            P1,P2:
        </div>
        <div class="box">
            <div class="hidd">
                <div class="div">
                    <div class="text">x [ <small>1</small> ] =</div>
                    <input v-model="x1" placeholder="Valoare aleasă" class="input" type="number" />
                </div>
            </div>
            <div class="hidd">
                <div class="div">
                    <div class="text">x [ <small>2</small> ] =</div>
                    <input v-model="x2" placeholder="Valoare aleasă" class="input" type="number" />
                </div>
            </div>
            <hr />
            <button type="button" class="bttn" @click="createGraph()">Generează Grafic</button>
        </div>
        <div
            v-if="graph == true"
            id="chartContainer"
            style="height: 500px; width:90%; margin:auto; margin-top:10px;"
        ></div>
    </div>
</template>

<script>
import * as CanvasJS from '../canvasjs.min.js';
export default {
    name: 'ABX',
    components: {},
    methods: {
        AddOne() {
            this.data.push(null);
        },
        createGraph() {
            this.graph = true;
            setTimeout(() => {
                var dotz = [];
                for (let i = 0; i < this.dots.length - 1; i++) {
                    dotz.push({ x: parseFloat(this.dots[i].x), y: parseFloat(this.dots[i].y) });
                }
                console.log(dotz);
                var chart = new CanvasJS.Chart('chartContainer', {
                    animationEnabled: true,
                    toolTip: {
                        shared: false
                    },
                    axisX: {
                        title: 'X',
                        tickColor: 'red'
                    },
                    axisY: {
                        title: 'Y',
                        tickColor: 'red'
                    },
                    data: [
                        {
                            type: 'line',
                            markerType: 'cross',
                            markerSize: 10,
                            markerColor: 'black',
                            lineColor: '#0000004d',
                            lineThickness: 0,
                            lineDashType: 'dash',
                            dataPoints: dotz
                        },
                        {
                            type: 'line',
                            markerSize: 20,
                            dataPoints: [
                                {
                                    x: parseFloat(this.x1),
                                    y: this.a + this.b * parseFloat(this.x1)
                                },
                                {
                                    x: parseFloat(this.x2),
                                    y: this.a + this.b * parseFloat(this.x2)
                                }
                            ]
                        }
                    ]
                });

                chart.render();
            }, 500);
        }
    },
    data: function() {
        return {
            data: [null],
            data2: [null],
            a: 0,
            b: 0,
            x1: 0,
            x2: 0,
            dots: [null],
            graph: false
        };
    },
    mounted() {
        setInterval(() => {
            if (this.data[this.data.length - 1] != null) {
                this.AddOne();
            }

            var xkSum = 0,
                x2Sum = 0,
                xykSum = 0,
                ykSum = 0,
                dotz = [];

            for (let i = 0; i < this.data.length; i++) {
                if (parseFloat(this.data[i])) {
                    xkSum += parseFloat(this.data[i]);
                    x2Sum += parseFloat(this.data[i]) * parseFloat(this.data[i]);
                }
                if (parseFloat(this.data2[i])) {
                    ykSum += parseFloat(this.data2[i]);
                    xykSum += parseFloat(this.data[i]) * parseFloat(this.data2[i]);
                }
                dotz = [...dotz, { x: this.data[i], y: this.data2[i] }];
            }
            this.dots = dotz;
            this.a =
                (x2Sum * ykSum - xkSum * xykSum) / ((this.data.length - 1) * x2Sum - xkSum * xkSum);
            this.b =
                ((this.data.length - 1) * xykSum - xkSum * ykSum) /
                ((this.data.length - 1) * x2Sum - xkSum * xkSum);
        }, 100);
    }
};
</script>

<style lang="scss" scoped>
#abx {
    font-family: 'Montserrat', sans-serif;
    font-weight: 300;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: white;
    display: flex;
    flex-flow: column;
}
.bttn {
    width: 30%;
    border: solid 1px white;
    margin: auto;
    padding: 15px;
    // border-radius: 0px;
    color: white;
    background-color: #2c3e50;
    transition: ease 0.5s;
    font-size: 25px;
    cursor: pointer;
    &:hover,
    &.active {
        border: solid 1px #2e3133;
        background-color: white;
        color: black;
    }
}
@media (max-width: 1000px) {
    .bttn {
        width: 70%;
    }
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
    }
    @media (max-width: 1000px) {
        .hidd {
            flex-direction: column;
            .div {
                display: block;
                margin: 0px;
                .input {
                    width: 80%;
                }
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
}
</style>
