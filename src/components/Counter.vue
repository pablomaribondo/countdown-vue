<template>
  <div v-if="loaded">
    <section class="text-3xl flex justify-center content-center flex-col mx-auto text-center">
      <h5 v-if="!expired">Buy now</h5>
      <h5 v-else>Timer is done!</h5>
    </section>
    <section class="text-6xl flex justify-center content-center">
      <div class="days mr-2 relative">
        {{ displayDays }}
        <div class="label text-sm absolute bottom-0">days</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="hours mr-2 relative">
        {{ displayHours }}
        <div class="label text-sm absolute bottom-0">hours</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="minutes mr-2 relative">
        {{ displayMinutes }}
        <div class="label text-sm absolute bottom-0">minutes</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="seconds mr-2 relative">
        {{ displaySeconds }}
        <div class="label text-sm absolute bottom-0">seconds</div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  props: ['endYear', 'endMonth', 'endDate', 'endHour', 'endMinute', 'endSecond', 'endMillisecond'],
  data: () => ({
    displayDays: '00',
    displayHours: '00',
    displayMinutes: '00',
    displaySeconds: '00',
    loaded: false,
    expired: false,
  }),
  computed: {
    seconds: () => 1000,
    minutes() { return this.seconds * 60; },
    hours() { return this.minutes * 60; },
    days() { return this.hours * 24; },
    end() {
      return new Date(
        this.endYear,
        this.endMonth,
        this.endDate,
        this.endHour,
        this.endMinute,
        this.endSecond,
        this.endMillisecond,
      );
    },
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();
        const distance = this.end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.expired = true;
          this.loaded = true;
          return;
        }

        const days = Math.floor(distance / this.days);
        const hours = Math.floor((distance % this.days) / this.hours);
        const minutes = Math.floor((distance % this.hours) / this.minutes);
        const seconds = Math.floor((distance % this.minutes) / this.seconds);

        this.displayDays = days.toString().padStart(2, '0');
        this.displayHours = hours.toString().padStart(2, '0');
        this.displayMinutes = minutes.toString().padStart(2, '0');
        this.displaySeconds = seconds.toString().padStart(2, '0');

        this.loaded = true;
      }, 1000);
    },
  },
};
</script>

<style>
.seconds {
  max-width: 60pxs;
}
</style>
