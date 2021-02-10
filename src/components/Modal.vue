<template>
  <div class="modal__mask">
    <div class="modal__wrapper">
      <div class="modal__container">
        <div class="modal__header">
          <slot name="header"> Example Header </slot>
        </div>

        <div class="modal__body">
          <slot name="body"> Example Body </slot>
        </div>

        <div class="modal__footer">
          <slot name="footer">
            Example Footer
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="css">
@import "../assets/styles/modal.css";
</style>
