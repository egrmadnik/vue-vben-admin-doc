# Authority

Role information or permission code Permission mode is automatically distinguished

## Usage

```vue
<template>
  <div>
    <Authority :value="RoleEnum.ADMIN">
      <a-button type="primary" block> 只有admin角色可见 </a-button>
    </Authority>
  </div>
</template>
<script>
  import { Authority } from '/@/components/Authority';
  import { defineComponent } from 'vue';
  export default defineComponent({
    components: { Authority },
  });
</script>
```

## Props
| Properties | Type | Default | Description |
| --- | --- | --- | --- |
| value | `RoleEnum,RoleEnum[],string,string[]` | - | Role information or permission code Permission mode is automatically distinguished |
