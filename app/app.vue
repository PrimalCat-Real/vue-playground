<script setup>
const SIZE = 32;
const colors = ref([
  { r: 255, g: 0, b: 0, id: 1 },
  { r: 0, g: 255, b: 0, id: 2 },
  { r: 0, g: 0, b: 255, id: 3 }
])
let lastId = 4
const addBlock = () => {
  colors.value.push({r: Math.random()*255, g: Math.random()*255, b: Math.random()*255, id: lastId++})
}

const resetBloks = () => {
  colors.value = []
}

const shuffleBlocks = () => {
  let newColors = [...colors.value]
  for(let index = 0; index < newColors.length; index++ ){
    const randomIndex = getRandomIntInclusive(
      index,
      newColors.length - 1
    )
    const randomColor = newColors[randomIndex]
    newColors[randomIndex] = newColors[index]
    newColors[index] = randomColor
  }
  colors.value = newColors
  // mergBlocks(colors.value, newColors)
}
const mergBlocks = (oldColors, newColors) => {

}

const renderedColors = computed(() => {
  return colors.value.map((color, index) => ({
    ...color,
    position: index
  })).sort((first, second) => first.id - second.id)
})

// const shuffleBlocksV1 = () => {
//   let newColors = [...colors.value]
//   for(let index = 0; index < 3; index++ ){
//     const separatorIndex= getRandomIntInclusive(0, newColors.length)
//     const firstPart = newColors.slice(0, separatorIndex)
//     const secondPart = newColors.slice(separatorIndex, newColors.length)
//     newColors = [...secondPart, ...firstPart]
//   }
//   colors.value = newColors
// }
function getRandomIntInclusive(min, max) {
  const minCeiled = Math.ceil(min);
  const maxFloored = Math.floor(max);
  return Math.floor(Math.random() * (maxFloored - minCeiled + 1) + minCeiled);
}

const GAP = 40

</script>
<template>
  <div id="colors" class="min-h-40 w-full border border-slate-500 relative ">
      <!-- <div class="h-8 w-8 absolute transition-transform duration-1000" 
      :style="{backgroundColor: `rgb(${color.r}, ${color.g}, ${color.b})`,
        transform: `translate(${GAP*index}px)`,
        border: `1px solid rgb(${color.r*0.7}, ${color.g*0.7}, ${color.b*0.7})`}" 
      :key="color.id"
      v-for="(color, index) in colors" 
      >
      </div> -->
      <div  class="h-8 w-8 absolute transition-transform duration-1000" :style="{ transform: `translate(${GAP*color.position}px)`, backgroundColor: `rgb(${color.r}, ${color.g}, ${color.b})`,  border: `1px solid rgb(${color.r*0.7}, ${color.g*0.7}, ${color.b*0.7})`}" :key="color.id" v-for="color in renderedColors">

      </div>
      
  </div>
  <div class="flex gap-2">
    <button @click="addBlock">Add</button>
    <button @click="resetBloks">Reset</button>
    <button @click="shuffleBlocks">Shuffle</button>
  </div>
  
</template>

