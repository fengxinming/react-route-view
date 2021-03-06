# react-route-view

[![npm package](https://nodei.co/npm/react-route-view.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/react-route-view)

> A Higher-Order Route component with guard hooks.

[![NPM version](https://img.shields.io/npm/v/react-route-view.svg?style=flat)](https://npmjs.org/package/react-route-view)
[![NPM Downloads](https://img.shields.io/npm/dm/react-route-view.svg?style=flat)](https://npmjs.org/package/react-route-view)

[Online Demo](https://fengxinming.github.io/react-route-view/)

## react-route-view 是什么

`react-route-view` 是具有守卫钩子的路由高阶组件。

## 路由初始化流程

![image](https://user-images.githubusercontent.com/6262382/126643754-f4149852-083e-4230-ad38-15abfae15c8a.png)

## 路由切换流程

![image](https://user-images.githubusercontent.com/6262382/126643755-c36bab0b-9843-4c27-9be1-b6aa4d10ccde.png)

## 内部流程

![image](https://user-images.githubusercontent.com/6262382/126643777-21634657-cc7d-4c02-b129-961c7e4ef092.png)

## 导航流程

```txt
┌───────────────────────┐
│     push / replace    │
└──────────┬────────────┘
┌──────────┴────────────┐
│     匹配路由           │
└──────────┬────────────┘
┌──────────┴────────────┐
│     beforeEach        │
└──────────┬────────────┘
┌──────────┴────────────┐
│     beforeEnter       │
└──────────┬────────────┘
┌──────────┴────────────┐
│     渲染已匹配的组件    │
└──────────┬────────────┘
┌──────────┴────────────┐
│     afterEach         │
└──────────┬────────────┘
┌──────────┴────────────┐
│     afterEnter        │
└───────────────────────┘
```

## 安装

```bash
tnpm i react react-router@5 history@4 react-route-view --save
```

## API

### create

配置参数定义：

```ts
import {Location, History, UnregisterCallback} from 'history';
import {match, RouteComponentProps} from 'react-router';

export interface IRouteInfo {
  name: string;
  location: Location;
  history: History;
  match: match;
  query: object;
  meta?: any;
}

export type Next = (done?: boolean) => void;
export type NextRedirect = (location?: string | Location) => void;
export type NextError = (err?: Error) => void;

export type BeforeEnter = (to: IRouteInfo, from: IRouteInfo, next: Next | NextError | NextRedirect) => void;
export type AfterEnter = (to: IRouteInfo, from: IRouteInfo) => void;

export type ConnectionRouteComponent = (route: RouteComponentProps) => React.ReactComponentElement;
export type ConnectionFn = (
  component: React.FC | React.Component, 
  config: {
    beforeEnter: BeforeEnter | BeforeEnter[],
    afterEnter: AfterEnter | AfterEnter[]
  }
) => ConnectionRouteComponent;

export interface ICreatedResult {
  connect: ConnectionFn;
  unlisten: (history: History) => UnregisterCallback;
}

export interface IRouteViewProps {
  onError: ErrorHandler;
  propsFromRoute: RouteComponentProps,
  component: React.Component,
  beforeHooks: BeforeEnter[],
  afterHooks: AfterEnter[]
}

export type ErrorHandler = (err: Error, props: IRouteViewProps) => void;

export function create(config: {
  onError: ErrorHandler;
  beforeEach: BeforeEnter | BeforeEnter[],
  afterEach: AfterEnter | AfterEnter[]
}): ICreatedResult;
```

创建一个具有守卫钩子的路由高阶组件，包含以下实例属性：

#### connect

返回一个高阶组件在路由匹配后会进入守卫钩子函数

```ts
type ConnectionRouteComponent = (route: RouteComponentProps) => React.ReactComponentElement;

connect(
  component: React.FC | React.Component, 
  config: {
    beforeEnter: BeforeEnter | BeforeEnter[],
    afterEnter: AfterEnter | AfterEnter[]
  }
) => ConnectionRouteComponent;
```

#### unlisten

移除对 history 的监听

### RouteView

配置参数定义：

```ts
import {RouteComponentProps} from 'react-router';
export interface IRouteViewProps {
  onError: ErrorHandler;
  propsFromRoute: RouteComponentProps,
  component: React.Component,
  beforeHooks: BeforeEnter[],
  afterHooks: AfterEnter[]
}

export type ErrorHandler = (err: Error, props: IRouteViewProps) => void;

export const RouteView: React.Component<IRouteViewProps, any>;
```

## 使用

参见 [示例](https://github.com/fengxinming/react-route-view/tree/main/examples/demo)
