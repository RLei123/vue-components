<template>
  <div class="my-form">
    <Form>
      <template>
        <div v-for="item in formJson" :key="item.Title">
          <label v-if="item.Type==1">
            {{item.Title}}
            <Input type="text" :placeholder="item.Title" />
          </label>

          <label v-if="item.Type==2">
            {{item.Title}}
            <label v-for="v in item.Values" :key="v">
              <Input type="radio" />
              {{v}}
            </label>
          </label>

          <label v-if="item.Type==3">
            {{item.Title}}
            <label v-for="v in item.Values" :key="v">
              <Input type="checkbox" />
              {{v}}
            </label>
          </label>
        </div>
      </template>
    </Form>
  </div>
</template>

<script>
export default {
  name: "",
  props: {
    formStr: {
      type: String,
      required: true
    }
  },
  computed: {
    formJson() {
      return JSON.parse(this.formStr);
    }
  },
  data() {
    return {};
  }
};
</script>

<style scoped>

</style>