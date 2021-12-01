<template>
  <div class="body">
    <slot />
    <div
      class="toggle"
      :class="[state_class]"
      @click.self="onClick"
    >
      <div
        class="draggable"
        :style="style"
        @mousedown.prevent="dragStart"
      />
    </div>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      width: 100,
      state: false,
      pressed: 0,
      position: 0
    }
  },
  computed: {
    style () {
      return {
        transform: `translateX(${this.pos_percentage})`
      }
    },
    pos_percentage () {
      return `${this.position / this.width * 100}%`
    },
    state_class () {
      if (this.state) {
        return 'active'
      }
      return ''
    }
  },
  watch: {
    position () {
      this.state = this.position >= 50
    }
  },
  mounted () {
    this.toggle(this.value)
  },
  methods: {
    onClick () {
      this.toggle(!this.state)
      this.emit()
    },
    toggle (state) {
      this.state = state
      this.position = !state
        ? 0
        : 100
    },
    dragging (e) {
      const pos = e.clientX - this.$el.offsetLeft
      const percent = pos / this.width * 100
      this.position = percent <= 0
        ? 0
        : percent >= 100
          ? 100
          : percent
    },
    dragStart (e) {
      this.startTimer()
      window.addEventListener('mousemove', this.dragging)
      window.addEventListener('mouseup', this.dragStop)
    },
    dragStop () {
      window.removeEventListener('mousemove', this.dragging)
      window.removeEventListener('mouseup', this.dragStop)
      this.resolvePosition()
      clearInterval(this.$options.interval)
      if (this.pressed < 30) {
        this.toggle(!this.state)
      }
      this.pressed = 0
      this.emit()
    },
    startTimer () {
      this.$options.interval = setInterval(() => {
        this.pressed++
      }, 1)
    },
    resolvePosition () {
      this.position = this.state
        ? 100
        : 0
    },
    emit () {
      this.$emit('input', this.state)
    }
  }
}

</script>

<style scoped>

.toggle {
  width: 40px;
  height: 20px;
  background: #CEBCBD;
  border: 2px solid #ddd;
  border-radius: 200px;
  transition: background 0.6s;
}
.toggle .draggable {
  width: 18px;
  height: 17px;
  background: #fff;
  border-radius: 100%;
  box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.6);
  transform: translateX(0%);
  transition: transform 0.05s ease-in-out;
}
.toggle.active {
  background: #72d09c;
  transition: background 0.6s;
}

.app {
  display: flex;
}

.switches {
  margin-right: 30px;
}

pre {
  margin: 0;
  background: #513d56;
  color: #efefef;
  padding: 20px;
  border-radius: 6px;
  width: 200px;
}

.body {
  margin: 15px 0;
}
</style>
