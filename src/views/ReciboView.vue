<template>
  <div class="recibo">
    <FormularioRecibo />
  </div>
</template>
<script>
// @ is an alias to /src

import FormularioRecibo from "@/components/FormRecibo.vue";

export default {
  name: "ReciboView",

  components: {
    FormularioRecibo,
  },
};
</script>
