<template>
  <div>
    <div class="background">
      <div class="background__background" v-on:click="$refs.inputref.focus()">
        <input
          type="text"
          ref="inputref"
          v-model="newmyinput"
          v-on:input="$emit('myinput', newmyinput)"
        />
        <div class="button" v-on:click="clining"></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['newmyinput'],
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    clining: function () {
      this.$emit("cleaninput", this.counter++);
      this.newmyinput = "";
    },
  },
  mounted () {
    this.$refs.inputref.focus()
  }
};
</script>
<style scoped>
.background {
  background-color: rgb(46, 46, 46);
  min-height: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 22px;
}

.background__background {
  max-width: 1300px;
  width: 100%;
  position: relative;
  margin: auto;
}

input {
  min-height: 45px;
  max-width: 1300px;
  width: 100%;
  background-color: rgb(109, 109, 109);
  border-radius: 10px;
  position: relative;
  margin: auto;
  border: 0px solid black;
  font-size: 30px;
  color: white;
  padding-left: 20px;
  box-sizing: border-box;
}

input:focus {
  outline: none;
}

.button {
  width: 30px;
  height: 30px;
  background-color: rgb(170, 170, 170);
  border-radius: 100%;
  top: 50%;
  right: 0;
  transform: translate(-50%, -50%);
  position: absolute;
}

.button::after {
  content: "";
  display: block;
  width: 3px;
  height: 21px;
  background-color: rgb(109, 109, 109);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(-45deg);
  border-radius: 1px;
}
.button::before {
  content: "";
  display: block;
  width: 3px;
  height: 21px;
  background-color: rgb(109, 109, 109);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
  border-radius: 1px;
}
</style>