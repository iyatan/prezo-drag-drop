<template>
  <div>
    <div :style="controlStyle">
      <div>
        <button :style="buttonStyle" @click="undo">Undo</button>
        <p>Press the 'u' key</p>
      </div>
      <div>
        <button :style="buttonStyle" @click="redo">Redo</button>
        <p>Press the 'r' key</p>
      </div>
    </div>

    <div
      :style="containerStyle"
      @drop="onDrop($event)"
      @dragenter.prevent
      @dragover.prevent
      ref="containerRef"
    >
      <div
        v-for="(slide, index) in slides"
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
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */
import { ref, onBeforeUpdate } from "vue";
import { useEventListener, useElementBounding } from "@vueuse/core";

export default {
  setup() {
    const itemRefs = ref([]);
    onBeforeUpdate(() => {
      itemRefs.value = [];
    });
    const containerRef = ref(null);
    const { top, bottom } = useElementBounding(containerRef);

    useEventListener(containerRef, "drag", (e) => {
      // handle scroll here
      if (e.clientY < top.value + 50) {
        containerRef.value.scrollTop -= 10;
      }
      if (e.clientY > bottom.value - 50) {
        containerRef.value.scrollTop += 10;
      }
    });

    return { itemRefs, containerRef };
  },
  data() {
    return {
      dropIndex: null,
      containerStyle: {
        margin: "20px 20px",
        width: "240px",
        border: "1px solid black",
        borderRadius: "4px",
        display: "flex",
        flexDirection: "column",
        gap: "4px",
        alignItems: "center",
        overflow: "hidden auto",
        height: "80vh",
        padding: "8px",
      },
      slideContainerStyle: {
        borderRadius: "4px",
        padding: "8px",
        width: "100%",
        height: "130px",
        position: "relative",
      },
      slideStyle: {
        display: "flex",
        alignItems: "center",
        padding: "0 10px",
        height: "114px",
        borderRadius: "6px",
        justifyContent: "center",
        background: "#FFF",
        border: "1px solid #e5e8eb",
        width: "100%",
        color: "black",
      },
      controlStyle: {
        display: "flex",
        flexDirection: "row",
        justifyContent: "space-between",
        textAlign: "center",
      },
      buttonStyle: {
        padding: "8px 16px",
        borderRadius: "4px",
        border: "1px solid #342DF2",
        background: "#342DF2",
        color: "white",
        cursor: "pointer",
      },
      slides: [
        {
          id: "1",
          title: "Slide 1",
        },
        {
          id: "2",
          title: "Slide 2",
        },
        {
          id: "3",
          title: "Slide 3",
        },
        {
          id: "4",
          title: "Slide 4",
        },
        {
          id: "5",
          title: "Slide 5",
        },
        {
          id: "6",
          title: "Slide 6",
        },
        {
          id: "7",
          title: "Slide 7",
        },
        {
          id: "8",
          title: "Slide 8",
        },
        {
          id: "9",
          title: "Slide 9",
        },
        {
          id: "10",
          title: "Slide 10",
        },
      ],
      history: [],
      currentIndex: -1,
    };
  },

  created() {
    this.updateHistory();
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener("keyup", (event) => {
        if (event.key === "u") {
          this.undo();
        } else if (event.key === "r") {
          this.redo();
        }
      });
    });
  },

  methods: {
    handleDragStart(event, index) {
      event.dataTransfer.setData("text", index.toString());
      event.target.style.backgroundColor = "lightgray";
    },
    onDrop(event) {
      event.target.parentNode.style.borderTop = "";
      event.target.parentNode.style.borderBottom = "";
      event.preventDefault();
      const oldIndex = parseInt(event.dataTransfer.getData("text"), 10);

      const newIndex = this.dropIndex;

      if (oldIndex === newIndex) return;

      const slides = [...this.slides];
      let removedElement = slides.splice(oldIndex, 1)[0];
      slides.splice(newIndex, 0, removedElement);
      this.slides = slides;

      this.updateHistory();
    },
    handleDragOver(event, index) {
      this.dropIndex = index;

      event.preventDefault();
      if (index >= 4) {
        event.target.parentNode.style.borderBottom = "4px solid #342DF2";
      } else {
        event.target.parentNode.style.borderTop = "4px solid #342DF2";
      }
    },
    handleDragLeave(event) {
      event.target.parentNode.style.borderTop = "";
      event.target.parentNode.style.borderBottom = "";
    },
    updateHistory() {
      // Save current state to history
      this.history = this.history.slice(0, this.currentIndex + 1);
      this.history.push(this.slides);
      this.currentIndex++;
    },
    undo() {
      if (this.currentIndex <= 0) return;

      // Restore previous state from history
      this.currentIndex--;
      this.slides = this.history[this.currentIndex];
    },
    redo() {
      if (this.currentIndex === this.history.length - 1) return;

      // Restore next state from history
      this.currentIndex++;
      this.slides = this.history[this.currentIndex];
    },
  },
};
</script>
