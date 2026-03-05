<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { ChevronRight, ChevronLeft, Linkedin } from 'lucide-vue-next'

const { team } = useContent()

const carousel = ref<HTMLElement | null>(null)
const canScrollLeft = ref(false)
const canScrollRight = ref(true)

function scroll(dir: 'left' | 'right') {
  if (!carousel.value) return
  const amount = 280
  carousel.value.scrollBy({
    left: dir === 'right' ? amount : -amount,
    behavior: 'smooth',
  })
}

function checkScroll() {
  if (!carousel.value) return
  canScrollLeft.value = carousel.value.scrollLeft > 10
  canScrollRight.value =
    carousel.value.scrollLeft < carousel.value.scrollWidth - carousel.value.clientWidth - 10
}

onMounted(() => {
  if (carousel.value) {
    carousel.value.addEventListener('scroll', checkScroll, { passive: true })
    checkScroll()
  }
})
</script>

<template>
  <section
    class="team-section h-full relative flex flex-col justify-center overflow-hidden"
  >
    <div class="max-w-[1440px] mx-auto px-6 lg:px-12 w-full relative z-10">
      <UiSectionTitle :title="team.sectionTitle" class="parallax-title" />

      <div class="relative mt-8">
        <!-- Carousel -->
        <div
          ref="carousel"
          class="carousel flex gap-6 overflow-x-auto scrollbar-hide pb-4 snap-x snap-mandatory"
        >
          <div
            v-for="(member, idx) in team.members"
            :key="member.name"
            class="team-card flex-shrink-0 snap-start parallax-card"
            :style="{ '--card-delay': idx }"
          >
            <a :href="member.linkedin" class="linkedin-icon" aria-label="LinkedIn">
              <Linkedin :size="20" />
            </a>
            <h4 class="team-name font-heading">{{ member.name }}</h4>
            <p class="team-role font-body">{{ member.role }}</p>
          </div>
        </div>

        <!-- Navigation arrows -->
        <button
          v-show="canScrollLeft"
          class="carousel-arrow carousel-arrow--left"
          :aria-label="team.prevLabel"
          @click="scroll('left')"
        >
          <ChevronLeft :size="28" />
        </button>
        <button
          v-show="canScrollRight"
          class="carousel-arrow carousel-arrow--right"
          :aria-label="team.nextLabel"
          @click="scroll('right')"
        >
          <ChevronRight :size="28" />
        </button>
      </div>
    </div>
  </section>
</template>

<style scoped lang="scss">
.team-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #0d0b2e;
  border-radius: 16px;
  padding: clamp(1.5rem, 3vw, 2.5rem) clamp(1.25rem, 2vw, 2rem);
  min-width: 180px;
  width: clamp(180px, 25vw, 240px);
  gap: 0.75rem;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.team-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 40px rgba(123, 63, 160, 0.3);
}

.linkedin-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: white;
  color: #0d0b2e;
  transition: all 0.3s;
}
.linkedin-icon:hover {
  background: var(--color-accent-magenta);
  color: white;
}

.team-name {
  font-size: clamp(0.9rem, 1.5vw, 1.15rem);
  font-weight: 700;
  color: white;
  text-align: center;
}

.team-role {
  font-size: clamp(0.75rem, 1.2vw, 0.85rem);
  color: var(--color-text-light);
  text-align: center;
  font-style: italic;
}

.carousel-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(8px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  color: white;
  cursor: pointer;
  transition: all 0.3s;
  z-index: 2;
}
.carousel-arrow:hover {
  background: rgba(255, 255, 255, 0.2);
}
.carousel-arrow--left {
  left: -20px;
}
.carousel-arrow--right {
  right: -20px;
}

.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}

/* Parallax d'entrée */
.parallax-title {
  opacity: 0;
  transform: translateY(60px);
  transition: opacity 0.9s ease, transform 1s ease;
}
.parallax-card {
  opacity: 0;
  transform: translateY(120px);
  transition: opacity 0.9s ease, transform 1.2s cubic-bezier(0.22, 1, 0.36, 1);
  transition-delay: calc(var(--card-delay, 0) * 0.08s + 0.15s);
}
/* Activation via main.scss : #equipe.swiper-slide-active */
</style>
