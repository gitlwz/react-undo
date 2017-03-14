Undo 是通过包装reducer 创建一个state History,保留历史的state，可以做退一步，进一步的操作；


API（包装reducer，其中config参数为history配置）

import undoable from 'redux-undo';
undoable(reducer)
undoable(reducer, config)


和 combineReducers 配合使用

combineReducers({
  counter: undoable(counter)
})


包装后的renducers 变成了，可通过state.present (获取当前), state.past (获取过去)
{
  past: [...pastStatesHere...],
  present: {...currentStateHere...},
  future: [...futureStatesHere...]
}


store.dispatch(ActionCreators.undo()) // undo the last action 退一步
store.dispatch(ActionCreators.redo()) // redo the last action 进一步

store.dispatch(ActionCreators.jump(-2)) // undo 2 steps
store.dispatch(ActionCreators.jump(5)) // redo 5 steps

store.dispatch(ActionCreators.jumpToPast(index)) // jump to requested index in the past[] array
store.dispatch(ActionCreators.jumpToFuture(index)) // jump to requested index in the future[] array

store.dispatch(ActionCreators.clearHistory()) // [beta only] Remove all items from past[] and future[] arrays


配置项config
undoable(reducer, {
  limit: false, // set to a number to turn on a limit for the history // 保存到历史的数量

  filter: () => true, // see `Filtering Actions` section　　//过滤一部分action，不记录/记录在history

  undoType: ActionTypes.UNDO, // define a custom action type for this undo action
  redoType: ActionTypes.REDO, // define a custom action type for this redo action

  jumpType: ActionTypes.JUMP, // define custom action type for this jump action

  jumpToPastType: ActionTypes.JUMP_TO_PAST, // define custom action type for this jumpToPast action
  jumpToFutureType: ActionTypes.JUMP_TO_FUTURE, // define custom action type for this jumpToFuture action

  clearHistoryType: ActionTypes.CLEAR_HISTORY, // [beta only] define custom action type for this clearHistory action

  initTypes: ['@@redux-undo/INIT'] // history will be (re)set upon init action type

  debug: false, // set to `true` to turn on debugging

  neverSkipReducer: false, // prevent undoable from skipping the reducer on undo/redo
})

详细的可以看下一http://www.cnblogs.com/miaowwwww/p/6057992.html


现在问题是
1,多次触发action后历史树过大的问题；
2,在react-router中所有page都构建历史树，是否有必要！(这点我想undo可定是有解决的方案，是我理解太浅！)
3,undo和react-router-redux中的历史回退！