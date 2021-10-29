<template>
  <div class="d-flex justify-content-center">
    
    <span class="btn btn-light">ToDo {{todo}}</span>
    <span class="btn btn-light">InProgress {{progress}}</span>
    <span class="btn btn-light">Finished {{finished}}</span>
  </div>
</template>

<script>
export default {
  name: 'TaskProgress',
  props: {todo: Number, progress: Number, finished: Number},
}
</script>