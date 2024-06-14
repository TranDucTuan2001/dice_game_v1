<template>
  <div class="wrapper-popup" v-bind:class="openPopup">
    <div class="rule">
      <h3>Luật chơi</h3>

      <ul>
        <li>
          Random dư liệu 2 con xúc xắc . Kiểm tra xem người chơi có xoay đúng
          con số 1 không?
        </li>
        <li>Nếu xoay trúng số 1 -> Đổi lượt chơi -> Reset điểm tạm thời</li>
        <li>Nếu xoay ok, ấn HOLD để dữ điểm</li>
        <li>Game kết thúc khi có người vượt qua điểm đã nhập vào bạn đầu</li>
      </ul>
      <button class="confirm" v-on:click="confirm">Đã hiểu</button>
    </div>
  </div>
</template>
<script>
export default {
  name: "rule",
  data() {
    return {};
  },
  props: {
    isOpenPopup: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    confirm() {
      this.$emit("handleConfirm");
    },
  },
  computed: {
    openPopup() {
      return { "open-popup": this.isOpenPopup };
    },
  },
};
</script>
<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  background-color: rgba(0, 0, 0, 0.4);
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}
.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}
.rule {
  width: 350px;
  padding: 20px;
  font-family: Arial, Helvetica, sans-serif;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);
  background-color: #fff;
  position: absolute;
  transition: all 0.3s ease;
}
.rule h3 {
  margin-bottom: 10px;
}
.rule ul {
  padding: 15px;
}
.rule ul li {
 padding-bottom: 10px;
}
.rule .confirm {
  margin-top: 10px;
  cursor: pointer;
  border: 2px solid #333;
  padding: 8px 15px;
  background-color: #fff;
  transition: all 0.3s ease;
}
.rule .confirm:hover {
  background-color: #333;
  color: #fff;
}
</style>
