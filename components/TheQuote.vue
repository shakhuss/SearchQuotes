<template>
  <div class="quote-entry">
      <div class="text-center">
          <h2>Quote: {{ item.quote }}</h2>
          <h3>Context: {{ item.context}}</h3>
          <h4>Source: {{ item.source }}</h4>
          <h4>Theme: {{ item.theme | capitalize}}</h4>

      </div>
  </div>
</template>

<script>
export default {
    name: 'TheQuote',
    filters: {
    capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
    }
    },
    props:{
      item:{
        type: Object,
        required: true
      }
    }
};
</script>

<style>
.quote-entry{
    box-shadow: rgba(0, 0, 0, 0.117647) 0px 1px 6px, rgba(0, 0, 0, 0.117647) 0px 1px 4px;
    max-width: 60%;
    margin: auto;
    align-content: center;
}


</style>