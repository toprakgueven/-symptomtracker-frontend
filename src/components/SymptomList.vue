<script setup>
const symptome = [
  {
    id: 1,
    bezeichnung: 'Kopfschmerzen',
    beschreibung: 'Starke Kopfschmerzen am Morgen',
    datum: '05.10.2026',
    staerke: 7
  },
  {
    id: 2,
    bezeichnung: 'Augenzucken',
    beschreibung: 'Leichtes Zucken am linken Auge',
    datum: '04.10.2026',
    staerke: 3
  },
  {
    id: 3,
    bezeichnung: 'Knieschmerzen',
    beschreibung: 'Schmerzen nach dem Sport',
    datum: '03.10.2026',
    staerke: 5
  }
]
</script>

<template>
  <div>
    <h2>Meine Symptome</h2>

    <div
      v-for="symptom in symptome"
      :key="symptom.id"
    >
      <h3>{{ symptom.bezeichnung }}</h3>
      <p>{{ symptom.beschreibung }}</p>
      <p>Datum: {{ symptom.datum }}</p>
      <p>Stärke: {{ symptom.staerke }}/10</p>
    </div>
  </div>
</template>