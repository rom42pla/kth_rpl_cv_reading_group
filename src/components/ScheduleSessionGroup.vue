<template>
  <div class="py-3 border-bottom">
    <div class="container">
      <div class="row">
        <div class="col col-12 col-lg-6 text-muted small mb-2">📅 {{ formattedDate }}</div>
        <div class="col col-12 col-lg-6 text-muted small mb-2">📍 {{ venue }}</div>
      </div>

      <div
        v-for="(session, index) in sessions"
        :key="session.session_number ?? index"
        :class="{ 'pt-3 mt-2 border-top': index > 0 }"
      >
        <!-- topic -->
        <div v-if="session.topic && index === 0" class="row">
          <div class="col text-muted mb-1 small">🏷️ {{ session.topic }} topic</div>
        </div>
        <div class="row">
          <div class="col col-12 col-lg-6 text-muted small mb-1">🕐 {{ session.time }}</div>
          <div v-if="session.presenter" class="col col-12 col-lg-6 text-muted small mb-1">
            🎤 {{ session.presenter }}
          </div>
        </div>
        <!-- paper details -->
        <div class="row">
          <div class="col lead mb-1">
            {{ session.paper_title }}
          </div>
        </div>
        <div class="row">
          <div v-if="session.paper_authors" class="col col-12 col-lg-6 text-muted small mb-1">
            {{ session.paper_authors }}
          </div>
          <div v-if="session.paper_venue" class="col col-12 col-lg-4 text-muted small mb-1">
            {{ session.paper_venue }}, {{ session.paper_year }}
          </div>
        </div>
        <!-- buttons -->
        <div class="row">
          <div class="col mb-1">
            <a
              v-if="session.paper_link"
              :href="session.paper_link"
              class="btn btn-sm btn-primary me-2"
              target="_blank"
            >
              Read paper
            </a>
            <a
              v-if="session.arxiv_link"
              :href="session.arxiv_link"
              class="btn btn-sm me-2"
              style="background-color: #b31b1b; color: white"
              target="_blank"
            >
              Read paper on arXiv
            </a>
            <a
              v-if="session.slides_link"
              :href="session.slides_link"
              class="btn btn-sm btn-secondary me-2"
              target="_blank"
            >
              Read slides
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

interface Session {
  session_number: number
  date: string
  time: string | null
  venue: string | null
  topic: string | null
  presenter: string | null
  paper_authors: string | null
  paper_title: string | null
  paper_venue: string | null
  paper_year: string | null
  slides_link: string | null
  paper_link: string | null
  arxiv_link: string | null
}

const props = defineProps<{
  sessions: Session[]
}>()

const formattedDate = computed(() => {
  const date = props.sessions[0]?.date
  if (!date) return 'Date TBA'
  return new Date(date).toLocaleDateString('en-GB', {
    weekday: 'long',
    day: 'numeric',
    month: 'long',
    year: 'numeric',
  })
})

const venue = computed(() => props.sessions[0]?.venue ?? 'venue TBA')
</script>
