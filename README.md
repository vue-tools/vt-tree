# vt-tree
树形多选组件
 
# Use 
```shell
$ npm install vt-tree --save
```

```shell
 import Tree from 'vt-tree'
 
 Vue.component('Tree', Tree)
```
 
 
# Api 
 
### Props 
*   `data` Array类型，如下 
       ```
        [
           {
              id: 1,
              text: "一级1",
              children:
                [
                 {id: 11,text: "一级11" },
                 {id: 12,text: "一级12" }
                ]
           },
           {  
              id: 2,
              name: "一级2"
           },
           {  
              id: 3,
              name: "一级3"
           }
        ]
         ```
*   `textField` 显示文本的字段，默认是 `'text'` 
*   `childrenField` 子节点数据的字段，默认`'children'`
*   `checkbox` 是否显示复选框，默认`false`
*   `checkedField` 控制复选框选中状态的字段，默认是 `'checked'` 
*   `cascadeCheck` 选中节点时是否选中该节点下所有的子节点，默认`true`


### Event 
*   `check` 选中节点时触发的事件，事件参数为Array类型的所有选中的节点
 
### Methods 
*   `getCheckedNodes` 返回所有选中的节点的Array数组