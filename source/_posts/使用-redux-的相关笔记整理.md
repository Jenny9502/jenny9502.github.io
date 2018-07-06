---
title: 使用 redux 的相关笔记整理
date: 2018-03-10 12:04:57
tags:
---

因为项目用的脚手架是[react-boilerplate](https://github.com/react-boilerplate/react-boilerplate),只要在创建容器(containers)选择添加redux,
则会自动生成 actions.js , constants.js , index.js , reducer.js , saga.js , selectors.js 五个文件.

index.js 就不用解释了吧.


## Constants.js

用来定义 type 的

        /*
         * AppConstants
         * Each action has a corresponding type, which the reducer knows and picks up on.
         * To avoid weird typos between the reducer and the actions, we save them as
         * constants here. We prefix them with 'yourproject/YourComponent' so we avoid
         * reducers accidentally picking up actions they shouldn't.
         *
         * Follow this format:
         * export const YOUR_ACTION_CONSTANT = 'yourproject/YourContainer/YOUR_ACTION_CONSTANT';
         */

        export const LOAD_REPOS = 'boilerplate/App/LOAD_REPOS';
        export const LOAD_REPOS_SUCCESS = 'boilerplate/App/LOAD_REPOS_SUCCESS';
        export const LOAD_REPOS_ERROR = 'boilerplate/App/LOAD_REPOS_ERROR';

>需要新增 type 时,直接更具上面写法更改就好了,这个 type 就相当于以一个指令,当你需要更改数据时就会发送这样一个 type 信号,然后在
action.js, saga.js, reducer.js , 文件就会接收到这个信号,做出相应的操作.

## reducer.js

初始化 state

        /*
         * AppReducer
         *
         * The reducer takes care of our data. Using actions, we can change our
         * application state.
         * To add a new action, add it to the switch statement in the reducer function
         *
         * Example:
         * case YOUR_ACTION_CONSTANT:
         *   return state.set('yourStateVariable', true);
         */

        import { fromJS } from 'immutable';

        import {
          LOAD_REPOS_SUCCESS,
          LOAD_REPOS,
          LOAD_REPOS_ERROR,
        } from './constants';

        // The initial state of the App
        const initialState = fromJS({
          loading: false,
          error: false,
          currentUser: false,
          userData: {
            repositories: false,
          },
        });

        function appReducer(state = initialState, action) {
          switch (action.type) {
            case LOAD_REPOS:
              return state
                .set('loading', true)
                .set('error', false)
                .setIn(['userData', 'repositories'], false);
            case LOAD_REPOS_SUCCESS:
              return state
                .setIn(['userData', 'repositories'], action.repos)
                .set('loading', false)
                .set('currentUser', action.username);
            case LOAD_REPOS_ERROR:
              return state
                .set('error', action.error)
                .set('loading', false);
            default:
              return state;
          }
        }

        export default appReducer;

>其中 initialState 就是初始化 参数, appReducer 是对检测到相应的 type 值,进行相应的赋值, 其中的 action.username 是在发送 type 的函数
附带的参数值.这个参数会在 actions.js 定义方法的时候定义.

        //将 值(true) 赋给 初始化的参数('loading')
        .set('loading',true)

## actions.js

        /*
         * App Actions
         *
         * Actions change things in your application
         * Since this boilerplate uses a uni-directional data flow, specifically redux,
         * we have these actions which are the only way your application interacts with
         * your application state. This guarantees that your state is up to date and nobody
         * messes it up weirdly somewhere.
         *
         * To add a new Action:
         * 1) Import your constant
         * 2) Add a function like this:
         *    export function yourAction(var) {
         *        return { type: YOUR_ACTION_CONSTANT, var: var }
         *    }
         */

        import {
          LOAD_REPOS,
          LOAD_REPOS_SUCCESS,
          LOAD_REPOS_ERROR,
        } from './constants';

        /**
         * Load the repositories, this action starts the request saga
         *
         * @return {object} An action object with a type of LOAD_REPOS
         */
        export function loadRepos() {
          return {
            type: LOAD_REPOS,
          };
        }

        /**
         * Dispatched when the repositories are loaded by the request saga
         *
         * @param  {array} repos The repository data
         * @param  {string} username The current username
         *
         * @return {object}      An action object with a type of LOAD_REPOS_SUCCESS passing the repos
         */
        export function reposLoaded(repos, username) {
          return {
            type: LOAD_REPOS_SUCCESS,
            repos,
            username,
          };
        }

        /**
         * Dispatched when loading the repositories fails
         *
         * @param  {object} error The error
         *
         * @return {object}       An action object with a type of LOAD_REPOS_ERROR passing the error
         */
        export function repoLoadingError(error) {
          return {
            type: LOAD_REPOS_ERROR,
            error,
          };
        }

>actions.js 是告诉机制发生了某个type信号的时候,带了什么参数值,然后回到 reducer.js 判断同一个type 值进行赋值.

其中 loadRepos() ,repoLoadingError(),都没有带其他参数.而我们在reducer.js 里定义接受这些发送过来的type值时,也没有需要接受其他的值.
但是 reposLoaded(),这里带了两个参数(repos, username),我们也能在reducer.js 里定义接受这些发送过来的type值时,需要到这两个值,将其赋给 currentUser,['userData', 'repositories'].

## selectors.js

        /**
         * The global state selectors
         */

        import { createSelector } from 'reselect';

        const selectGlobal = (state) => state.get('global');

        const selectRoute = (state) => state.get('route');

        const makeSelectCurrentUser = () => createSelector(
          selectGlobal,
          (globalState) => globalState.get('currentUser')
        );

        const makeSelectLoading = () => createSelector(
          selectGlobal,
          (globalState) => globalState.get('loading')
        );

        const makeSelectError = () => createSelector(
          selectGlobal,
          (globalState) => globalState.get('error')
        );

        const makeSelectRepos = () => createSelector(
          selectGlobal,
          (globalState) => globalState.getIn(['userData', 'repositories'])
        );

        const makeSelectLocation = () => createSelector(
          selectRoute,
          (routeState) => routeState.get('location').toJS()
        );

        export {
          selectGlobal,
          makeSelectCurrentUser,
          makeSelectLoading,
          makeSelectError,
          makeSelectRepos,
          makeSelectLocation,
        };

>selectors.js 是用来给 index.js 取值的. 在index.js通过props取值. 如下

>以上是redux的基本实现,一步一步按定义的来,可以很快上手的.

## saga.js

这个是用来执行异步请求的

        /**
         * Gets the repositories of the user from Github
         */

        import { call, put, select, takeLatest } from 'redux-saga/effects';
        import { LOAD_REPOS } from 'containers/App/constants';
        import { reposLoaded, repoLoadingError } from 'containers/App/actions';

        import request from 'utils/request';
        import { makeSelectUsername } from 'containers/HomePage/selectors';

        /**
         * Github repos request/response handler
         */
        export function* getRepos() {
          // Select username from store
          const username = yield select(makeSelectUsername());
          const requestURL = `https://api.github.com/users/${username}/repos?type=all&sort=updated`;

          try {
            // Call our request helper (see 'utils/request')
            const repos = yield call(request, requestURL);
            yield put(reposLoaded(repos, username));
          } catch (err) {
            yield put(repoLoadingError(err));
          }
        }

        /**
         * Root saga manages watcher lifecycle
         */
        export default function* githubData() {
          // Watches for LOAD_REPOS actions and calls getRepos when one comes in.
          // By using `takeLatest` only the result of the latest API call is applied.
          // It returns task descriptor (just like fork) so we can continue execution
          // It will be cancelled automatically on component unmount
          yield takeLatest(LOAD_REPOS, getRepos);
        }

>其中 yield takeLatest(LOAD_REPOS, getRepos) 就是用来监听 type 值发出的信号的. 当应用发送了 'LOAD_REPOS'
这个信号时,就会自动的执行 getRepos() 方法. 在getRepos方法中,我们可以看到,当请求完成成功后,会调用 actions.js 里的
reposLoaded(repos, username),并把repos, username两个参数值附送过去

## index.js

###取值

        const mapStateToProps = createStructuredSelector({
              repos: makeSelectRepos(),
              username: makeSelectUsername(),
              loading: makeSelectLoading(),
              error: makeSelectError(),
            });

            const { repos, username, loading, error} = this.props ;

### 发送type 其实就是发送action

        dispatch(loadRepos());

>loadRepos(),在actions.js定义了他所带的type值是:  LOAD_REPOS, 所以saga.js 也会因此触发 getRepos.通过getRepos函数会触发reposLoaded,
repoLoadingError 的 action. 然后再触发reducer.js 里的赋值,从而反应给selectors.js 而刷新界面.


## Tips

>以我开发这段时间来看哦,尽量就是当前容器用只用当前容器的 actions.js , constants.js , reducer.js , saga.js , selectors.js.
除非是app.js 下的 全局值. 如果容器引入其他容器的那些type ,action.很容易造成不必要的错误.特别是用了 react-loadable (按需加载).