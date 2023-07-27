# vite搭建React项目

1. npm命令创建项目

   ```
   npm create vite@latest
   ```

2. vite.config.js的配置文件

   ```
   import { defineConfig } from 'vite'
   import react from '@vitejs/plugin-react'
   import path from 'path'
   const __dirname = path.resolve();
   
   // https://vitejs.dev/config/
   export default defineConfig({
     plugins: [react()],
     resolve: {
       alias: {
         '@': path.resolve(__dirname, 'src')
       }
     },
     server: {
       host: "localhost",
       port: 8080,
       open: true
     }
   })
   ```

3. 目录创建

   ```
   在src目录下面创建目录
   1.assets:该目录下存放图片、全局样式，自行创建文件夹
   2.components:存放公共组件
   3.router:用于配置路由文件
   4.store:仓库，Redux等等
   5.views:页面
   ```

4. package.json，less和less-loader需要安装在devDependencies中

   ```
     "dependencies": {
       "@ant-design/icons": "^4.6.2",
       "antd": "^4.24.12",
       "react": "^18.2.0",
       "react-dom": "^18.2.0",
       "react-redux": "^8.1.1",
       "react-router-dom": "^6.14.2",
       "reset-css": "^5.0.2"
     },
     "devDependencies": {
       "@types/node": "^20.4.5",
       "@types/react": "^18.2.15",
       "@types/react-dom": "^18.2.7",
       "@vitejs/plugin-react": "^4.0.3",
       "eslint": "^8.45.0",
       "eslint-plugin-react": "^7.32.2",
       "eslint-plugin-react-hooks": "^4.6.0",
       "eslint-plugin-react-refresh": "^0.4.3",
       "less": "^4.1.3",
       "less-loader": "^11.1.3",
       "vite": "^4.4.5"
     }
   ```

5. src目录下main.jsx

   ```
   import ReactDOM from 'react-dom/client'
   // 1.样式初始化
   import "reset-css";
   // 2.UI框架等样式
   // 3.全局样式
   import "@/assets/styles/global.less";
   // 4.组件样式
   import App from '@/views/App.jsx'
   import { BrowserRouter } from 'react-router-dom';
   
   ReactDOM.createRoot(document.getElementById('root')).render(
     <BrowserRouter>
       <App />
     </BrowserRouter>   
   )
   ```



6. 全局样式global.less

   ```
   * {
     margin: 0;
     padding: 0;
   }
   html, body {
     width: 100vw;
     height: 10vh;
     background-color: #eee;
     // 禁用文字选中
     user-select: none;
   }
   
   img {
     // 禁用图片拖拽
     -webkit-user-drag: none;
   }
   
   .logo {
     height: 32px;
     margin: 16px;
     background: rgba(255, 255, 255, 0.3);
   }
   
   .site-layout-background {
     background: #fff;
   }
   ```

7. 配置路由

   ```
   import Login from "@/views/Login";
   import { Navigate } from "react-router-dom";
   import React, { lazy } from "react";
   import Home from "@/views/Home";
   import NotFound from "@/views/404";
   const Page1 = lazy(() => import("@/views/Page/Page1"));
   const Page2 = lazy(() => import("@/views/Page/Page2"));
   const Page3 = lazy(() => import("@/views/Page/Page3"));
   const Page4 = lazy(() => import("@/views/Page/Page4"));
   // 懒加载模式的组件，外面需要套一层Loading的提示加载组件
   const withLoadingComp = (comp) => {
     return <React.Suspense fallback={<div>Loading</div>}>{comp}</React.Suspense>;
   };
   const routes = [
     {
       path: "/",
       element: <Navigate to="/login" />
     },
     {
       path: "/login",
       element: <Login />
     },
     {
       path: "/",
       element: <Home />,
       children: [
         {
           path: "/page1",
           element: withLoadingComp(<Page1 />)
         },
         {
           path: "/page2",
           element: withLoadingComp(<Page2 />)
         },
         {
           path: "/page3",
           element: withLoadingComp(<Page3 />)
         },
         {
           path: "/page4",
           element: withLoadingComp(<Page4 />)
         },
       ]
     },
     {
       path: '/404',
       element: <NotFound />
     },
     {
       path: "*",
       element: <Navigate to="/404" />
     }
   ];
   export default routes;
   ```

   

8. src/views目录下App.jsx

   ```
   import "antd/dist/antd.css";
   import { useRoutes } from "react-router-dom";
   import router from "@/router";
   const App = () => {
     const outlet = useRoutes(router);
     return (
       <div className="app">
         {outlet}
       </div>
     )
   };
   export default App
   ```

9. Home主页

   ```
   import {DesktopOutlined} from '@ant-design/icons';
   import { Breadcrumb, Layout, Menu } from 'antd';
   import { useState } from 'react';
   const { Header, Content, Footer, Sider } = Layout;
   import { Outlet } from 'react-router-dom';
   import { useNavigate, useLocation } from 'react-router-dom';
   const items = [
     {
       label: "Option1",
       key: "/page1",
       icon: <DesktopOutlined />,
       children: [
         {
           label: "Page1",
           key: "/page1",
         },
         {
           label: "Page2",
           key: "/page2"
         }
       ]
     },
     {
       label: "Option2",
       key: "/page3",
       icon: <DesktopOutlined />,
       children: [
         {
           label: "Page3",
           key: "/page3",
         },
         {
           label: "Page3",
           key: "/page4"
         }
       ]
     },
   ];
   const App = () => {
     const [collapsed, setCollapsed] = useState(false);
     const navigateTo = useNavigate();
     const currentRoute = useLocation();
     const menuClick = (e) => {
       // 点击跳转到对应的路由
       navigateTo(e.key);
     };
     let firstOpenkey = "";
     function findKey(obj) {
       return obj.key === currentRoute.pathname
     }
     for (let i = 0; i < items.length; i++) {
       if (items[i]['children'] && items[i]['children'].length > 0 && items[i]['children'].find(findKey)) {
         firstOpenkey = items[i].key;
         break;
       }
     }
     const [openKeys, setopenKeys] = useState([firstOpenkey]);
     const handleOpenChange = (keys) => {
       setopenKeys([keys[keys.length - 1]])
       console.log(111);
       console.log(keys);
     }
     return (
       <Layout
         style={{
           minHeight: '100vh',
         }}
       >
         <Sider collapsible collapsed={collapsed} onCollapse={(value) => setCollapsed(value)}>
           <div className="logo" />
           <Menu
             theme="dark"
             defaultSelectedKeys={[currentRoute.pathname]}
             mode="inline"
             items={items}
             onClick={menuClick}
             // 某项菜单展开和回收的事件
             onOpenChange={handleOpenChange}
             // 当前菜单展开项的数组
             openKeys={openKeys}
           />
         </Sider>
         <Layout className="site-layout">
           <Header
             className="site-layout-background"
             style={{
               padding: 0,
             }}
           >
             <Breadcrumb
               style={{
                 margin: '16px 0',
               }}
             >
               <Breadcrumb.Item>User</Breadcrumb.Item>
               <Breadcrumb.Item>Bill</Breadcrumb.Item>
             </Breadcrumb>
           </Header>
           <Content
             style={{
               margin: '16px 16px 0',
             }}
           >
             <Outlet />
           </Content>
           <Footer
             style={{
               textAlign: 'center',
             }}
           >
             Ant Design ©2018 Created by Ant UED
           </Footer>
         </Layout>
       </Layout>
     );
   };
   export default App;
   ```

   