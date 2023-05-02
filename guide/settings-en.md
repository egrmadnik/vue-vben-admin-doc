# Project configuration items

Used to modify the project's color scheme, layout, cache, multi-language, component default configuration

## Environment variables configuration

The project's environment variables are configured in the project root directory in [.env](https://github.com/vbenjs/vue-vben-admin/blob/main/.env), [.env.development](https://github.com/vbenjs/vue vben-admin/blob/main/.env.development), [.env.production](https://github.com/vbenjs/vue-vben-admin/blob/main/.env.production)

For details, see [Vite Documentation](https://github.com/vitejs/vite#modes-and-environment-variables)

```bash
.env # Loaded in all environments
.env.local # Loaded in all environments, but will be ignored by git
.env.[mode] # only be loaded in the specified mode
.env.[mode].local # Loaded only in the specified mode, but ignored by git

```
::: tip A gentle reminder

- Only variables starting with `VITE_ ` will be embedded in the client-side package, and you can access them in the project code like this:

```js
console.log(import.meta.env.VITE_PROT).
```

- The variables starting with ``VITE_GLOB_*` are added to the [\_app.config.js](#production environment dynamic configuration) configuration file during packaging.

::.

### Configuration item description

### .env

For all environments

```bash
# Whether to turn on mock data or not, when off, you need to dock the backend interface by yourself
VITE_USE_MOCK=true
# Resource public path, need to start and end with /
VITE_PUBLIC_PATH=/
# Whether to delete the Console.log
VITE_DROP_CONSOLE=false
# local development proxy, can solve cross-domain and multi-address proxy
# If the interface address is matched, it will be forwarded to http://localhost:3000 to prevent local cross-domain problems
# can have more than one, note that multiple can not line feed, otherwise the proxy will fail
VITE_PROXY=[["/api", "http://localhost:3000"],["api1", "http://localhost:3001"],["/upload", "http://localhost:3001/upload"]]
# Interface address
# If there are no cross-domain issues, just configure it here directly
VITE_GLOB_API_URL=/api
# File upload interface Optional
VITE_GLOB_UPLOAD_URL=/upload
# interface address prefix, some systems have prefixes for all interface addresses, you can add them here, easy to switch
VITE_GLOB_API_URL_PREFIX=
```

::: warning Note

The `VITE_PROXY` and `VITE_GLOB_API_URL`, /api should be unique and not conflict with the name of the interface.

If your interface is something like `http://localhost:3000/api`, please consider replacing `VITE_GLOB_API_URL=/xxxx` with another name

::.

### .env.production

For production environments

```bash
### Enable or disable mock
VITE_USE_MOCK=true
# Interface address can be forwarded by nginx or written directly to the actual address
VITE_GLOB_API_URL=/api
# The file upload address can be forwarded by nginx or written directly to the actual address
VITE_GLOB_UPLOAD_URL=/upload
# interface address prefix, some systems have prefixes for all interface addresses, you can add them here to make it easy to switch
VITE_GLOB_API_URL_PREFIX=
# Whether to delete the Console.log
VITE_DROP_CONSOLE=true
# Resource public path, needs to start and end with /
VITE_PUBLIC_PATH=/
# Whether to output gz|br files as packaged
# Optional: gzip | brotli | none
# There can be more than one, e.g. 'gzip'|'brotli', this will generate both .gz and .br files
VITE_BUILD_COMPRESS = 'gzip'
# Pack or not compress images
VITE_USE_IMAGEMIN = false
# Whether to package with pwa enabled or not
VITE_USE_PWA = false
# If or not it is compatible with older browsers. If this is enabled, the packaging time will be twice as slow. It will be more compatible with older browsers, and will automatically use the corresponding version according to browser compatibility
VITE_LEGACY = false
VITE_LEGACY = false

## Dynamic configuration of production environment

### Description

When `yarn build` is executed, the `_app.config.js` file is automatically generated and inserted into `index.html` after the project is built.

**Note: The development environment does not generate **

```js
// _app.config.js
// Variable name naming rules __PRODUCTION__xxx_CONF__ xxx: VITE_GLOB_APP_SHORT_NAME for .env configuration
window.__PRODUCTION__VUE_VBEN_ADMIN__CONF__ = {
  VITE_GLOB_APP_TITLE: 'vben admin'.
  VITE_GLOB_APP_SHORT_NAME: 'vue_vben_admin'.
  VITE_GLOB_API_URL: '/app'.
  VITE_GLOB_API_URL_PREFIX: '/'.
  VITE_GLOB_UPLOAD_URL: '/upload'.
}.
```

### Role

`_app.config.js` is used for projects that need to dynamically modify the configuration requirements after packaging, such as the interface address. Instead of repackaging, you can modify the variables in `/dist/_app.config.js` after packaging and refresh to update the local variables in the code.

### How to get global variables

To get the variables in `_app.config.js`, you can use the function [src/hooks/setting/index.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/src/hooks/setting/ index.ts) function to get them

### How to add (add a dynamically modifiable configuration item)

1. First, in `.env` or the corresponding development environment configuration file, add a variable that needs to be dynamically configurable, starting with `VITE_GLOB_`. 2.

2. The variables starting with `VITE_GLOB_` will be automatically added to the environment variables, and the newly added types will be defined by modifying the values of the `GlobEnvConfig` and `GlobConfig` environment variables in `src/types/config.d.ts`.

3. Add the new return value to the [useGlobSetting](https://github.com/vbenjs/vue-vben-admin/tree/main/src/hooks/setting/index.ts) function

```js
const {
  VITE_GLOB_APP_TITLE,
  VITE_GLOB_API_URL,
  VITE_GLOB_APP_SHORT_NAME,
  VITE_GLOB_API_URL_PREFIX,
  VITE_GLOB_UPLOAD_URL,
} = ENV;

export const useGlobSetting = (): SettingWrap => {
  // Take global configuration
  const glob: Readonly<GlobConfig> = {
    title: VITE_GLOB_APP_TITLE,
    apiUrl: VITE_GLOB_API_URL,
    shortName: VITE_GLOB_APP_SHORT_NAME,
    urlPrefix: VITE_GLOB_API_URL_PREFIX,
    uploadUrl: VITE_GLOB_UPLOAD_URL
  };
  return glob as Readonly<GlobConfig>;
};

```

## 项目配置

::: warning

项目配置文件用于配置项目内展示的内容、布局、文本等效果，存于`localStorage`中。如果更改了项目配置，需要手动**清空** `localStorage` 缓存，刷新重新登录后方可生效。

:::

### 配置文件路径

[src/settings/projectSetting.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/src/settings/projectSetting.ts)

### 说明

```js
// ! 改动后需要清空浏览器缓存
const setting: ProjectConfig = {
  // 是否显示SettingButton
  showSettingButton: true,

  // 是否显示主题切换按钮
  showDarkModeToggle: true,

  // 设置按钮位置 可选项
  // SettingButtonPositionEnum.AUTO: 自动选择
  // SettingButtonPositionEnum.HEADER: 位于头部
  // SettingButtonPositionEnum.FIXED: 固定在右侧
  settingButtonPosition: SettingButtonPositionEnum.AUTO,

  // 权限模式,默认前端角色权限模式
  // ROUTE_MAPPING: 前端模式（菜单由路由生成，默认）
  // ROLE：前端模式（菜单路由分开）
  permissionMode: PermissionModeEnum.ROUTE_MAPPING,
  // 权限缓存存放位置。默认存放于localStorage
  permissionCacheType: CacheTypeEnum.LOCAL,
  // 会话超时处理方案
  // SessionTimeoutProcessingEnum.ROUTE_JUMP: 路由跳转到登录页
  // SessionTimeoutProcessingEnum.PAGE_COVERAGE: 生成登录弹窗，覆盖当前页面
  sessionTimeoutProcessing: SessionTimeoutProcessingEnum.ROUTE_JUMP,
  // 项目主题色
  themeColor: primaryColor,
  // 网站灰色模式，用于可能悼念的日期开启
  grayMode: false,
  // 色弱模式
  colorWeak: false,
  // 是否取消菜单,顶部,多标签页显示, 用于可能内嵌在别的系统内
  fullContent: false,
  // 主题内容宽度
  contentMode: ContentEnum.FULL,
  // 是否显示logo
  showLogo: true,
  // 是否显示底部信息 copyright
  showFooter: true,
  // 头部配置
  headerSetting: {
    // 背景色
    bgColor: '#ffffff',
    // 固定头部
    fixed: true,
    // 是否显示顶部
    show: true,
    // 主题
    theme: MenuThemeEnum.LIGHT,
    // 开启锁屏功能
    useLockPage: true,
    // 显示全屏按钮
    showFullScreen: true,
    // 显示文档按钮
    showDoc: true,
    // 显示消息中心按钮
    showNotice: true,
    // 显示菜单搜索按钮
    showSearch: true,
  },
  // 菜单配置
  menuSetting: {
    // 背景色
    bgColor: '#273352',
    // 是否固定住菜单
    fixed: true,
    // 菜单折叠
    collapsed: false,
    // 折叠菜单时候是否显示菜单名
    collapsedShowTitle: false,
    // 是否可拖拽
    canDrag: true,
    // 是否显示
    show: true,
    // 菜单宽度
    menuWidth: 180,
    // 菜单模式
    mode: MenuModeEnum.INLINE,
    // 菜单类型
    type: MenuTypeEnum.SIDEBAR,
    // 菜单主题
    theme: MenuThemeEnum.DARK,
    // 分割菜单
    split: false,
    // 顶部菜单布局
    topMenuAlign: 'start',
    // 折叠触发器的位置
    trigger: TriggerEnum.HEADER,
    // 手风琴模式，只展示一个菜单
    accordion: true,
    // 在路由切换的时候关闭左侧混合菜单展开菜单
    closeMixSidebarOnChange: false,
    // 左侧混合菜单模块切换触发方式
    mixSideTrigger: MixSidebarTriggerEnum.CLICK,
    // 是否固定左侧混合菜单
    mixSideFixed: false,
  },
  // 多标签
  multiTabsSetting: {
    // 刷新后是否保留已经打开的标签页
    cache: false,
    // 开启
    show: true,
    // 开启快速操作
    showQuick: true,
    // 是否可以拖拽
    canDrag: true,
    // 是否显示刷新按钮
    showRedo: true,
    // 是否显示折叠按钮
    showFold: true,
  },

  // 动画配置
  transitionSetting: {
    //  是否开启切换动画
    enable: true,
    // 动画名
    basicTransition: RouterTransitionEnum.FADE_SIDE,
    // 是否打开页面切换loading
    openPageLoading: true,
    // 是否打开页面切换顶部进度条
    openNProgress: false,
  },

  // 是否开启KeepAlive缓存  开发时候最好关闭,不然每次都需要清除缓存
  openKeepAlive: true,
  // 自动锁屏时间，为0不锁屏。 单位分钟 默认1个小时
  lockTime: 0,
  // 显示面包屑
  showBreadCrumb: true,
  // 显示面包屑图标
  showBreadCrumbIcon: false,
  // 是否使用全局错误捕获
  useErrorHandle: false,
  // 是否开启回到顶部
  useOpenBackTop: true,
  //  是否可以嵌入iframe页面
  canEmbedIFramePage: true,
  // 切换界面的时候是否删除未关闭的message及notify
  closeMessageOnSwitch: true,
  // 切换界面的时候是否取消已经发送但是未响应的http请求。
  // 如果开启,想对单独接口覆盖。可以在单独接口设置
  removeAllHttpPending: true,
};
```

## 缓存配置

用于配置缓存内容加密信息，对缓存到浏览器的信息进行 AES 加密

在 [/@/settings/encryptionSetting.ts](https://github.com/vbenjs/vue-vben-admin/blob/main/src/settings/encryptionSetting.ts) 内可以配置 `localStorage` 及 `sessionStorage` 缓存信息

**前提:** 使用项目自带的缓存工具类 [/@/utils/cache](https://github.com/vbenjs/vue-vben-admin/blob/main/src/utils/cache/index.ts) 来进行缓存操作

```ts
import { isDevMode } from '/@/utils/env';

// 缓存默认过期时间
export const DEFAULT_CACHE_TIME = 60 * 60 * 24 * 7;

// 开启缓存加密后，加密密钥。采用aes加密
export const cacheCipher = {
  key: '_11111000001111@',
  iv: '@11111000001111_',
};

// 是否加密缓存，默认生产环境加密
export const enableStorageEncryption = !isDevMode();
```

## 多语言配置

用于配置多语言信息

在 [src/settings/localeSetting.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/src/settings/localeSetting.ts) 内配置

```ts
export const LOCALE: { [key: string]: LocaleType } = {
  ZH_CN: 'zh_CN',
  EN_US: 'en',
};

export const localeSetting: LocaleSetting = {
  // 是否显示语言选择器
  showPicker: true,
  // 当前语言
  locale: LOCALE.ZH_CN,
  // 默认语言
  fallback: LOCALE.ZH_CN,
  // 允许的语言
  availableLocales: [LOCALE.ZH_CN, LOCALE.EN_US],
};

// 语言列表
export const localeList: DropMenu[] = [
  {
    text: '简体中文',
    event: LOCALE.ZH_CN,
  },
  {
    text: 'English',
    event: LOCALE.EN_US,
  },
];
```

## 主题色配置

默认全局主题色配置位于 [build/config/glob/themeConfig.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/build/config/themeConfig.ts) 内

只需要修改 primaryColor 为您需要的配色，然后重新执行 `yarn serve` 即可

```js
/**
 * less global variable
 */
export const primaryColor = '#0960bd';
```

## 样式配置

### css 前缀设置

用于修改项目内组件 class 的统一前缀

- 在 [src/settings/designSetting.ts](https://github.com/vbenjs/vue-vben-admin/blob/main/src/settings/designSetting.ts) 内配置

```ts
export const prefixCls = 'vben';
```

- 在 [src/design/var/index.less](https://github.com/vbenjs/vue-vben-admin/blob/main/src/design/var/index.less) 配置 css 前缀

```less
@namespace: vben;
```

### 前缀使用

**在 css 内**

```vue
<style lang="less" scoped>
  /* namespace已经全局注入，不需要额外再引入 */
  @prefix-cls: ~'@{namespace}-app-logo';

  .@{prefix-cls} {
    width: 100%;
  }
</style>
```

**在 vue/ts 内**

```ts
import { useDesign } from '/@/hooks/web/useDesign';

const { prefixCls } = useDesign('app-logo');

// prefixCls => vben-app-logo
```

## 颜色配置

用于预设一些颜色数组

在 [src/settings/designSetting.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/src/settings/designSetting.ts) 内配置

```ts
//  app主题色预设
export const APP_PRESET_COLOR_LIST: string[] = [
  '#0960bd',
  '#0084f4',
  '#009688',
  '#536dfe',
  '#ff5c93',
  '#ee4f12',
  '#0096c7',
  '#9c27b0',
  '#ff9800',
];

// 顶部背景色预设
export const HEADER_PRESET_BG_COLOR_LIST: string[] = [
  '#ffffff',
  '#009688',
  '#5172DC',
  '#1E9FFF',
  '#018ffb',
  '#409eff',
  '#4e73df',
  '#e74c3c',
  '#24292e',
  '#394664',
  '#001529',
  '#383f45',
];

// 左侧菜单背景色预设
export const SIDE_BAR_BG_COLOR_LIST: string[] = [
  '#001529',
  '#273352',
  '#ffffff',
  '#191b24',
  '#191a23',
  '#304156',
  '#001628',
  '#28333E',
  '#344058',
  '#383f45',
];
```
# Component default parameter configuration

Configured within [src/settings/componentSetting.ts](https://github.com/vbenjs/vue-vben-admin/tree/main/src/settings/componentSetting.ts)

```ts
// Used to configure the general configuration of certain components without modifying them
import type { SorterResult } from '... /components/Table'.

export default {
  // Table configuration
  table: {
    // Generic configuration for table interface requests, can be overridden in component prop
    // Supports xxx.xxx.xxx format
    fetchSetting: {
      // Current page field passed to the backend
      pageField: 'page'.
      // field passed to the backend for how many items to display per page
      sizeField: 'pageSize'.
      // The field of the table data returned by the interface
      listField: 'items'.
      // field that returns the total number of items in the table
      totalField: 'total'.
    }.
    // Optional paging options
    pageSizeOptions: ['10', '50', '80', '100'].
    // default number of items to display per page
    defaultPageSize: 10.
    // default sorting method
    defaultSortFn: (sortInfo: SorterResult) => {
      const { field, order } = sortInfo.
      return {
        // Sort fields
        field.
        // sort by asc/desc
        order.
      }.
    }.
    // Custom filter method
    defaultFilterFn: (data: Partial<Recordable<string[]>>) => {
      return data.
    }.
  }.
  // Scrolling component configuration
  scrollbar: {
    // Whether to use native scrolling style
    // When enabled, the menu, popup, and drawer will use the native scrollbar component
    native: false.
  }.
}.
```
