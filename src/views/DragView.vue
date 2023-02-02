<template>
  <div
    :style="containerStyle"
    @drop="onDrop($event)"
    @dragenter.prevent
    @dragover.prevent
    ref="containerRef"
  >
    <div
      v-for="slide, index in slides"
      :key="slide.id"
      :style="slideContainerStyle"
      ref="itemRefs"
    >
      <div
        :style="slideStyle"
        draggable="true"
        @dragstart="handleDragStart($event, index)"
        @dragover="handleDragOver($event, index)"
        @dragleave="handleDragLeave($event, index)"
      >
        {{ slide.title }}
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */
import { ref, onBeforeUpdate } from 'vue';
import { useEventListener, useElementBounding } from '@vueuse/core';

export default {
  setup() {
    const itemRefs = ref([]);

    onBeforeUpdate(() => {
      itemRefs.value = [];
    });

    const containerRef = ref(null);

    const { top, bottom } = useElementBounding(containerRef);

    useEventListener(containerRef, 'drag', (e) => {
      // handle scroll here
    });

    return { itemRefs, containerRef };
  },
  data() {
    return {
      containerStyle: {
        margin: '20px 20px',
        width: '240px',
        border: '1px solid black',
        borderRadius: '4px',
        display: 'flex',
        flexDirection: 'column',
        gap: '4px',
        alignItems: 'center',
        overflow: 'hidden auto',
        height: '80vh',
        padding: '8px',
      },
      slideContainerStyle: {
        borderRadius: '4px',
        padding: '8px',
        width: '100%',
        height: '130px',
        position: 'relative',
      },
      slideStyle: {
        display: 'flex',
        alignItems: 'center',
        padding: '0 10px',
        height: '114px',
        borderRadius: '6px',
        justifyContent: 'center',
        background: '#FFF',
        border: '1px solid #e5e8eb',
        width: '100%',
        color: 'black',
      },
      slides: [
        {
          id: '1',
          title: 'Slide 1',
        },
        {
          id: '2',
          title: 'Slide 2',
        },
        {
          id: '3',
          title: 'Slide 3',
        },
        {
          id: '4',
          title: 'Slide 4',
        },
        {
          id: '5',
          title: 'Slide 5',
        },
        {
          id: '6',
          title: 'Slide 6',
        },
        {
          id: '7',
          title: 'Slide 7',
        },
        {
          id: '8',
          title: 'Slide 8',
        },
        {
          id: '9',
          title: 'Slide 9',
        },
        {
          id: '10',
          title: 'Slide 10',
        },
      ],
    };
  },
  computed: {
  },
  methods: {
    handleDragStart(event, index) {
    },
    onDrop(event) {
    },
    handleDragOver(event, index) {
    },
    handleDragLeave(event, index) {
    },
  },
};
</script>
