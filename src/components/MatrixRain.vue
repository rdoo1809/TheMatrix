<template>
  <canvas ref="canvas" class="matrix-rain"></canvas>
</template>

<script lang="ts">
import { onBeforeUnmount } from 'vue'

export default {
  name: 'MatrixRain',
  mounted() {
    const canvas = this.$refs.canvas as HTMLCanvasElement
    const ctx = canvas.getContext('2d')!

    // Set canvas size to window size
    const resize = () => {
      canvas.width = window.innerWidth
      canvas.height = window.innerHeight
    }
    window.addEventListener('resize', resize)
    resize()

    // Matrix characters
    const chars = '日ﾊﾐﾋｰｳｼﾅﾓﾆｻﾜﾂｵﾘｱﾎﾃﾏｹﾒｴｶｷﾑﾕﾗｾﾈｽﾀﾇﾍ'.split('')
    const columns = Math.floor(canvas.width / 20)
    const drops: number[] = new Array(columns).fill(1)
    const speed = 0.5 // Slower speed factor

    function draw() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
      ctx.fillRect(0, 0, canvas.width, canvas.height)

      ctx.fillStyle = '#0F0'
      ctx.font = '20px monospace'

      for (let i = 0; i < drops.length; i++) {
        const text = chars[Math.floor(Math.random() * chars.length)]
        ctx.fillText(text, i * 20, drops[i] * 20)

        if (drops[i] * 20 > canvas.height && Math.random() > 0.975) {
          drops[i] = 0
        }
        drops[i] += speed // Apply speed factor
      }
    }

    const interval = setInterval(draw, 50) // Increased interval from 33 to 50ms

    onBeforeUnmount(() => {
      clearInterval(interval)
      window.removeEventListener('resize', resize)
    })
  },
}
</script>

<style scoped>
.matrix-rain {
  position: fixed;
  top: 0;
  left: 0;
  z-index: -1;
  background: black;
}
</style>
