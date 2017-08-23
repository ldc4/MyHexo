title: 红黑树Java实现
date: 2016-05-03 19:45:20
categories:
- DataStructure
tags:
- RBT
---

![](http://ldc4.qiniudn.com/images/RBTree/RBTree-1.png "红黑树")

<!--more-->

## 红黑树定义

红黑树是每个节点都带有颜色属性的二叉查找树，颜色为红色或黑色。在二叉查找树强制一般要求以外，对于任何有效的红黑树我们增加了如下的额外要求：

	1. 节点是红色或黑色。
	2. 根是黑色。
	3. 所有叶子都是黑色（叶子是NIL节点）。
	4. 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）
	5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

（摘自维基百科）


## 红黑树Java代码实现

### 节点颜色
```
//节点颜色
enum Color{
	BLACK,
	RED
}
```

### 红黑树节点
```
//红黑树节点
class RBTNode {
	
	private int value; //值
	private RBTNode left;//左子节点
	private RBTNode right;//右子节点
	private RBTNode parent;//父亲节点
	private Color color;//节点颜色
	
	
	public RBTNode(int value){
		this(value,Color.BLACK);
	}
	
	public RBTNode(int value,Color color){
		this(value,null,null,null,color);
	}
	
	public RBTNode(int value, RBTNode left, RBTNode right, RBTNode parent, Color color) {
		this.value = value;
		this.left = left;
		this.right = right;
		this.parent = parent;
		this.setColor(color);
	}


	public int getValue() {
		return value;
	}

	public void setValue(int value) {
		this.value = value;
	}

	public RBTNode getLeft() {
		return left;
	}

	public void setLeft(RBTNode left) {
		this.left = left;
	}

	public RBTNode getRight() {
		return right;
	}

	public void setRight(RBTNode right) {
		this.right = right;
	}

	public RBTNode getParent() {
		return parent;
	}

	public void setParent(RBTNode parent) {
		this.parent = parent;
	}

	public Color getColor() {
		return color;
	}

	public void setColor(Color color) {
		this.color = color;
	}
	
	
	//找到祖父节点
	public RBTNode getGrandparent(){
		return this.getParent().getParent();
	}
	//找到叔父节点
	public RBTNode getUncle(){
		if(this.parent == this.getGrandparent().left){
			return getGrandparent().right;
		}else{
			return getGrandparent().left;
		}
	}
	//找到兄弟节点
	public RBTNode getSibling(){
		if(this == this.parent.left){
			return this.parent.right;
		}else{
			return this.parent.left;
		}
	}
}
```

### 红黑树
```
public class RBTree{
	
	//NIL节点，即根节点，要求为黑色
	private final RBTNode NIL = new RBTNode(-1);
	
	private RBTNode root;
	
	public RBTree(){
		root = NIL;
	}
	
	public RBTree(RBTNode node){
		root = node;
	}
	
	
	//插入节点
	public void insert(RBTNode node){
		RBTNode temp = root,previous = NIL;
		
		//找到插入节点
		while(temp!=NIL){
			previous = temp;
			if(temp.getValue() < node.getValue()){
				temp = previous.getRight();
			}else if(temp.getValue() > node.getValue()){
				temp = previous.getLeft();
			}else{
				return;
			}
		}
		
		node.setParent(previous);
		
		//如果是空树
		if(previous==NIL){
			root = node;
		}else if(previous.getValue() > node.getValue()){
			previous.setLeft(node);
		}else{
			previous.setRight(node);
		}
		
		node.setLeft(NIL);
		node.setRight(NIL);
		node.setColor(Color.RED);//因为性质5的约束，插入节点必须是红节点（其实这里可以理解为，插入节点为红色，可以更方便操作）
		insert1(node);
	}

	

	//插入情形1:默认插入节点是红色，下面用N表示插入节点，P表示父亲节点，U表示叔父节点
	//插入节点为根节点（插入是作为叶子节点插入的，这里要注意红黑树的nil叶子节点与树的叶子节点的区别）
	private void insert1(RBTNode node){
		if(node.getParent() == NIL){
			node.setColor(Color.BLACK);
		}else{
			insert2(node);
		}
	}
	//插入情形2:N红P黑U任意
	//如果父节点是黑色，则没有影响。
	private void insert2(RBTNode node){
		if(node.getParent().getColor() == Color.BLACK){
			return ;
		}else{
			insert3(node);
		}
	}
	//插入情形3:N红P红U红
	//如果叔父节点不为空且是红色(由于前面的情况排除，到达这里 父亲节点是红色)
	private void insert3(RBTNode node) {
		//这里有个隐形的条件：祖父节点一定是存在的，且为黑
		if(node.getUncle()!=NIL && node.getUncle().getColor() == Color.RED){
			node.getParent().setColor(Color.BLACK);
			node.getUncle().setColor(Color.BLACK);
			node.getGrandparent().setColor(Color.RED);
			//如果祖父节点是根节点，而根节点要为黑色
			insert1(node.getGrandparent());
		}else{
			insert4(node);
		}
	}
	//插入情形4：N红P红  U黑/NIL (LR或者RL型)
	//叔父节点是黑色或者为空,且插入节点是父节点的右子节点，父节点是祖父节点的左子节点
	//左右旋后变成情况5
	private void insert4(RBTNode node) {
		if(node == node.getParent().getRight() && node.getParent() == node.getGrandparent().getLeft()){
			leftRotate(node.getParent());
			node = node.getLeft();
		}else if(node == node.getParent().getLeft() && node.getParent() == node.getGrandparent().getRight()){
			rightRotate(node.getParent());
			node = node.getRight();
		}
		insert5(node);
	}
	//插入情形5：N红P红 U黑/NIL  (RR或者LL型)
	//父节点P是红色而叔父节点U是黑色或缺少，新节点N是其父节点的左子节点，而父节点P又是其父节点G的左子节点。
	private void insert5(RBTNode node) {
		node.getParent().setColor(Color.BLACK);
		node.getGrandparent().setColor(Color.RED);
		if(node == node.getParent().getLeft() && node.getParent() == node.getGrandparent().getLeft()){
			rightRotate(node.getGrandparent());
		}else{
			leftRotate(node.getGrandparent());
		}
	}

	//左旋
	private void leftRotate(RBTNode node) {
		
		//得到Node的右子节点
		RBTNode nr = node.getRight();
		
		node.setRight(nr.getLeft());
		
		if(node.getRight() != NIL){
			node.getRight().setParent(node);
		}
		
		nr.setLeft(node);
		nr.setParent(node.getParent());
		if(node.getParent() == NIL){
			root = nr;
		}else if(node.getParent().getLeft() == node){
			node.getParent().setLeft(nr);
		}else if(node.getParent().getRight() == node){
			node.getParent().setRight(nr);
		}
		node.setParent(nr);
		
	}
	//右旋
	private void rightRotate(RBTNode node) {

		RBTNode nl = node.getLeft();
		
		node.setLeft(nl.getRight());
		
		if(node.getLeft() != NIL){
			node.getLeft().setParent(node);
		}
		
		nl.setRight(node);
		
		nl.setParent(node.getParent());
		
		if(node.getParent() == NIL){
			root = nl;
		}else if(node.getParent().getLeft() == node){
			node.getParent().setLeft(nl);
		}else if(node.getParent().getRight() == node){
			node.getParent().setRight(nl);
		}

		node.setParent(nl);

	}
		
	//按值删除
	public void deleteWithValue(int value){
		delete(search(value));
	}
	//按节点删除
	public void delete(RBTNode node){
		
		RBTNode temp = NIL;
		RBTNode child = NIL;
		
		if(node == null){
			return ;
		}else{
			if(node.getLeft() == NIL || node.getRight() == NIL){
				temp = node;
			}else{
				temp = successor(node);
			}
			
			//如果不是原节点，只需要把后继节点的值赋值过来就行了
			if(temp != node){
				node.setValue(temp.getValue());
			}

			//因为前面的操作，这里最终退化成删除只有1个孩子的节点的情况，或者叶子节点
			child = temp.getLeft()==NIL?temp.getRight():temp.getLeft();
			
			//删除节点具体行为
			child.setParent(temp.getParent());
			//如果temp是根
			if(temp.getParent() == NIL){
				root = child;
			}else if(temp == temp.getParent().getLeft()){
				temp.getParent().setLeft(child);
			}else if(temp == temp.getParent().getRight()){
				temp.getParent().setRight(child);
			}
			
			if(temp.getColor() == Color.BLACK){
				if(child.getColor() == Color.RED){
					child.setColor(Color.BLACK);
				}else{
					delete1(child);//删除修正，删除黑色节点，儿子节点也为黑色
				}
			}
			
		}
		
	}
		
	//情形1：如果node是新的根,什么也不干.
	//下面用N表示删除节点的孩子节点（即方法调用的child参数），S表示N的兄弟节点，P表示N的父亲节点，SL表示S的左子，SR表示S的右子
	private void delete1(RBTNode node) {
		if(node.getParent() != NIL){
			delete2(node);
		}
	}
	//情形2：S红
	//兄弟节点是红色
	private void delete2(RBTNode node) {
		RBTNode sibling = node.getSibling();
		
		if(sibling.getColor() == Color.RED){
			node.getParent().setColor(Color.RED);
			sibling.setColor(Color.BLACK);
			if(node == node.getParent().getLeft()){
				leftRotate(node.getParent());
			}else{
				rightRotate(node.getParent());
			}
		}
		
		delete3(node);//去执行4,5,6,的情况，不满足3的条件，会直接跳过
	}
	//情形3：P黑S黑SL黑SR黑
	private void delete3(RBTNode node) {
		RBTNode sibling = node.getSibling();
		
		if(node.getParent().getColor() == Color.BLACK 
				&& sibling.getColor() == Color.BLACK
				&& sibling.getLeft().getColor() == Color.BLACK
				&& sibling.getRight().getColor() == Color.BLACK){
			sibling.setColor(Color.RED);
			//这就变成了第一种情况了
			delete1(node.getParent());
		}else{
			delete4(node);
		}
	}
	//情形4：P红S黑SL黑SR黑
	private void delete4(RBTNode node) {
		RBTNode sibling = node.getSibling();
		
		if(node.getParent().getColor() == Color.RED 
				&& sibling.getColor() == Color.BLACK
				&& sibling.getLeft().getColor() == Color.BLACK
				&& sibling.getRight().getColor() == Color.BLACK){
			sibling.setColor(Color.RED);
			node.getParent().setColor(Color.BLACK);
		}else{
			delete5(node);
		}
	}	
	//情形5：S黑SL红SR黑且N是S的父亲的左子( 还要对称一下)
	private void delete5(RBTNode node) {
		RBTNode sibling = node.getSibling();
		
		//这个条件可以不要，因为到达这里只能是红色。
		//情形2执行了的话，兄弟节点就变成了黑色
		//if(sibling.getColor() == Color.BLACK){
		if(node == node.getParent().getLeft() 
				&& sibling.getRight().getColor() == Color.BLACK
				&& sibling.getLeft().getColor() == Color.RED){
			sibling.setColor(Color.RED);
			sibling.getLeft().setColor(Color.BLACK);
			rightRotate(sibling);
		}else if(node == node.getParent().getLeft() 
				&& sibling.getRight().getColor() == Color.BLACK
				&& sibling.getLeft().getColor() == Color.RED){
			sibling.setColor(Color.RED);
			sibling.getRight().setColor(Color.BLACK);
			leftRotate(sibling);
		}
		//}
		delete6(node);
	}
	
	//情形6：S黑SL黑SR红且N是S的父亲的左子（依然对称一下）
	private void delete6(RBTNode node) {
		RBTNode sibling = node.getSibling();
		
		sibling.setColor(node.getParent().getColor());
		node.getParent().setColor(Color.BLACK);
		
		if(node == node.getParent().getLeft()){
			sibling.getRight().setColor(Color.BLACK);
			leftRotate(node.getParent());
		}else{
			sibling.getLeft().setColor(Color.BLACK);
			rightRotate(node.getParent());
		}
	}
	
	
    //查找节点  
    public RBTNode search(int data) {  
    	RBTNode temp = root;  
          
        while (temp != NIL) {  
            if (temp.getValue() == data) {  
                return temp;  
            } else  if (data < temp.getValue()) {  
                temp = temp.getLeft();  
            } else {  
                temp = temp.getRight();  
            }  
        }  
        return null;  
    }  
    
    //查找后继节点 (待仔细分析)(我觉得这里是应该是狭义的后继，只考虑后继的第一种情况)
    public RBTNode successor(RBTNode node) {  
    	RBTNode nr = node.getRight();  
        if(nr != NIL) {  
        	RBTNode previous = null;  
            while (nr != NIL) {  
                previous = nr;  
                nr = nr.getLeft();  
            }  
            return previous;  
        }else{  
        	
        	return node;
        	
        	/*
        	RBTNode parent = node.getParent();  
        	while (parent != NIL && node != parent.getLeft()) {  
                node = parent;  
                parent = parent.getParent();  
        	}   
        	return parent;
        	*/
        } 
    }
	
	//中序遍历红黑树  
    public void printTree() {  
        inOrderTraverse(root);  
    }  
	      
    private void inOrderTraverse(RBTNode node) {  
        if (node != NIL) {  
            inOrderTraverse(node.getLeft());  
            System.out.println(" 节点："+node.getValue() + "的颜色为：" + node.getColor());  
            inOrderTraverse(node.getRight());  
        }  
    }  
}
```

## 红黑树的应用
典型的用途是实现关联数组

例如Java的TreeMap就是基于红黑树（Red-Black tree）的 NavigableMap 实现

TreeSet是基于 TreeMap 的 NavigableSet 实现


## 写在最后

之前一直搞不清楚，平衡树，搜索树，查找树，AVL树，红黑树，B+树，B-树的关系。
但是又害怕被面试官问到这类问题。
正好遇到一个面经题：让你介绍一下平衡树，左旋，右旋。
左旋右旋这东西一直在我脑海中徘徊，但又具体不知道是什么。总之，这种感觉很烦。
本来要去看看平衡树，然后索性就直接找红黑树看了。
红黑树里面的逻辑很复杂，我懒悠悠的花了一天时间才勉强照着别人的代码，以及维基百科的C语言实现，敲出来。
总之，一些基本概念知道了，也会写左旋右旋了，代码就没有继续调试。大差不差的。

在中午吃饭，晚上吃饭的时候，老是想快速省略这步，简单了解下就行了，毕竟快要阿里面试了，合理分配时间很重要。
由此延伸出想法：把时间用在正确的地方。
当然，这个所谓的正确，是很难把握的。
或许，我花一整天时间来实现红黑树，以后在某个时间点某个事情上排上用场了。当然这概率很小，哈哈。
不废话了，赶紧进入下一个阶段。


参考资料：
https://zh.wikipedia.org/wiki/红黑树
http://www.cnblogs.com/fornever/archive/2011/12/02/2270692.html
http://blog.csdn.net/skylinesky/article/details/6610950

源代码：
[红黑树](https://github.com/ldc4/LeetCode/tree/master/src/net/ldc4/argorithm/struct)