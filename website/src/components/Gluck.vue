<template>
    <div class="flex flex-col items-center justify-center min-h-screen p-4">
        <button @click="waehleStandesamt" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition duration-300 ease-in-out mb-4">
            Glückliche Wahl
        </button>
        <div v-if="ausgewaehltesStandesamt" class="max-w-sm rounded overflow-hidden shadow-lg bg-white p-4">
            <h2 class="font-bold text-xl mb-2">{{ ausgewaehltesStandesamt.LOCATION }}</h2>
            <p class="text-gray-700 text-base">Adresse: {{ ausgewaehltesStandesamt.ADRESSE }}</p>
            <a :href="ausgewaehltesStandesamt.WEBLINK1" target="_blank" class="text-blue-500 hover:text-blue-800 transition duration-300 ease-in-out">
                Weitere Informationen
            </a>
        </div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from 'vue';

    // Erstellt eine reaktive Referenz für die Standesämter und das ausgewählte Standesamt
    const standesaemter = ref([]);
    const ausgewaehltesStandesamt = ref(null);

    // Definiert die Funktion 'ladeStandesämter'
    const ladeStandesämter = async () => {
        try {
            // Sendet eine Anfrage an die API, um die Daten der Standesämter zu erhalten
            const response = await fetch("https://data.wien.gv.at/daten/geo?service=WFS&request=GetFeature&version=1.1.0&typeName=ogdwien:TRAUMHOCHZEITOGD&srsName=EPSG:4326&outputFormat=json");
            // Wandelt die Antwort in ein JSON-Objekt um
            const data = await response.json();
            // Speichert die Daten der Standesämter in der reaktiven Referenz 'standesämter'
            standesaemter.value = data.features.map(feature => feature.properties);
        } catch (error) {
            // Gibt einen Fehler aus, wenn beim Laden der Standesämter ein Problem auftritt
            console.error("Fehler beim Laden der Standesämter:", error);
        }
    };

    // Definiert die Funktion 'waehleStandesamt'
    const waehleStandesamt = () => {
        // Überprüft, ob Standesämter vorhanden sind
        if (standesaemter.value.length > 0) {
            // Wählt ein zufälliges Standesamt aus
            const zufaelligerIndex = Math.floor(Math.random() * standesaemter.value.length);
            // Speichert das ausgewählte Standesamt in der reaktiven Referenz 'ausgewaehltesStandesamt'
            ausgewaehltesStandesamt.value = standesaemter.value[zufaelligerIndex];
        }
    };
    // Wartet bis die Komponente geladen ist und ruft dann die Funktion 'ladeStandesämter' auf
    onMounted(ladeStandesämter);
</script>

<style>

</style>
  