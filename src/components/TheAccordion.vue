<script setup lang="ts">
import { ref } from 'vue'
import ChevronDown from '@/assets/chevron-down.svg'
import { type Accordion } from '@/types/Accordion'

// export interface Props {
//   msg?: string
//   labels?: string[]
// }

// const props = withDefaults(defineProps<Props>(), {
//   msg: 'hello',
//   labels: () => ['one', 'two']
// })
// interface test {
//   isMultipleOpen: boolean
// }

const props = defineProps<{
  isMultipleOpen: boolean
}>()
//specific typecasting requires boolean prop to be passed specifically, so " The Boolean absent props will be cast to  ##false## " - doesn't count in this case.

// When defining a prop using defineProps with TypeScript in Vue 3, you are specifying the expected type for the prop. In your case, you've defined isMultipleOpen as a Boolean. However, if this prop is not provided when using the component <TheAccordion />, TypeScript will assume it's undefined.

// According to the documentation you mentioned, if a Boolean prop is absent, it should be cast to false. However, TypeScript is strict about types, and undefined is not automatically cast to false in TypeScript.

// const props = defineProps({
//   isMultipleOpen: Boolean
// })

// interface Accordion {
//   id: number
//   isOpen: boolean
//   content: string
// }

const list = ref<Accordion[]>([
  {
    id: 1,
    isOpen: false,
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec turpis libero, finibus sit amet aliquam quis, mollis laoreet turpis. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Integer suscipit erat vitae nunc maximus maximus a eu nisi. Duis lacus erat, tempus id diam ac, sodales finibus mauris. Sed egestas nibh non nulla laoreet feugiat. Ut in dapibus leo, ut pellentesque purus. Pellentesque efficitur egestas orci'
  },
  {
    id: 2,
    isOpen: false,
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec turpis libero, finibus sit amet aliquam quis, mollis laoreet turpis. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Integer suscipit erat vitae nunc maximus maximus a eu nisi. Duis lacus erat, tempus id diam ac, sodales finibus mauris. Sed egestas nibh non nulla laoreet feugiat. Ut in dapibus leo, ut pellentesque purus. Pellentesque efficitur egestas orci'
  },
  {
    id: 3,
    isOpen: false,
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec turpis libero, finibus sit amet aliquam quis, mollis laoreet turpis. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Integer suscipit erat vitae nunc maximus maximus a eu nisi. Duis lacus erat, tempus id diam ac, sodales finibus mauris. Sed egestas nibh non nulla laoreet feugiat. Ut in dapibus leo, ut pellentesque purus. Pellentesque efficitur egestas orci'
  }
])

function activate(item: Accordion) {
  item.isOpen = !item.isOpen
  console.log(props.isMultipleOpen)
  if (!props.isMultipleOpen) {
    list.value.forEach((obj) => {
      if (obj.id !== item.id) {
        obj.isOpen = false
      }
    })
  }
}
</script>

<template>
  <ul class="w-96 rounded-sm shadow-md divide-y divide-dashed">
    <li v-for="item in list" :key="item.id">
      <div
        class="py-2.5 px-6 flex justify-between cursor-pointer hover:bg-gray-100"
        @click="activate(item)"
        :class="{ 'bg-gray-100': item.isOpen }"
      >
        <p>{{ `Control ${item.id}` }}</p>

        <ChevronDown
          :class="{ 'rotate-180': item.isOpen }"
          class="transition duration-300 ease-in-out"
        />
      </div>
      <transition name="expand">
        <div class="text-black py-2.5 px-6" v-show="item.isOpen">
          {{ item.content }}
        </div>
      </transition>
    </li>
  </ul>
</template>

<style>
.ease-custom {
  transition-timing-function: cubic-bezier(0.61, -0.53, 0.43, 1.43);
}
</style>
