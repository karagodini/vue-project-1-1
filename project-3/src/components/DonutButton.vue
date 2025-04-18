<template>
  <div
    class="donut-wrapper"
    :style="{ width: size + 'px', height: size + 'px' }"
    @click="onClick"
  >
    <svg
      :width="size"
      :height="size"
      viewBox="0 0 100 100"
      class="donut-svg"
    >
      <!-- Маска: оставляет видимой только кольцевую область -->
      <defs>
        <mask id="donut-mask">
          <!-- всё белое = видно, чёрное = прозрачное -->
          <rect width="100" height="100" fill="white" />
          <circle
            cx="50"
            cy="50"
            :r="innerRadius"
            fill="black"
          />
        </mask>
      </defs>

      <!-- Основной круг, в котором будет дырка -->
      <circle
        cx="50"
        cy="50"
        :r="outerRadius"
        fill="var(--donut-color, #1976d2)"
        mask="url(#donut-mask)"
      />
    </svg>

    <!-- Перекрытие центра, чтобы блокировать клики -->
    <div
      class="donut-hole-blocker"
      :style="{
        width: innerPx + 'px',
        height: innerPx + 'px',
        left: centerOffset + 'px',
        top: centerOffset + 'px'
      }"
      @click.stop
    ></div>

    <!-- Контент -->
    <v-btn
      class="donut-btn"
      variant="text"
      :style="{ width: size + 'px', height: size + 'px' }"
      disabled
    >
      <slot />
    </v-btn>
  </div>
</template>

<script>
export default {
  name: 'DonutButton',
  props: {
    size: {
      type: Number,
      default: 120
    },
    outerRadius: {
      type: Number,
      default: 45
    },
    innerRadius: {
      type: Number,
      default: 25
    }
  },
  computed: {
    innerPx() {
      return (this.innerRadius / 50) * (this.size / 2) * 2;
    },
    centerOffset() {
      return (this.size - this.innerPx) / 2;
    }
  },
  methods: {
    onClick(event) {
      const rect = event.currentTarget.getBoundingClientRect();
      const x = event.clientX - rect.left - rect.width / 2;
      const y = event.clientY - rect.top - rect.height / 2;
      const distance = Math.sqrt(x * x + y * y);

      const outer = this.size / 2 * (this.outerRadius / 50);
      const inner = this.size / 2 * (this.innerRadius / 50);

      if (distance > inner && distance < outer) {
        this.$emit('click', event);
      }
    }
  }
}
</script>

<style scoped>
.donut-wrapper {
  position: relative;
  display: inline-block;
  cursor: pointer;
  user-select: none;
}

.donut-svg {
  position: relative;
  z-index: 1;
}

.donut-hole-blocker {
  position: absolute;
  background-color: transparent;
  border-radius: 50%;
  pointer-events: all;
  z-index: 3;
}

.donut-btn {
  position: absolute;
  top: 0;
  left: 0;
  pointer-events: none;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #b0b0b0;
  font-weight: bold;
  text-align: center;
  z-index: 2;
}
</style>
