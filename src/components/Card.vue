<template>
  <div class="flex flex-col md:flex-row roudned-md gap-4 border">
    <slot name="header" />
    <div class="p-4 space-y-2">
      <slot name="content" />
    </div>
  </div>
</template>
