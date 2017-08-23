title: 阿里实习Java研发笔试附加题
date: 2016-04-23 20:44:46
categories:
- ForOffer
tags:
- writter-test
---

阿里笔试附加题确实难。
以至于我花1天时间都不能把他较为完美的解决。
第一题就做了文件分块，第二题算是做完，第三题做了第一个小题。

<!--more-->

# 第一题

>Hadoop是当下大数据处理的事实标准之一，具有广泛的应用场景。作为Hadoop生态基础的HDFS分布式文件系统，它具有极高的容错性，适合部署在廉价的机器上，并能提供高吞吐量的数据访问能力，专为大规模数据存取而设计。
>请用Java程序来模拟HDFS的三个应用场景：写文件、读文件、Node节点单点故障。场景1为必选，场景2和3可选但必需延续场景1的实现方案。程序请使用JDK原生API来实现。
>问题1：请用文字阐述你的设计方案。
>问题2：请用Java程序来分别实现你的方案。

前提：了解HDFS，IO流
思路：单就写文件的功能来看的话，需要将一个文件，写到HDFS中，这里的文件就是文件，然后机架就用文件夹来模拟，就像MySql一样，每个文件为一个块64MB。
```
package net.ldc4.argorithm.foroffer;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;



class HDFS{
	
	File[] racks = new File[3];//假设就3个机架
	
	public HDFS(String path){
		File file = new File(path,"HDFS");
		
		file.mkdir();
		
		for(int i=0;i<racks.length;i++){
			racks[i] = new File(file,"rock"+i);
			racks[i].mkdir();
		}
	}
}

public class Ali_1{
	public static void main(String[] args) {
		HDFS hdfs = new HDFS(".");
		new Ali_1().hdfsWrite(new File("hdfs.txt"), hdfs);
	}
	
	private boolean hdfsWrite(File file,HDFS hdfs){
		
		//0.读取file信息
		String fileName = file.getName();
		int hashcode = Math.abs(file.hashCode());
		long fileSize = file.length();
		int n = (int)(fileSize/1024.0/1024.0);//如果是63.9结果就会变为63
		//1.计算需要分成几块
		int pieceNum = n/64 + 1;
		//2.将文件按64MB分块
		BufferedInputStream bufin = null;
		BufferedOutputStream[] bufout = new BufferedOutputStream[pieceNum];
		if(file.isFile()){
			try {
				bufin = new BufferedInputStream(new FileInputStream(file));
				int len = 1024*1024*64;
				byte[] b = new byte[len];
				int selectIndex = hashcode % hdfs.racks.length;//根据hash来选择存储的机架
				for(int i=0;(len = bufin.read(b))>0;i++){
					//3.Switch(根据hashcode)，默认机架文件夹名为Rock-?
					File blockFile = new File(hdfs.racks[selectIndex],fileName+"-"+i);
					bufout[i] = new BufferedOutputStream(new FileOutputStream(blockFile));
					bufout[i].write(b,0,len);
				}
			} catch (IOException e) {
				new RuntimeException(e);
			}
		}else{
			System.out.println("不是文件");
			return false;
		}
		
		return true;
	}
	
}



/*这是之前提交的代码。。
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;


public class Ali_1 {

	private static StringBuffer log = new StringBuffer();
	
	public static void main(String[] args) {
		
	}
	
	
	private boolean write(String data, File[] files){
		int size = files.length;
		int hashcode = data.hashCode();
		
		File selectFile = files[hashcode%size];
		
		BufferedOutputStream out = null;
		BufferedInputStream in = null;
		try {
			out = new BufferedOutputStream(new FileOutputStream(selectFile));
			//写之前把要写的部分先日志记录下来
			in = new BufferedInputStream(new FileInputStream(selectFile));
			
			String originData = "";
			
			byte[] bytes = new byte[1024*8];
			int len = 0;
			while((len = in.read(bytes, 0, len))>0){
				originData += bytes.toString();
			}
			
			log.append(originData);
			log.append("\n");
		} catch (IOException e) {
			//回滚（因为还没进行数据读写，所以没有回滚操作）
			return false;
		}
		
		byte[] bytes = data.getBytes();
		
		try {
			log.append(bytes);//日志记录修改
			out.write(bytes);
		} catch (IOException e) {
			//回滚
			rollback(out);
			return false;
		}
		
		return true;
	}

	//回滚即进行逆向操作
	private void rollback(BufferedOutputStream out) {
		String logData = log.toString();
		String[] datas = logData.split("\n");
		String originData = datas[0];
		String data = datas[1];
		
		//
		
	}
	
}
*/
```




# 第二题

>优惠券是目前较为受用户欢迎的促销手段，为了方便用户使用优惠券，网站在用户提交购买购物车中的商品时自动为用户推荐并使用最合适的优惠券。目前假设有两类优惠券：
>1、“满包邮”：即在单一店铺中购买商品总价满足一定条件时会减免用户的快递费用，例如：满100包邮
>2、“红包”：即单一店铺中购买商品总价满足一定条件时会产生一定程度的金额减免，例如：满100减10、满300减20等
>请就如上设定，设计购物车提交时优惠券的推荐程序，规定每个店铺只能使用一张优惠券。
>问题1：请阐述你的设计方案，形式不限
>问题2：请用Java实现推荐程序，代码范围限定使用JDK原生API

前提：每个店铺只能使用一张优惠券。
思路：
有哪些对象？
优惠券，包邮，联想到邮费，加上商品本身的数量，价格，以及多种商品，商品隶属于某个店铺。红包，这里可以把之前的邮费都归类于优惠券。商店拥有优惠券的设置权利
因此：店铺、商品、优惠券三者之间的一个关系

每个商品的邮费不一样

优惠券得达到100才能用。。

```
package net.ldc4.argorithm.foroffer;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class Ali_2 {

	public static void main(String[] args) {
		//模拟购物车：
		Shop shopA = new Shop();
		shopA.setName("A商店");
		ArrayList<Coupon> couponList = new ArrayList<Coupon>();
		Coupon c1 = new Coupon();
		c1.setLimit(250);
		c1.setType(Coupon.MJY);
		couponList.add(c1);
		
		Coupon c2 = new Coupon();
		c2.setLimit(250);
		c2.setType(Coupon.HB);
		c2.setMoney(50);
		couponList.add(c2);
		
		shopA.setCouponList(couponList);
		
		
		
		Goods goodsA = new Goods();
		goodsA.setName("AAA");
		goodsA.setShop(shopA);
		goodsA.setNum(2);
		goodsA.setPrice(99.9);
		goodsA.setPostage(10.0);
		
		Goods goodsB = new Goods();
		goodsB.setName("BBB");
		goodsB.setShop(shopA);
		goodsB.setNum(1);
		goodsB.setPrice(59);
		goodsB.setPostage(5.0);
		
		Goods goodsC = new Goods();
		goodsC.setName("CCC");
		goodsC.setShop(shopA);
		goodsC.setNum(3);
		goodsC.setPrice(44);
		goodsC.setPostage(3.0);
	
		Goods[] goods = {goodsA,goodsB,goodsC};
		
		buy(goods);
	}
	
	//参数：购物车里面的商品
	private static void buy(Goods[] goods){
		//把商品分类
		HashMap<Shop,ArrayList<Goods>> map = new HashMap<Shop,ArrayList<Goods>>();
		int len = goods.length;
		for(int i=0; i<len; i++){
			ArrayList<Goods> goodList = map.get(goods[i].getShop());
			if(goodList == null){
				goodList = new ArrayList<Goods>();
			}
			goodList.add(goods[i]);
			map.put(goods[i].getShop(), goodList);
		}
		
		for(Map.Entry<Shop,ArrayList<Goods>> entry : map.entrySet()){
			Coupon result = recommond(entry.getKey(), entry.getValue());
			System.out.println(result);
		}
		
	}
	//参数：同一店铺的购物车
	//返回：推荐使用的优惠券
	private static Coupon recommond(Shop shop, ArrayList<Goods> goodList){
		//计算商品的总值
		double totalPrice = 0.0;
		Iterator<Goods> it = goodList.iterator();
		while(it.hasNext()){
			Goods g = it.next();
			totalPrice = totalPrice + g.getPrice()*g.getNum();
		}

		//为商店支持的每个优惠券(且用户满额的)计算优惠价格
		ArrayList<Coupon> couponList = shop.getCouponList();
		Iterator<Coupon> cit = couponList.iterator();
		Coupon result = new Coupon();//设置最好的优惠券
		result.setMoney(0.0);//初始化
		while(cit.hasNext()){
			Coupon c = cit.next();
			if(c.getLimit()>totalPrice)
				continue;
			//满包邮
			if(c.getType()==Coupon.MJY){
				//计算所有商品的邮费
				double postage = 0;
				Iterator<Goods> git = goodList.iterator();
				while(git.hasNext()){
					Goods g = git.next();
					postage += g.getPostage()*g.getNum();
				}
				c.setMoney(postage);
				if(result.getMoney()<c.getMoney()){
					result = c;
				}
			}
			//红包
			if(c.getType()==Coupon.HB){
				if(result.getMoney()<c.getMoney()){
					result = c;
				}
			}
		}
		return result;
	}
	
}


class Shop{
	private String name;//名称
	private ArrayList<Coupon> couponList;//商店开放的优惠券
	private ArrayList<Goods> goodsList;//商店开放购买的商品
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public ArrayList<Coupon> getCouponList() {
		return couponList;
	}
	public void setCouponList(ArrayList<Coupon> couponList) {
		this.couponList = couponList;
	}
	public ArrayList<Goods> getGoodsList() {
		return goodsList;
	}
	public void setGoodsList(ArrayList<Goods> goodsList) {
		this.goodsList = goodsList;
	}
}

class Goods{
	private String name;//名称
	private double price;//价格
	private double postage;//邮费
	private int num;//数量
	
	private Shop shop;//所属商店

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public double getPrice() {
		return price;
	}

	public void setPrice(double price) {
		this.price = price;
	}

	public double getPostage() {
		return postage;
	}

	public void setPostage(double postage) {
		this.postage = postage;
	}

	public int getNum() {
		return num;
	}

	public void setNum(int num) {
		this.num = num;
	}

	public Shop getShop() {
		return shop;
	}

	public void setShop(Shop shop) {
		this.shop = shop;
	}
	
	
}

class Coupon{
	public static final int MJY = 0;
	public static final int HB = 1;
	
	private int type;
	private double money;
	private double limit;
	public double getLimit() {
		return limit;
	}
	public void setLimit(double limit) {
		this.limit = limit;
	}
	public int getType() {
		return type;
	}
	public void setType(int type) {
		this.type = type;
	}
	public double getMoney() {
		return money;
	}
	public void setMoney(double money) {
		this.money = money;
	}
	@Override
	public String toString() {
		return "Coupon [type=" + type + ", money=" + money + ", limit=" + limit
				+ "]";
	}
}

```


# 第三题

>问题1：尝试用java编写一个转账接口，传入转出账号，转入账号，转账金额3个数据，完成转出和转入账号的资金处理，该服务要确保在资金处理时转出账户的余额不会透支，金额计算准确，能够支撑每天10万笔的个人用户之间转账。
>问题2：假设接口构建完成后，淘宝的担保交易也准备使用该接口，每次用户购买淘宝的商品，都会调用转账接口，将资金由买家账户转到一个担保交易的中间账户，等到买家收到货并满意后进行确认收货，再调用转账接口从这个担保交易中间账户转账资金到卖家账户，通过这样的手段保证买家的权益，做到只有买家对货满意才给卖家钱。此时面对淘宝担保交易的海量交易处理，原来面向个人用户间转账的转账接口可能会遇到怎样的问题？你有怎样的解决方案？并尝试在不侵入原接口主处理流程的前提下修改代码，优雅的支持淘宝担保交易记账模式。


问题1：不透支（逻辑），金额计算准确（多线程） ，10万（大数据）。
问题2：
会遇到什么问题呢？。。。



```
package net.ldc4.argorithm.foroffer;

public class Ali_3 {
	public static void main(String[] args) {
		final User user1 = new User();
		user1.setID("1");
		user1.setMoney(1000);
		
		final User user2 = new User();
		user2.setID("2");
		user2.setMoney(1000);
		
		final Ali_3 ali = new Ali_3();
		
		Runnable run = new Runnable() {
			public void run() {
				try {
					//Thread.sleep(1000);
				} catch (Exception e) {
					e.printStackTrace();
				}
				ali.transfer(user1, user2, 1);
			}
		};
		
		for(int i=0; i<1; i++){
			new Thread(run).start();
		}
	
	}
	
	private synchronized boolean transfer(User fromUser, User toUser, double money){
		if(money > fromUser.getMoney())
			return false;
		double rowMoney = fromUser.getMoney();
		fromUser.setMoney(fromUser.getMoney() - money);
		try{
			//if(true){throw new RuntimeException();}
			toUser.setMoney(toUser.getMoney() + money);
		}catch(Exception e){
			//回滚，这里就简单处理了。思路的话：根据日志来回滚。
			fromUser.setMoney(rowMoney);
			return false;
		}
		System.out.println(fromUser.getMoney()+":"+toUser.getMoney());
		return true;
	}
	
}


class User{
	private String ID;
	private double money;
	public String getID() {
		return ID;
	}
	public void setID(String iD) {
		ID = iD;
	}
	public double getMoney() {
		return money;
	}
	public void setMoney(double money) {
		this.money = money;
	}
}
```


代码位置:[LeetCode->net.ldc4.argorithm.foroffer](https://github.com/ldc4/LeetCode/tree/master/src/net/ldc4/argorithm/foroffer)