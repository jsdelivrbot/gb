// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import VueRouter from 'vue-router'
import Mint from 'mint-ui';
Vue.config.productionTip = false;
Vue.use(VueRouter);
Vue.use(Mint);
import page from './components/page/page.vue';
import detalis from './components/new/detalis.vue';
import meDe from './components/me/meDe.vue';
import about from './components/me/about.vue';
import collect from './components/me/collect.vue';
import count from './components/me/count.vue';
import feed from './components/me/feed.vue';
import name from './components/me/name.vue';
import pass from './components/me/pass.vue';
import phone from './components/me/phone.vue';
import phone2 from './components/me/phone2.vue';
import login from './components/login/login.vue';
import retrieve from './components/login/retrieve.vue';
import register from './components/login/register.vue';
import rob from './components/find/rob.vue';
import sell from './components/find/sellTime.vue';
import shoeDe from './components/find/shoeDe.vue';
import binding from './components/find/binding.vue';
import ready from './components/find/ready.vue';

const routes = [
  {path: '/', component: page},
  {path: '/detalis/:newid/:newuid/:newtitle/:newurl/:newtime/:newname/:newdesc/:num/:readys/:reference', component: detalis},
  {path: '/meDe', component: meDe,},
  {path: '/about', component: about},
  {path: '/collect', component: collect},
  {path: '/count', component: count},
  {path: '/feed', component: feed},
  {path: '/name/:ni', component: name},
  {path: '/pass', component: pass},
  {path: '/phone', component: phone},
  {path: '/phone2', component: phone2},
  {path: '/login', component: login},
  {path: '/retrieve/:id', component: retrieve},
  {path: '/register/:tel/:code/:pass', component: register},
  {path: '/rob', component: rob},
  {path: '/sell', component: sell},
  {path: '/shoeDe/:id', component: shoeDe},
  {path: '/binding', component: binding},
  {path: '/ready', component: ready}
];

const router = new VueRouter({
  routes
});

const app = new Vue({
  router,
  render: h => h(App)
}).$mount('#app');
