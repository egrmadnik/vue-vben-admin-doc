# Menu

The project menu configuration is stored in the [src/router/menus](https://github.com/vbenjs/vue-vben-admin/tree/main/src/router/menus) below

::: tip Tips

The menu must match the route to be displayed

:::

## Menu item type

```ts
export interface Menu {
  // Menu Name
  name: string;
  //menu icon, if not, it will try to use route.meta.icon
  icon?: string;
  // Menu path
  path: string;
  // Disable or not
  disabled?: boolean;
  // Submenu
  children?: Menu[];
  // Menu tab settings
  tag: {
    // True to show the dots
    dot: boolean;
    // Content
    content: string';
    //  Type
    type: 'error' | 'primary' | 'warn' | 'success';
  };
  //  
  hideMenu?: boolean;
}
```

## Menu Module

A menu file will be treated as a module

::: tip Tips

The path field of children does not need to start with `/`

:::

```ts
import type { MenuModule } from '/@/router/types';
import { t } from '/@/hooks/web/useI18n';
const menu: MenuModule = {
  orderNo: 10,
  menu: {
    name: t('routes.dashboard.dashboard'),
    path: '/dashboard',

    children: [
      {
        path: 'analysis',
        name: t('routes.dashboard.analysis'),
      },
      {
        path: 'workbench',
        name: t('routes.dashboard.workbench'),
      },
    ],
  },
};
export default menu;
```

The above modules will translate into the following structure

```ts
[
  path: '/dashboard',
  name: t('routes.dashboard.dashboard'),
  children: [
    {
      path: 'dashboard/analysis',
      name: t('routes.dashboard.analysis'),
    },
    {
      path: 'dashboard/workbench',
      name: t('routes.dashboard.workbench'),
    },
  ],
]
```

## New menu
Just add a new module file directly in [src/router/routes/modules](https://github.com/vbenjs/vue-vben-admin/tree/main/src/router/routes/modules).

There is no need to introduce it manually, files placed in [src/router/routes/modules](https://github.com/vbenjs/vue-vben-admin/tree/main/src/router/routes/modules) will be loaded automatically.


## Menu Sorting

Within the menu module, set the `orderNo` variable, the larger the value, the further down the order
