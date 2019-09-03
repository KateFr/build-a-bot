<template>
  <div>
    <div class="content">
      <button class="add-to-cart" @click="addToCart()">Add to cart</button>
    </div>

    <div class="top-row">
      <div class="robot-name">{{selectedRobot.head.title}}</div>
      <PartSelector :parts="availableParts.heads" position="top" />
    </div>
    <div class="middle-row">
      <PartSelector :parts="availableParts.arms" position="left" />
      <PartSelector :parts="availableParts.torsos" position="center" />
      <PartSelector :parts="availableParts.arms" position="right" />
    </div>
    <div class="bottom-row">
      <PartSelector :parts="availableParts.bases" position="bottom" />
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
          <tr>
            <th>Robot</th>
            <th class="cost">Cost</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(robot,index) in cart" :key="index">
            <td>{{robot.head.title}}</td>
            <td>{{robot.cost}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import availableParts from '../data/parts';
import PartSelector from './PartSelector.vue';

export default {
  name: 'RobotBuilder',
  components: { PartSelector },
  data() {
    return {
      cart: [],
      availableParts,
      selectedRobot: {
        head: {},
        torso: {},
        leftArm: {},
        rightArm: {},
        base: {},
      },
    };
  },
  computed: {
    saleBorderClass() {
      return this.selectedRobot.head.onSale ? 'sale-border' : '';
    },
  },
  methods: {
    addToCart() {
      const robot = this.selectedRobot;
      const cost =        robot.head.cost
        + robot.torso.cost
        + robot.leftArm.cost
        + robot.rightArm.cost
        + robot.base.cost;
      this.cart.push(Object.assign({}, robot, { cost }));
      console.log(this.cart);
    },
  },
};
</script>
<style lang="scss" scoped>
.top-row {
  display: flex;
  justify-content: space-around;
}
.middle-row {
  display: flex;
  justify-content: center;
}
.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}
.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}

.sale {
  color: red;
}

.content {
  position: relative;
}

.add-to-cart {
  position: absolute;
  font-size: 16px;
  right: 30px;
  width: 220px;
  padding: 3px;
}

td,
th {
  text-align: left;
  padding: 5px;
}

.cost {
  text-align: right;
}

.sale-border {
  border: 3px solid red;
}
</style>
