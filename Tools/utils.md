一维数组转树形结构

```javascript
const list = [
  { id: 14, name: "米莎", parentId: 2 },
  { id: 2, name: "猎人", parentId: 1 },
  { id: 13, name: "雷欧克", parentId: 2 },
  { id: 3, name: "法师", parentId: 1 },
  { id: 15, name: "法力浮龙", parentId: 3 },
  { id: 1, name: "炉石传说", parentId: 0 },
  { id: 4, name: "战士", parentId: 1 },
  { id: 16, name: "血吼", parentId: 4 }
]

// 数组转 树结构
function List2Tree(list) {

  this.tree = [];
  this.list = list;
  this.tempMap = new Map();

  // 获取树结构片段 返回以该 id 为根元素的 树
  this.findChildren = id => {
    const childTree = [];

    this.list.forEach((item, index) => {
      if (item) {
        // 找到子元素
        if (item.parentId === id) {
          this.list[index] = undefined;

          // 如果已经 递归过子元素
          if (this.tempMap.has(item.id)) {
            childTree.push({
              ...item,
              children: this.tempMap.get(item.id)
            });

            this.tempMap.delete(id);
          } else {
            childTree.push({
              ...item,
              children: this.findChildren(
                item.id,
                this.list.slice(0, index).concat(this.list.slice(index + 1))
              )
            });
          }
        }
      }
    });
    // 缓存递归结果
    this.tempMap.set(id, childTree);
    // 去掉遍历过的元素
    this.list = this.list.filter(item => item);
    return childTree;
  };

  // 拼接成树
  this.splice2Tree = node => {
    let tempTree = {
      ...node,
      children: this.findChildren(node.id)
    };

    const isRoot = this.list.filter(item => item.id === node.parentId);
    if (isRoot.length === 0) {
      this.tree = tempTree;
    } else {
      this.tempMap.set(tempTree.id, tempTree.children);
      this.splice2Tree(isRoot[0]);
    }
  };

  // 获取树
  this.getTree = () => {
    this.splice2Tree(this.list[0]);

    const secRoot = this.tree
    this.tempMap.set(secRoot.id, secRoot.children);

    return this.findChildren(secRoot.parentId);
  };
}

export default List2Tree
const obj = new List2Tree(list)
const treeData = obj.getTree()

console.log(JSON.stringify(treeData, null, 2));
```

