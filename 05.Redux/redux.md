redux将所有数据存储到树中，且树是唯一的。

Redux基本概念
store：存储树结构。
state：维护的数据，一般维护成树的结构。
reducer：对state进行更新的函数，每个state绑定一个reducer。传入两个参数：当前state和action，返回新state。
action：一个普通对象，存储reducer的传入参数，一般描述对state的更新类型。
dispatch：传入一个参数action，对整棵state树操作一遍。
React-Redux基本概念
Provider组件：用来包裹整个项目，其store属性用来存储redux的store对象。
connect(mapStateToProps, mapDispatchToProps)函数：用来将store与组件关联起来。
mapStateToProps：每次store中的状态更新后调用一次，用来更新组件中的值。
mapDispatchToProps：组件创建时调用一次，用来将store的dispatch函数传入组件。

