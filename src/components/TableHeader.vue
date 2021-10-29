<template>
  <th scope="col" :style="{ 'width': width + 'px' }">{{label}}</th>
</template>

<script>
export default {
  name: 'TableHeader',
  props: {type: String, label: String, width: Number},
  mounted() {
  }
}
</script>