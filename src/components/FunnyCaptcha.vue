<script setup lang="ts">
import { onMounted, ref } from "vue";

// ref is voor te werken met primitieve data werken (boolian, string,...), meer aangeraden dan reactive
  const captchaElements = ref([
    {
      image: 'https://media-cldnry.s-nbcnews.com/image/upload/t_fit-1500w,f_auto,q_auto:best/streams/2014/March/140317/2D274905391507-today-tech-grandmabetty-140317.jpg',
      isSelected: false,
      isFunny: true
    },
    {
      image: 'https://static.independent.co.uk/s3fs-public/thumbnails/image/2014/08/04/12/betty-simpson.jpg?quality=75&width=982&height=726&auto=webp',
      isSelected: false,
      isFunny: true
    },
    {
      image: 'https://www.gottabemobile.com/wp-content/uploads/grandma-computer-1280x720.jpg',
      isSelected: false,
      isFunny: true
    },
    {
      image: 'http://i.kinja-img.com/gawker-media/image/upload/s--eohLVwaE--/c_fit,fl_progressive,q_80,w_636/ibtgbfp6llb7gjtpbjt7.jpg',
      isSelected: false,
      isFunny: false
    }
  ])

  // const props = defineProps(['seconds'])
  const props = defineProps({
    seconds: {
      type: [Number, String],
      required: false,
      default: 0
    }
  })

  const emit = defineEmits(['correct', 'incorrect']);

  const secondsleft = ref(props.seconds as number);

  onMounted(() => {
      console.log("On Mounted ...");
      if (props.seconds > 0){
        const timer = setInterval(() => {
          secondsleft.value--;
          if(secondsleft.value <= 0){
            clearInterval(timer);
            confirm_click();
          }
        }, 1000)
      }
    })

  function confirm_click(){
    console.log("Confirmed!");
    const isCorrect = captchaElements.value.reduce((result, obj) => result && (obj.isFunny === obj.isSelected), true);

    //Sum of all elements
    // const sumWithInitial = array1.reduce(
    //(previousValue, currentValue) => previousValue + currentValue, initialValue
    //)

    // if (isCorrect) (
    //   alert("it works!")
    // )

    emit(isCorrect ? 'correct' : 'incorrect');
}

  // const confirm_click = function() {
  //   console.log("Confirmed")
  // }
</script>

<template>
  <div>
    <h1>Captcha view - Select the funny grannies</h1>
    <p v-if="props.seconds > 0">You have {{secondsleft}} seconds left to decide if you're human.</p>
    <img class="image" v-for="(item, i) in captchaElements" :src="item.image" :key="`cpt${i}`" :class="{selected: item.isSelected}" @click="item.isSelected = !item.isSelected"/>
    <button @click="confirm_click">Confirm</button>
  </div>
</template>

<style scoped>
  .image {
    width: 200px;
    height: 200px;
    margin: 5px;
    border: solid white 3px;
    object-fit: fill;
  }
  .selected {
    border: solid lime 4px;
  }

  button {
    background: pink;
    color: black;
    border: solid white 2px;
  }

</style>