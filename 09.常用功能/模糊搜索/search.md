```tsx
import React, { useState, ReactNode } from 'react'
import { Checkbox, Space, TreeSelect, TreeSelectProps } from 'antd'
import emptyImg from '../../assets/empty.png'
import CIcon from '../CIcon'
import classNames from 'classnames'
import './index.less'
interface CTreeSelectProps extends TreeSelectProps {
  /** @deprecated */
  widthSize?: string //1.值为default 搜索区宽度240px 2.值为large 表单区宽度360px
  allowClear?: boolean
  showAllBtn?: boolean // 是否显示全选按钮
  indeterminate?: boolean // 树全选按钮半选状态
  treeAllChecked?: boolean // 树全选默认勾状态
  onTreeCheckAllChange?: (evt: any) => void // 树全选change
  className?: string // 设置tree的样式名
}
const CTreeSelect: React.FC<CTreeSelectProps> = (props) => {
  const {
    widthSize,
    placeholder = '请选择',
    allowClear = true,
    className,
    style,
    showAllBtn,
    indeterminate,
    treeAllChecked,
    onTreeCheckAllChange,
    ...resetProps
  } = props
  const [isDown, setIsDown] = useState<boolean>(true)
  const onFocus = () => {
    setIsDown(false)
    props.onFocus ? props.onFocus.call(props) : null
  }
  const onBlur = () => {
    setIsDown(true)
    props.onBlur ? props.onBlur.call(props) : null
  }
  /**
   * @description: 下拉框渲染
   * @param {*} originNode
   */
  const dropdownRender = (originNode: ReactNode) => {
    return (
      <div className={classNames('c-select-tree-wrapper', className)}>
        {showAllBtn && (
          <Space className="c-unit-checkbox">
            <Checkbox indeterminate={indeterminate} onChange={onTreeCheckAllChange} checked={treeAllChecked}>
              全选
            </Checkbox>
          </Space>
        )}
        {originNode}
      </div>
    )
  }
  return (
    <TreeSelect
      style={{ ...{ width: widthSize === 'large' ? 360 : 240 }, ...style }}
      className={`${className ? `c-tree-select ${className}` : 'c-tree-select'}`}
      notFoundContent={
        <div className={'c-select-tree-empty-wrapper'}>
          <img src={emptyImg} className={'img'} />
          <div className={'text'} style={{ marginTop: 4 }}>
            暂无数据
          </div>
        </div>
      }
      dropdownRender={dropdownRender}
      {...resetProps}
      placeholder={placeholder}
      showSearch
      maxTagCount="responsive"
      showArrow={true}
      allowClear={allowClear}
      onFocus={onFocus}
      onBlur={onBlur}
      suffixIcon={isDown ? <CIcon iconType={'icon-a-Inputbox_down'} /> : <CIcon iconType={'icon-a-Inputbox_up'} />}
    />
  )
}
export default CTreeSelect
```

```tsx
import { CTreeSelect } from '@S0060013.01/cbs-common-ui'
import {
  CommandModuleDataConfigProps,
  useCommandModuleTreeData,
  useCommandModuleTreeDataForApprove,
} from '@src/hooks'
import { TreeSelect, TreeSelectProps } from 'antd'
import { CheckboxChangeEvent } from 'antd/es/checkbox/Checkbox'
import { isObject } from 'lodash'
import React, { useCallback, useEffect, useMemo, useState } from 'react'
import useSelectFormateValue from './useSelectFormateValue'

interface CommandModuleTreeSelect extends Omit<TreeSelectProps, 'treeData' | 'onChange'> {
  widthSize?: string //1.值为default 搜索区宽度240px 2.值为large 表单区宽度360px
  dataConfig: CommandModuleDataConfigProps
  onChange?: (value?: any) => void
  showAllBtn?: boolean
}

const CommandTypeSelect = (props: CommandModuleTreeSelect) => {
  const {
    widthSize = 'large',
    showAllBtn = true,
    dataConfig,
    treeCheckable = true,
    showCheckedStrategy = TreeSelect.SHOW_CHILD,
    labelInValue,
    onChange,
    value,
    ...rest
  } = props
  const functionalData = useCommandModuleTreeData(dataConfig)

  const { returnValue, allChecked, halfChecked } = useSelectFormateValue(value, {
    labelInValue,
    showCheckedStrategy,
    ...functionalData,
  })

  useEffect(() => {
    if (JSON.stringify(value) !== JSON.stringify(returnValue)) {
      if (onChange) {
        onChange(returnValue)
      }
    }
  }, [JSON.stringify(value), JSON.stringify(returnValue)])

  /**
   * 兼容处理值的展示,防止警告
   */
  const valueShow = useMemo(() => {
    if (labelInValue && value) {
      if (!isObject(value)) {
        return undefined
      }
      if (Array.isArray(value) && value[0] && !isObject(value[0])) {
        return undefined
      }
    }
    return value
  }, [value, labelInValue])

  /**
   * 全选
   */
  const handleAllCheckChange = useCallback(
    (e: CheckboxChangeEvent) => {
      if (onChange) {
        onChange(e.target.checked ? Object.keys(functionalData.treeDataMap) : [])
      }
    },
    [onChange, functionalData.treeDataMap]
  )

  /**
   * 模糊搜索
   */
  const filterTreeNode = (input, treenode) => {
    if (treenode.children) {
      return treenode.children.some(child => {
        filterTreeNode(input, child)
      })
    }
    if (treenode.title.indexOf(input) > -1) {
      if (treenode.children) {
        treenode.children = treenode.children.filter(child => {
          filterTreeNode(input, child)
        })
      }
      const o = treenode
      console.log('treenode',o)
      return true
    }
    return false
  }

  return (
    <CTreeSelect
      widthSize={widthSize}
      treeData={functionalData.treeData}
      treeCheckable={treeCheckable}
      showCheckedStrategy={showCheckedStrategy}
      labelInValue={labelInValue}
      onChange={onChange}
      value={valueShow}
      showAllBtn={showAllBtn}
      indeterminate={halfChecked}
      treeAllChecked={allChecked}
      onTreeCheckAllChange={handleAllCheckChange}
      filterTreeNode={filterTreeNode}
      {...rest}
    />
  )
}

CommandTypeSelect.Approve = ((props: Partial<CommandModuleTreeSelect>) => {
  const { dataConfig = {}, ...rest } = props
  const data = useCommandModuleTreeDataForApprove(dataConfig)

  return (
    <CommandTypeSelect
      {...rest}
      dataConfig={{
        initConfig: {
          initData: data,
          preventFetch: true,
        },
      }}
    />
  )
}) as React.FC<Partial<CommandModuleTreeSelect>>

export default CommandTypeSelect
```

```tsx
import { useCallback, useMemo } from 'react'
import { TreeSelect, TreeSelectProps } from 'antd'
import { isArray, isNumber, isObject, isString } from 'lodash'
import { CommandModuleType } from '@src/models/instructionMessageQuery'

export interface FunctionalModuleReturnType<T = CommandModuleType> {
  loading: boolean
  treeData: TreeSelectProps['treeData']
  treeDataMap: Record<string, TreeSelectProps['treeData'][number]>
  originDataMap: Record<string, T>
  originList: T[]
}

/**
 * 获取原始数据
 * @param key
 * @returns
 */
const getMapData = <T = any>(key: string | number, dataMap: Record<string, T>): T => {
  let data = dataMap[key]
  if (!data) {
    console.warn(`no match function module for key ${key}`)
  }
  return data
}

/**
 * 获取指定key
 * @param value
 * @returns
 */
const getReturnKey = (value) => {
  if (isString(value) || isNumber(value)) {
    return value
  }
  return value.value
}

/**
 * 获取包含选中节点以及其下所有节点ids
 * @param treeList
 * @param checkStatus
 */
const getSelfWithChildrenIds = (
  treeList: TreeSelectProps['treeData'],
  checkStatus: boolean | (string | number)[]
): (string | number)[] => {
  return treeList.reduce((pre, cur) => {
    if (
      (Array.isArray(checkStatus) && checkStatus.includes(cur.value)) ||
      (typeof checkStatus === 'boolean' && checkStatus)
    ) {
      return [...pre, cur.value, ...getSelfWithChildrenIds(cur.children || [], true)]
    }
    return [...pre, ...getSelfWithChildrenIds(cur.children || [], checkStatus)]
  }, [] as any)
}

/**
 * 获取选中状态
 * @param selectKeys
 * @param allKeys
 */
const getSelectStatus = <T = number | string>(selectKeys: T[], allKeys: T[]) => {
  let allChecked = allKeys.length && selectKeys.length === allKeys.length
  let halfChecked = selectKeys.length && selectKeys.length < allKeys.length
  return { allChecked, halfChecked }
}

/**
 * 底层向上推获取选中的父节点
 * @param treeList
 * @param checkStatus
 */
const getSelectedParentUpIds = (
  checkStatus: (string | number)[],
  nodeMap?: Record<string, TreeSelectProps['treeData'][number]>
) => {
  let filterList = []
  let checkStatusClone = [...checkStatus]

  while (checkStatusClone.length) {
    let key = checkStatusClone[0]
    let parentId = nodeMap[key].pId
    if (!parentId) {
      checkStatusClone = checkStatusClone.filter((v) => v !== key)
      filterList.push(key)
      continue
    }
    let parent = nodeMap[parentId]
    let childrenIds = parent.children.map((v) => v.value)
    let allIn = childrenIds.every((key) => checkStatusClone.includes(key))
    if (allIn) {
      checkStatusClone = checkStatusClone.filter((key) => !childrenIds.includes(key))
      checkStatusClone.push(parentId)
    } else {
      checkStatusClone = checkStatusClone.filter((v) => v !== key)
      filterList.push(key)
    }
  }

  return filterList
}

const useSelectFormateValue = (
  value,
  {
    labelInValue,
    showCheckedStrategy,
    loading,
    treeData,
    treeDataMap,
    originDataMap,
  }: Pick<TreeSelectProps, 'labelInValue' | 'showCheckedStrategy'> & FunctionalModuleReturnType
) => {
  //匹配select,支持string/number/array/object
  const isValid = isString(value) || isNumber(value) || isObject(value)

  const isSingle = isString(value) || isNumber(value) || (isObject(value) && !isArray(value))

  const isEmpty = value == undefined

  /**
   * 返回封装处理的数据
   */
  const getReturnValue = useCallback(
    (key) => {
      if (!labelInValue) {
        return key
      }

      const node = getMapData(key, treeDataMap)
      const origin = getMapData(key, originDataMap)
      return { label: node?.title, value: key, disabled: !!node.disabled, origin }
    },
    [labelInValue, treeDataMap, originDataMap]
  )

  const strategyCheckedStatus = useMemo(() => {
    let status: { keys: (string | number)[]; allChecked: boolean; halfChecked: boolean } = {
      keys: [],
      allChecked: false,
      halfChecked: false,
    }

    if (!isValid || isEmpty || loading) {
      return status
    }

    let allKeys = Object.keys(treeDataMap)

    let allChildrenKeys = allKeys.filter((key) => {
      let node = getMapData(key, treeDataMap)
      return !node?.children?.length
    })

    if (isSingle) {
      status.keys = [getReturnKey(value)]
      const { allChecked, halfChecked } = getSelectStatus(status.keys, allKeys)
      status.allChecked = allChecked
      status.halfChecked = halfChecked
      return status
    }

    let initKeys = value.map((v) => {
      return getReturnKey(v)
    })

    //todo 1.父子非关联的处理 2.对disabled的过滤

    let allSelectKeys = getSelfWithChildrenIds(treeData, initKeys)

    let allSelectChildrenKeys = allSelectKeys.filter((key) => {
      let node = getMapData(key, treeDataMap)
      return !node?.children?.length
    })

    const { allChecked, halfChecked } = getSelectStatus(allSelectChildrenKeys, allChildrenKeys)

    status.allChecked = allChecked

    status.halfChecked = halfChecked

    status.keys = allSelectKeys

    if (showCheckedStrategy === TreeSelect.SHOW_CHILD) {
      status.keys = allSelectChildrenKeys
    }

    if (showCheckedStrategy === TreeSelect.SHOW_PARENT) {
      status.keys = getSelectedParentUpIds(allSelectChildrenKeys, treeDataMap)
    }

    return status
  }, [loading, value, treeData, treeDataMap, isValid, isSingle, isEmpty, showCheckedStrategy])

  const returnValue = useMemo(() => {
    if (isEmpty || !isValid) {
      return undefined
    }

    const { keys } = strategyCheckedStatus

    let values = keys.map((key) => getReturnValue(key))

    if (isSingle) {
      return values[0]
    }

    return values
  }, [strategyCheckedStatus, isValid, isSingle, isEmpty, getReturnValue])

  //数据加载中则直接返回
  if (loading) {
    return { returnValue: value, allChecked: false, halfChecked: false }
  }

  return {
    returnValue,
    allChecked: strategyCheckedStatus.allChecked,
    halfChecked: strategyCheckedStatus.halfChecked,
  }
}

export default useSelectFormateValue

```

````tsx
export interface CommandModuleType {
  key: string
  value: string
  typeList: CommandModuleType[]
}
````

````tsx
import { queryCommandClassifyTypeList } from '@src/api/direct-connect/api'
import { CommandModuleType } from '@src/models/instructionMessageQuery'
import { TreeSelectProps } from 'antd'
import { useEffect, useMemo, useState } from 'react'

export interface CommandModuleReturnType<T = CommandModuleType> {
  loading: boolean
  treeData: TreeSelectProps['treeData']
  treeDataMap: Record<string, TreeSelectProps['treeData'][number]>
  originDataMap: Record<string, T>
  originList: T[]
}
export interface CommandModuleDataConfigProps<T = CommandModuleType> {
  api?: () => Promise<T[]>
  config?: { filter?: (p: T) => boolean; handler?: (p: T) => TreeSelectProps['treeData'][number]; filterRoot?: boolean }
  initConfig?: {
    preventFetch: boolean
    initData: CommandModuleReturnType<T>
  }
}

/**
 * 格式化树形数据
 * @param data
 * @param parentId
 * @param handler
 */
const formatTreeData = <T extends CommandModuleType>(
  data: T[] = [],
  pId = '',
  childrenKey = 'children',
  handler: (p: T) => TreeSelectProps['treeData'][number],
  filter?: (v: T) => boolean
): TreeSelectProps['treeData'] => {
  return data.reduce((pre = [], cur: T) => {
    if (filter && filter(cur)) {
      return pre
    }
    let tmp: any = { ...handler(cur), pId }
    tmp.children =
      cur[childrenKey] && cur[childrenKey].length
        ? formatTreeData(cur[childrenKey], tmp.value, childrenKey, handler, filter)
        : null
    pre.push(tmp)
    return pre
  }, [])
}

/**
 * 获取树形所有节点map
 * @param treeList
 * @param obj
 */
const flattenTreeNodeMap = (
  treeList: CommandModuleType[] | TreeSelectProps['treeData'],
  childrenKey = 'children',
  valueKey: 'value' | 'key' = 'value',
  obj = {}
): Record<string, CommandModuleType> => {
  treeList.forEach((v) => {
    obj[v[valueKey]] = v
    if (v[childrenKey] && v[childrenKey].length) {
      flattenTreeNodeMap(v[childrenKey], childrenKey, valueKey, obj)
    }
  })
  return obj
}

export const useCommandModuleTreeData = (props: CommandModuleDataConfigProps): CommandModuleReturnType => {
  const { api, config, initConfig } = props
  const { filter, handler, filterRoot = true } = config || {}
  const { preventFetch = false, initData } = initConfig || {}
  const [originList, seOriginList] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    if (!preventFetch) {
      fetchTreeData()
    }
  }, [preventFetch])

  const fetchTreeData = async () => {
    try {
      setLoading(true)
      if (api) {
        const list = await api()
        seOriginList(list)
      }
    } catch (error) {
    } finally {
      setLoading(false)
    }
  }

  const treeData = useMemo(() => {
    let list = formatTreeData(
      originList,
      '',
      'typeList',
      (p) => {
        return {
          title: p.value,
          value: p.key,
          ...(handler ? handler(p) : {}),
        }
      },
      filter
    )

    if (filterRoot) {
      return list.filter((v) => !!v.children && !!v.children.length)
    }

    return list
  }, [filter, filterRoot, handler, originList])

  const treeDataMap = useMemo(() => flattenTreeNodeMap(treeData), [treeData])

  const originDataMap = useMemo(() => flattenTreeNodeMap(originList, 'typeList', 'key'), [originList])

  if (preventFetch) {
    return initData
  }

  return {
    loading,
    treeData,
    treeDataMap,
    originDataMap,
    originList,
  }
}

export const useCommandModuleTreeDataForApprove = (props: CommandModuleDataConfigProps = {}) => {
  const { config = {}, ...rest } = props
  return useCommandModuleTreeData({
    ...rest,
    api: queryCommandClassifyTypeList,
    config: {
      filterRoot: true,
      ...config,
    },
  })
}

export default useCommandModuleTreeData
````

