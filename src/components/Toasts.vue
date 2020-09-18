<template>
  <div style="position: fixed; top: 20px; right: 20px; min-width: 300px; z-index: 11000;">
    <div
      v-for="(item, i) in messages"
      :id="`toast-${i}`"
      :key="i"
      class="toast fade show"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
      data-autohide="false"
    >
      <div class="toast-header">
        <div
          class="inline-block rounded mr-2"
          style="width: 20px; height: 20px"
          :class="`bg-${ item.status }`"
        />
        <strong class="mr-auto">六角實戰班</strong>
        <small>現在</small>
        <button
          type="button"
          class="ml-2 mb-1 close"
          data-dismiss="toast"
          aria-label="Close"
          @click="closeToast(`toast-${i}`)"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="toast-body">
        {{ item.message }}
      </div>
    </div>
  </div>
</template>

<script>
/* global $ */

export default { // 匯出toast這個元件
  name: 'Toasts',
  data() {
    return {
      messages: [],
    };
  },
  created() {
    const vm = this;
    vm.$bus.$on('message:push', (message, status = 'warning') => { // $bus.$on監聽方法
      vm.updateMessage(message, status); //  近來會觸發這個方法
    });
  },
  methods: {
    updateMessage(message, status) { // 觸發之後會將資料push進去
      const timestamp = Math.floor(new Date() / 1000); // 時間計算
      this.messages.push({
        message, // 訊息
        status, // 資料狀態 danger or seusee ....等等
        timestamp, // 時間參數
      });
      this.removeMessageWithTiming(timestamp); // 五秒後移除該toast
    },
    removeMessage(num) {
      this.messages.splice(num, 1);
    },
    removeMessageWithTiming(timestamp) { // 傳參數進來
      const vm = this;
      setTimeout(() => {
        vm.messages.forEach((item, i) => {
          if (item.timestamp === timestamp) {
            vm.messages.splice(i, 1); // 透過timestamp刪除該訊息
          }
        });
      }, 5000); // 五秒後
    },
    closeToast(element) { // 手動隱藏toast方法
      $(`#${element}`).toast('hide');
    },
  },
};
</script>

<style scope>
.message-alert {
  position: fixed;
  max-width: 50%;
  top: 56px;
  right: 20px;
  z-index: 1100;
}
</style>
