<script setup>
import { ref } from 'vue';

const menuHidden = ref(true); // Skift mellem at vise/skjule menuen
const positions = ref([]); // Positioner for hver option
const radius = 60; // Radius for den cirkulære menu
const numOptions = 6; // Antal options i menuen

// Funktion til at vise den cirkulære menu ved klik
function showRadialMenu(event) {
  menuHidden.value = !menuHidden.value;

  if (!menuHidden.value) {
    const mouseX = event.clientX;
    const mouseY = event.clientY;

    // Beregn vinklen mellem elementerne (360 grader divideret med antallet af options)
    const anglePlacement = (2 * Math.PI) / numOptions;

    // Beregn placeringen af hvert menupunkt i cirklen
    positions.value = Array.from({ length: numOptions }).map((_, index) => {
      const angle = index * anglePlacement;

      // Beregn positionen af hver option relativt til musemarkøren
      const posX = mouseX + radius * Math.cos(angle) - 120; // Juster med en offset for centering
      const posY = mouseY + radius * Math.sin(angle) - 55; // Juster med en offset for centering

      return {
        left: `${posX}px`,
        top: `${posY}px`,
      };
    });
  }
}
// Funktion til at vise en besked, når en option klikkes
function showAlert(optionNumber) {
  alert(`You pressed option ${optionNumber}`);
}
</script>

<template>
  <div @click="showRadialMenu" class="container">
    <div v-if="!menuHidden" class="radial-menu">
      <div
        v-for="(position, index) in positions"
        :key="index"
        :style="position"
        @click.stop="showAlert(index + 1)"
        class="option"
      >
        {{ index + 1 }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
}

.radial-menu {
  position: absolute;
}

.option {
  position: absolute;
  width: 50px;
  height: 50px;
  background-color: lightblue;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  cursor: pointer;
}
</style>
