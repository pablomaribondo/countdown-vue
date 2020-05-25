<template>
  <div>
    <Counter
      :endYear="2020"
      :endMonth="5"
      :endDate="25"
      :endHour="9"
      :endMinute="53"
      :endSecond="59"
      :endMillisecond="31"
    />
  </div>
</template>

<script>
import Counter from '@/components/Counter.vue';

export default {
  components: {
    Counter,
  },
};
</script>

<style>
</style>
