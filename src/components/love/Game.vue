<template>
    <div class="game">
        <img class="bg" src="public/love_bg.png" alt="Fondo del juego" />
        <img
            v-if="bombeando"
            class="inflador"
            src="src/assets/inflador_cerrado.png"
            alt="Inflador cerrado"
        />
        <img
            v-else
            class="inflador"
            src="src/assets/inflador_abierto.png"
            alt="Inflador abierto"
        />
        <img
            :style="{ bottom: (100 + aire) / 2 + 'px', height: aire + '%', left: (90 - aire)/2 + '%' }"
            class="corazon"
            src="src/assets/corazon.png"
            alt="Corazon"
        />
    </div>
    <h1>Bombeaste {{ conteo }} veces</h1>
    <h1>Aire = {{ aire }}</h1>
</template>

<script setup>
import { onMounted, ref } from "vue";

const bombeando = ref(false);
const conteo = ref(0);
const aire = ref(0.0);

const bombear = () => {
    if (!bombeando.value) {
        bombeando.value = true;
        conteo.value++;
        aire.value++;
    }
};

const stopBombear = () => {
    bombeando.value = false;
};

const handleKeyPress = ({ key, type }) => {
    if (key == " ") {
        if (type == "keydown") bombear();
        else stopBombear();
    }
};

onMounted(() => {
    document.addEventListener("keydown", handleKeyPress);
    document.addEventListener("keyup", handleKeyPress);
    setInterval(() => {
        if(aire.value > 0.2) aire.value -= 0.01 * aire.value;
    }, 100);
});
</script>

<style>
.game {
    position: relative;
    top: 0;
    left: 0;
    .bg {
        cursor:
            url(https://cdn.custom-cursor.com/db/cursor/32/Gun_Sight.png), auto;
        position: relative;
        border: 2px solid black;
        width: 100%;
        z-index: 0;
    }
    .inflador {
        height: 30%;
        position: absolute;
        left: 40%;
        bottom: 5px;
        z-index: 100;
    }
    .corazon {
        position: absolute;
        z-index: 50;
    }
}
</style>
