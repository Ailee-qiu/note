````tsx
const CommandTypeSelect = (props: CommandModuleTreeSelect) => {
  const {
    ...
    showAllBtn, // 将showAllBtn作为props传入
    ...
  } = props

  // 在filterTreeNode方法中增加匹配标识
  const filterTreeNode = (input, treenode) => {
    let matched = false; // 初始值为false
    if (treenode.children) {
      treenode.children.forEach(child => {
        if (filterTreeNode(input, child)) {
          matched = true; // 如果子节点有匹配，则标记为true
        }
      });
      if (!matched) {
        return false; // 如果子节点都没有匹配，则当前节点也不匹配
      }
    }
    if (treenode.title.indexOf(input) > -1) {
      if (treenode.children) {
        treenode.children = treenode.children.filter(child => {
          return filterTreeNode(input, child); // 将匹配标识向下传递
        })
      }
      return true;
    }
    return matched; // 返回匹配标识
  }

  // 根据匹配标识来动态设置showAllBtn的值
  const [showAll, setShowAll] = useState(showAllBtn);
  useEffect(() => {
    if (!showAll && !filterTreeNodeValue) {
      setShowAll(true); // 如果没有匹配节点，则显示全选按钮
    } else if (showAll && filterTreeNodeValue) {
      setShowAll(false); // 如果有匹配节点，则隐藏全选按钮
    }
  }, [filterTreeNodeValue, showAll]);

  ...

  return (
    <CTreeSelect
      ...
      showAllBtn={showAll} // 使用动态的showAllBtn值
      ...
    />
  )
}
````



````tsx
const filterTreeNode = (input, treenode) => {
  let matchCount = 0;

  if (treenode.children) {
    treenode.children = treenode.children.filter(child => {
      if (filterTreeNode(input, child)) {
        matchCount++;
        return true;
      }
      return false;
    });
  }

  if (treenode.title.indexOf(input) > -1) {
    matchCount++;
    return true;
  }

  return false;
};

const [showAllBtn, setShowAllBtn] = useState<boolean>(true);

useEffect(() => {
  setShowAllBtn(functionalData.treeData.length > 0 && matchCount > 0);
}, [functionalData.treeData.length, matchCount]);

````

````tsx
const CommandTypeSelect = (props: CommandModuleTreeSelect) => {
  // ...

  /**
   * 全选
   */
  const handleAllCheckChange = useCallback(
    (e: CheckboxChangeEvent) => {
      if (onChange) {
        let newValue: any[] = []
        functionalData.treeData.forEach(node => {
          if (node.children) {
            node.children.forEach(child => {
              if (child.matched && !newValue.includes(child.key)) {
                newValue.push(child.key)
              }
            })
          }
        })
        onChange(newValue)
      }
    },
    [onChange, functionalData.treeData]
  )

  /**
   * 模糊搜索
   */
  const filterTreeNode = (input, treenode) => {
    let matched = false
    if (treenode.children) {
      treenode.children = treenode.children.filter(child => {
        return filterTreeNode(input, child)
      })
      matched = treenode.children.length > 0
    }
    if (treenode.title.indexOf(input) > -1) {
      matched = true
    }
    treenode.matched = matched
    return matched
  }

  return (
    <CTreeSelect
      widthSize={widthSize}
      treeData={functionalData.treeData}
      treeCheckable={treeCheckable}
      showCheckedStrategy={showCheckedStrategy
                          }
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
````

