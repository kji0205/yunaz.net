<template>
  <div>
    <h1>item38</h1>
  </div>
</template>

<script>
export default {
  name: "item38",
  mounted() {


    function Scene(context, width, height, images) {
      this.context = context;
      this.width = width
      this.height = height
      this.images = images
      this.actors = []
    }

    Scene.prototype.register = function (actor) {
      this.actors.push(actor)
    }
    Scene.prototype.unregister = function (actor) {
      let i = this.actors.indexOf(actor)
      if (i >= 0) {
        this.actors.splice(i, 1)
      }
    }
    Scene.prototype.draw = function () {
      this.context.clearRect(0, 0, this.width, this.height)
      for (let a = this.actors, i = 0, n = a.length; i < n; i++) {
        a[i].draw()
      }
    }

    function Actor(scene, x, y) {
      this.scene = scene
      this.x = x
      this.y = y
      scene.register(this)
    }

    Actor.prototype.moveTo = function (x, y) {
      this.x = x
      this.y = y
      this.scene.draw()
    }
    Actor.prototype.exit = function () {
      this.scene.unregister(this)
      this.scene.draw()
    }
    Actor.prototype.draw = function () {
      let image = this.scene.images[this.type]
      this.scene.context.drawImage(image, this.x, this.y)
    }
    Actor.prototype.width = function () {
      return this.scene.images[this.type].width
    }
    Actor.prototype.height = function () {
      return this.scene.images[this.type].height
    }

    function SpaceShip(scene, x, y) {
      Actor.call(this, scene, x, y)
      this.points = 0
    }

  }
}
</script>

<style scoped>

</style>
