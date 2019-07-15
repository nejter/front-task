<template>
  <div class="outerWrapper">
    <div class="innerWrapper">
      <div class="listElement">
        <p><label>Name<input v-model="employee.name" /></label> </p>
        <p><label>Street<input v-model="employee.address.street" /></label></p>
        <p><label>Suite<input v-model="employee.address.suite" /></label></p>
        <p><label>City<input v-model="employee.address.city" /></label></p>
        <p><label>Phone<input v-model="employee.phone" /></label></p>
        <p><label>Email<input v-model="employee.email" /></label></p>
        <button @click="$emit('closeModal')">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    employee: Object
  }
};
</script>

<style lang="scss" scoped>
.outerWrapper {
  background: whitesmoke;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 30px 30px -10px rgba(0, 0, 0, 0.5)
}

.innerWrapper {
  display: flex;
  height: 100%;
  padding: 50px 50px 50px 50px;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

label {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  text-align: right;
  line-height: 26px;
  margin-bottom: 10px;
}

input {
  height: 20px;
  flex: 0 0 20px;
  margin-left: 10px;
}
</style>
