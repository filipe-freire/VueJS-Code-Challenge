<template>
  <button @click="btnToggle" class="btn" :class="btnType">
    <i :class="iconClass"></i
    >{{ btnType === "regular" ? defaultText : btnType }}
  </button>
</template>

<script>
export default {
  data() {
    return {
      defaultText: "open",
    };
  },
  props: {
    btnType: {
      type: String,
      required: true,
      default: "regular",
      validator: function(btnType) {
        //! The btnType must match one of these strings
        const enums = ["save", "cancel", "delete", "edit", "regular"];
        return enums.indexOf(btnType) !== -1;
      },
    },
    canHandleExpand: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    btnToggle(e) {
      const btnClasses = [...e.target.classList];

      if (btnClasses.includes("edit")) {
        return this.$emit("handle-edit");
      }

      if (btnClasses.includes("save")) {
        return this.$emit("handle-save");
      }

      if (btnClasses.includes("cancel")) {
        return this.$emit("handle-cancel");
      }

      if (btnClasses.includes("delete")) {
        return this.$emit("handle-delete");
      }

      if (this.canHandleExpand) {
        this.$emit("handle-expand");
        return (this.defaultText =
          this.defaultText === "open" ? "close" : "open");
      }
    },
  },
  computed: {
    iconClass() {
      if (this.btnType === "delete") {
        return "far fa-trash-alt";
      }
      if (this.btnType === "save") {
        return "far fa-save";
      }
      if (this.btnType === "cancel") {
        return "far fa-meh";
      }
      if (this.btnType === "edit") return "far fa-edit";
      return "regular";
    },
  },
};
</script>

<style scoped>
.far {
  margin-right: 0.7ch;
}

.btn {
  padding: 0.7rem 1rem;
  border-radius: 7px;
  font-family: inherit;
  font-weight: 400;
  color: #fff;
  text-transform: capitalize;
}
.btn + .btn {
  margin-left: 1ch;
}
.btn.regular {
  background-color: var(--bg-regular);
  border: 1px solid var(--bg-regular-border);
  color: #4b536b;
}
.btn.edit {
  background-color: var(--bg-edit);
  border: 1px solid var(--bg-edit-border);
}
.btn.delete {
  background-color: var(--bg-delete);
  border: 1px solid var(--bg-delete-border);
  padding: 0.7rem 2rem;
}
.btn.save {
  background-color: var(--bg-save);
  border: 1px solid var(--bg-save-border);
}
.btn.cancel {
  background-color: var(--bg-regular);
  border: 1px solid var(--bg-regular-border);
  color: #4b536b;
}
</style>
