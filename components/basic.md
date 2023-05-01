# Basic Components

Some of the more basic ways to use generic components

## BasicTitle

Used to display headers, can display help buttons and text

### Usage

```vue
<template>
  <div>
    <BasicTitle helpMessage="Tip 1">Title</BasicTitle>
    <BasicTitle :helpMessage="['Tip 1', 'Tip 2']">Title</BasicTitle>
  </div>
</template>
<script>
  import { BasicTitle } from '/@/components/Basic/index';
  import { defineComponent } from 'vue';
  export default defineComponent({
    components: { BasicTitle },
  });
</script>
```

### Props


| Properties | Type | Default | Description |
| ----------- | ------------------ | ------- | ------------------------ |
| helpMessage | `string｜string[]` | - | Help button message to the right of the header span
| span | `boolean` | `false` | Whether or not to show the blue block to the left of the title |
| normal | `boolean` | `false` | Defaults the text to unbolded |

### Slots

| Name | Description |
| ------- | -------- |
| default | title text |

## BasicArrow

Arrowhead component with animated drawing

### Usage

```vue
<template>
  <div>
    <BasicArrow :expand="false" />
  </div>
</template>
<script>
  import { BasicArrow } from '/@/components/Basic/index';
  import { defineComponent } from 'vue';
  export default defineComponent({
    components: { BasicArrow },
  });
</script>
```

### Props


| Properties | Type | Default | Description |
| ------ | --------- | ------- | ----------------------------- |
| expand | `boolean` | `false` | arrow expand state |
| top | `boolean` | `false` | default arrow up |
| bottom | `boolean` | `false` | default down
| inset | `boolean` | `false` | Cancel padding/margin for inset |

## BasicHelp

Help button component

### Usage

```vue
<template>
  <div>
    <BasicHelp :text="['Tip1', 'Tip2']" />
    <BasicHelp text="hints" />
  </div>
</template>
<script>
  import { BasicHelp } from '/@/components/Basic/index';
  import { defineComponent } from 'vue';
  export default defineComponent({
    components: { BasicHelp },
  });
</script>
```

### Props

| Properties | Type | Default | Optional | Description |
| --------- | ------------------ | ------- | ------ | ------------------------------------------ |
| fontSize | `string` | `14px` | - | fontSize |
| color | `string` | #fff | - | color |
| text | `string｜string[]` | - | - | Text list |
| showIndex | `boolean`| true | - | Whether to show the number, if text is string[] |
| maxWidth | `string` | `600px` | - | maxWidth |
| placement | `string` | `right` | - | Display orientation, see Tooltip component |
| -      | 显示方向，参考 Tooltip 组件                |

### Slots

| Name | Description |
| ------- | -------- |
| default | default icon |
