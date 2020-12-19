<template>
  <div class="wrap">
    <header>
      <h2>profile</h2>
    </header>
    <section>
      <div class="content">
        <div class="left">
          <img src="../assets/img1.jpeg"/>
        </div>
        <div class="right">This command will add nuxt as a dependency to your project and add it to your package.json.
          The node_modules directory will also be created which is where all your installed packages and dependencies
          are stored.
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "profile"
}
</script>

<style scoped>
.wrap {
  width: 100vw;
  height: 100vh;
  background-color: #526488;
  display: flex;
  flex-direction: column;
}

header {
  width: 100%;
  height: 10%;
  background-color: #35495e;
}

.content {
  display: flex;
}

.content .left {
  width: 30%;
  height: 100%;
}
.content .right{
  width: 70%;
}

.content .left img{
  width: 100%;
}

</style>
