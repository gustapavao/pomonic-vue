<script setup>
import {inject, ref} from 'vue';
import Icon from '/public/icons/notification_icon.svg';
import Song from '/public/icons/notification.mp3';

const title = ref("Parabénsss");
const msg = ref("Vamos para mais um");

const emit = defineEmits(['time-over']);

function notifyMe() {
  emit('time-over')
  if (!("Notification" in window)) {
    alert("This browser does not support Desktop notifications");
  }
  if (Notification.permission === "granted") {
    callNotify();
    return;
  }
  if (Notification.permission !== "denied") {
    Notification.requestPermission((permission) => {
      if (permission === "granted") {
        callNotify();
      }
    });
  }
}

function callNotify() {
  new Notification(title.value, { body: msg.value, icon: Icon });
  new Audio(Song).play();
}

defineExpose({
  notifyMe
});
</script>
