<template>

    <div class="app">

        <div class="header">

            <div
                :class="page == 0 ? 'button active corner-left' : 'button corner-left'"
                @click="Page(0)"
            >
                 Calculator Erori
            </div>

            <div :class="page == 1 ? 'button active' : 'button'" @click="Page(1)"> y=a+bx </div>

            <div
                :class="page == 2 ? 'button active corner-right' : 'button corner-right'"
                @click="Page(2)"
            >
                 Medie Facultate
            </div>

        </div>

        <div class="body">

            <hr />

            <CErori v-if="page === 0" />

            <ABX v-if="page === 1" />

            <medieFacultate v-if="page === 2" />

            <hr />

        </div>

        <div class="footer">

            <a href="https://github.com/Michael-xT/Calculator-Erori-si-Medie">

                <small>
                     Realizat cu ☕ și ❤️ de către Mihăiță T.
                    <br />
                     Versiunea 1.3
                </small>

            </a>

        </div>

    </div>

</template>

<script>
 import CErori from './components/CErori.vue'; import ABX from './components/abx.vue'; import medieFacultate
from './components/medieFacultate.vue'; export default { name: 'App', components: { CErori, ABX, medieFacultate
}, data: function() { return { page: 0 }; }, methods: { Page(id) { this.page = id; } } };
</script>

<style lang="scss">
$background: #2c3e50;
* {
    margin: 0;
}
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;300;400;500&display=swap');
body {
    background: $background;
    height: 100vh;
    width: 100%;
}
.app {
    font-family: 'Montserrat', sans-serif;
    font-weight: 300;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: white;
    display: flex;
    font-size: 20px;
    flex-flow: column;
    .header {
        background: darken($background, 5%);
        position: fixed;
        // background: darkblue;
        width: 100%;
        height: 100px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        .button {
            margin-left: -1px;
            width: 15%;
            border: solid 1px white;
            padding: 15px;
            font-size: 25px;
            font-weight: 500;
            color: white;
            background: $background;
            // opacity: 1;
            z-index: 0;
            transition: ease 0.5s;
            cursor: pointer;
            &:hover,
            &.active {
                background: white;
                color: black;
            }
            &.corner-right {
                border-radius: 0px 10px 10px 0px;
            }
            &.corner-left {
                border-radius: 10px 0px 0px 10px;
            }
        }
    }
    @media (max-width: 1000px) {
        .header {
            width: 100%;
            flex-direction: row;
            .button {
                width: 100%;
                padding: 10px;
                font-size: 20px;
            }
        }
    }
    .body {
        margin-top: 100px;
        margin-bottom: 50px;
    }
    .footer {
        background: darken($background, 5%);
        position: fixed;
        bottom: 0px;
        width: 100%;
        height: 50px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
    }
}
::placeholder {
    /* Chrome, Firefox, Opera, Safari 10.1+ */
    color: rgba(255, 255, 255, 0.3);
    // z-index: 1;
    // opacity: 0.6; /* Firefox */
}
hr {
    width: 30%;
    height: 1px;
    border: hidden;
    background: gray;
    margin: 5px auto;
}
a {
    color: white;
    text-decoration: none;

    transition: ease 0.5s;
    &:hover {
        color: grey;
    }
}
/* width */
::-webkit-scrollbar {
    width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
    background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
    background: #888;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
    background: #555;
}
</style>

