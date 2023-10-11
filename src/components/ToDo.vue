<template>
  <div class="general">
    <div class="box">
      {{ text }}
    </div>
    <div>
      <input type="checkbox" name="check" id="check" v-model="check" />
      <button @click="deleteToDo">
        <img
          src="https://static.vecteezy.com/system/resources/previews/016/775/743/non_2x/red-cross-isolated-png.png"
          alt="supprimer"
          width="10px"
          height="10px"
        />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  computed: {
    check: {
      set(value) {
        this.$emit("checked", { id: this.id, value });
      },
      get() {
        return this.checked;
      },
    },
  },
  props: {
    text: String,
    checked: Number,
    id: Number,
  },

  methods: {
    deleteToDo() {
      this.$emit("delete", this.id);
    },
  },
};
</script>

<style>
.general {
  display: flex;
}
.box {
  border: 1px solid black;
  width: 60%;
}
</style>
