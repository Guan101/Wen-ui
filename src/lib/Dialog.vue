<template>
<template v-if="visible">
  <Teleport to="body">
    <div class="Wen-dialog-overlay" @click="onClickOverlay"></div>
    <div class="Wen-dialog-wrapper">
      <div class="Wen-dialog">
        <header>
          <slot name="title" />
          <span @click="close" class="Wen-dialog-close"></span>
        </header>
        <main>
          <slot name="content" />
        </main>
        <footer>
          <Button level="main" @click="onClickOk">OK</Button>
          <Button @click="onClickCancel">Cancel</Button>
        </footer>
      </div>
    </div>
  </Teleport>
</template>
</template>

<script lang="ts" setup="props, context">
import { SetupContext } from 'vue';
import Button from "./Button.vue";
declare const props: {
  visible: boolean;
  closeOnClickOverlay: boolean; 
  ok: () => boolean; 
  cancel: () => void
}
declare const context: SetupContext
export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    closeOnClickOverlay: {
      type: Boolean,
      default: true
    },
    ok: {
      type: Function
    },
    cancel: {
      type: Function
    }
  },
  components: {
    Button,
  },
};
export const close = () => {
  context.emit('update:visible', false)
}
export const onClickOverlay = () => {
  if (props.closeOnClickOverlay) {
    close()
  }
}
export const onClickOk = () => {
  if (props.ok && props.ok() !== false) {
    close()
  }
}
export const onClickCancel = () => {
  props.cancel && props.cancel()
  close()
}
</script>

<style lang="scss">
$radius: 4px;
$border-color: #d9d9d9;

.Wen-dialog {
  background: white;
  border-radius: $radius;
  box-shadow: 0 0 3px fade_out(black, 0.5);
  min-width: 15em;
  max-width: 90%;

  &-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: fade_out(black, 0.5);
    z-index: 10;
  }

  &-wrapper {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 11;
  }

  >header {
    padding: 12px 16px;
    border-bottom: 1px solid $border-color;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 20px;
  }

  >main {
    padding: 12px 16px;
  }

  >footer {
    border-top: 1px solid $border-color;
    padding: 12px 16px;
    text-align: right;
  }

  &-close {
    position: relative;
    display: inline-block;
    width: 16px;
    height: 16px;
    cursor: pointer;

    &::before,
    &::after {
      content: '';
      position: absolute;
      height: 1px;
      background: black;
      width: 100%;
      top: 50%;
      left: 50%;
    }

    &::before {
      transform: translate(-50%, -50%) rotate(-45deg);
    }

    &::after {
      transform: translate(-50%, -50%) rotate(45deg);
    }

  }
}
</style>
