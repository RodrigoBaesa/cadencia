<script setup>
import { onMounted } from 'vue'
import { Factory } from 'vexflow'
import notesMap from './notesMap.json'

const props = defineProps({
                clef: String,
                compass: String,
                notesInput: String,
                stem: String,
              })

const convertNotes = (input) => {
  return input.replace(/(do|re|mi|fa|sol|la|si)/gi, (match) => {
    return notesMap[match.toLowerCase()];
  });
};

const scoreInput = props.notesInput;
const convertedNotes = convertNotes(scoreInput);

onMounted(() => {
  const factory = new Factory({
    renderer: { elementId: 'output'/* , width: 500, height: 200  */},
  });
  
  const score = factory.EasyScore();
  
  factory
    .System()
    .addStave({
      voices: [
        score.voice(score.notes(convertedNotes, { stem: props.stem })), 
      ],
    })
    .addClef(props.clef)
    .addTimeSignature(props.compass);
    
  factory.draw();
})
</script>

<template>
  <div class="card">
    <div id="output" class="vexflow-container"></div>
  </div>
</template>

<style scoped>
.vexflow-container {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
  background-color: white;
  border-radius: 8px;
  padding: 1rem;
}
</style>