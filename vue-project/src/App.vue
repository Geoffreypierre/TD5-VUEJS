<script setup lang="ts">
import { ref, computed } from "vue";
import type { Ref } from "vue";
interface Tache {
  id: number;
  description: string;
  faite: boolean;
}
let id = 0;
let nouvelleTache = ref("");
const tachesFiltrees = computed(() => {
  return cacheFaits.value ? taches.value.filter((e) => !e.faite) : taches.value;
});
const cacheFaits = ref(false);
const taches: Ref<Tache[]> = ref([
  { id: id++, description: "Apprendre vue", faite: false },
  { id: id++, description: "Finir la SAÉ", faite: false },
  { id: id++, description: "Réviser pour l'interro", faite: false },
]);

function ajouterTache() {
  if (nouvelleTache.value != "") {
    taches.value.push({
      id: id++,
      description: nouvelleTache.value,
      faite: false,
    });
    nouvelleTache.value = "";
  }
}
function retirerTache(tache: Tache) {
  taches.value = taches.value.filter((e) => e.description != tache.description);
}
</script>

<template>
  <div id="wrapper">
    <ul>
      <input
        placeholder="Ajouter une nouvelle tâche"
        v-model.trim="nouvelleTache"
      />
      <button @click="ajouterTache">Ajouter</button>
      <button @click="cacheFaits = !cacheFaits">
        {{ cacheFaits ? "Tout montrer" : "Cacher les tâches terminées" }}
      </button>
      <li v-for="tache in tachesFiltrees" :key="tache.id">
        <input type="checkbox" v-model="tache.faite" />
        <span v-bind:class="{ fait: tache.faite }">{{
          tache.description
        }}</span>
        <button @click="retirerTache(tache)">Retirer tâche</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
#wrapper {
  border-radius: 5px;
  border: solid black 2px;
  padding: 10px;
}
ul,
span {
  padding: 10px;
}
li {
  list-style: none;
  padding: 2px 0px;
}
.fait {
  text-decoration: line-through;
}
</style>
