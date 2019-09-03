<template>
  <div>
    <div class="content">
      <div class="preview">
        <CollapsibleSection>
          <div class="preview-content">
            <div class="top-row">
              <img :src="selectedRobot.head.src" />
            </div>
            <div class="middle-row">
              <img :src="selectedRobot.leftArm.src" class="rotate-left" />
              <img :src="selectedRobot.torso.src" />
              <img :src="selectedRobot.rightArm.src" class="rotate-right" />
            </div>
            <div class="bottom-row">
              <img :src="selectedRobot.base.src" />
            </div>
          </div>
        </CollapsibleSection>
        <button class="add-to-cart" @click="addToCart()">Add to cart</button>
      </div>
    </div>

    <div class="top-row">
      <div class="robot-name">{{selectedRobot.head.title}}</div>
      <PartSelector
        :parts="availableParts.heads"
        position="top"
        @partSelected="part => selectedRobot.head=part"
      />
    </div>
    <div class="middle-row">
      <PartSelector
        :parts="availableParts.arms"
        position="left"
        @partSelected="part => selectedRobot.leftArm=part"
      />
      <PartSelector
        :parts="availableParts.torsos"
        position="center"
        @partSelected="part => selectedRobot.torso=part"
      />
      <PartSelector
        :parts="availableParts.arms"
        position="right"
        @partSelected="part => selectedRobot.rightArm=part"
      />
    </div>
    <div class="bottom-row">
      <PartSelector
        :parts="availableParts.bases"
        position="bottom"
        @partSelected="part => selectedRobot.base=part"
      />
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
import CollapsibleSection from '../shared/CollapsibleSection.vue';

export default {
  name: 'RobotBuilder',
  beforeRouteLeave(to, from, next) {
    if (this.asddedToCart) {
      next(true);
    } else {
      /* eslint no-restricted-globals: 0 */
      // eslint-disable-next-line no-alert
      const response = confirm(
        'You have not added your robot to the cart, are you sure you want to leave?',
      );
      next(response);
    }
  },
  components: { PartSelector, CollapsibleSection },
  data() {
    return {
      addedToCart: false,
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
      this.addedToCart = true;
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
  width: 210px;
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

.preview {
  position: absolute;
  top: -20px;
  right: 0;
  width: 210px;
  height: 210px;
  padding: 5px;
}
.preview-content {
  border: 1px solid #999;
}
.preview img {
  width: 50px;
  height: 50px;
}
.rotate-right {
  transform: rotate(90deg);
}
.rotate-left {
  transform: rotate(-90deg);
}
</style>
