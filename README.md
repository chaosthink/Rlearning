# Rlearning
data()# 查看数据集列表
data(co2)  # 载入CO2数据集
library(MASS)
##r的复习，从头开始认真敲一遍
data(package='MASS')
data(SP500)
star<- SP500
head(star)
plot(SP500)
################################
#读取文本文件
getwd()
data212<- read.table('salary.txt',T)
data212
data818<- read.csv('salary.csv',T)
head(data818)
#以上两个函数不能出现缺失值，不然会报错或变NA值
data2<- scan('salary.txt',skip=1,what=list(City="",Work=0,Price=0,Salary=0))
data2  #what里可以直接定义数据类型，skip可以指定跳过数据行数
mode(data2) #查看数据类型
names(data2)
data.fwf<- read.fwf('fwf.txt',widths=c(2,4,4,3),col.names=c('w','x','y','z')) 
data.fwf###读取固定长度的数据
##
data.excle<- read.delim('clipboard')
#下载程序包
install.packages('RODBC')
library(RODBC)
excel.read<- odbcConnectExcel2007('Salary.xlsx')
sqlTables(excel.read)
data.excle2<- sqlFetch(excel.read,'Sheet1')
data.excel2<- sqlQuery(excel.read,'select*from[Sheet1$]')
close(excel.read)
##重点：使用RODBC打开的是整个excel，然后进行操作，使用sqlQuery可以像sql一样操控excel
##连接数据库
channel<- odbvConnect('LEV2',uid='L2st',pwd='sa')
#连接lev2数据库，ID-密码
snt1<- sqlFetch(channel,'TC_SNT')
#连接channel表
wa<- sqlQuery(channel,'SELECT*FROM TC_SNT WHERE ID=1 order by RECORD_TIME DESC ')；
#使用sqlQuery进行查询
####
####读取网页数据
install.packages('XML')
library(XML)
baseURL<- 'http://data.eastmoney.com/center/stock.html'
table<- readHTMLTable(baseURL,T,which=1)

james<- 'http://www.basketball-reference.com/players/j/jamesle01.html'
jamesr<- readHTMLTable(readLines(james),which=1,header=T)
dim(jamesr)
head(jamesr)
##步骤，找出链接，然后读取，注意选取第几个表格
##保存数据
write.table(data212,file='salary1.txt',col.names=T,quote=F)
#将数据保存为TXT文件
#########
#############12/10
data23<- read.table('salary.txt',T,stringsAsFactors=F)
a<- c("Hong",1910,75.0,41.8)
data24<- rbind(data23,a)
data24
is.data.frame(data24)
####
####构造数据框
weight<- c(150,135,210,140)
height<- c(65,61,70,65)
gender<- c('F','F','M','F')
stu<- data.frame(weight,height,gender)
stu   ##data.frame是按列组合的
####merge
index<- list('City'=data23$City,'index'=1:15)
index
hebing<- merge(index,data23,by='City')
data23
hebing
##简单切片
data23[data23$Salary>65,]
data[c(2,4),]
###
##排序
order.salary<- order(data23$Salary)
data23[order.salary,]
t(data23)  #转置
##################
#################
install.packages('reshape2')
library(reshape2)
#####揉函数包，主要使用的有 melt/acast/dcast
x<- data.frame(A=1:4,B=seq(1.2,1.5,0.1),C=rep(1:4))
x
melt(x)#拆成两列
data(airquality)
str(airquality)
head(airquality)
longdata<- melt(airquality,id.vars=c('Ozone','Month','Day'),measure.vars=2:4)
head(longdata)
#########
###################3
###########、
###################简易绘图
dat<- read.table('online shopping.txt',T)
x<- rnorm(1000)
x<- x[x<0]
y<- data.frame(x1=1:5,x2=rnorm(5,0,1),x3=rgamma(5,2,3))
y
par(mfrow=c(2,2))
attach(dat)
plot(period,amount)
hist(x,xlim=range(x),main='hist of x',freq=F,nclass=30,density=20,angle=45)
matplot(y,type='l',col=1:3)
plot(period,amount,pch=22,col='red',bg='yellow',cex=1.5)
title('online shopping')
#这里主要教par的用法，par是均匀的分成四部分，下面用layout可以任意分区
layout(matrix(1:4,2,2))
layout(matrix(c(1,3,2,3),2,2))
layout(matrix(c(1,1,2,3,2,3),2,3))
layout.show(3)
#####复习plot函数
detach(dat)
attach(dat)
plot(period,amount)
plot(amount~period,data=dat)#另一种画法
#hist()直方图——具有统计特征，连续型数据
#pie()饼图
#boxplot()箱线图
#barplot()条形图
#qqplot() 正态分布图
#heatmap() 热图
percent<- c(56.7,19.6,5.5,4.7,2.7,10.8)
label2<- c('天猫','京东','苏宁易购','腾讯','亚马逊中国','其他')
pie(percent,label2,col=2:7,main='star star')
#hist breaks设置柱形间距 freq等于F则表示y轴是密度而不是频率 prob若为T表示y轴是频率不是频数
###续上-- nclass改变分类数  desity和angle 设置柱形上的斜线 border设置边界颜色
x<- rnorm(1000) 
x<- x[x<0]
hist(x,xlim=range(x),freq=F,nclass=30,density=20,angle=45)
#####低级绘图函数——直接在图上加东西
lines(density(x),col='blue')
lines(-3:0,dnorm(-3:0,mean(x),sd(x)),col='red')
points(-2.5,0.45,col='green')
text(-2.5,0.45,'大煞笔')
axis(1,1:3)
####低级函数类似于补充
##points(x,y)  添加点
##lines(x,y) 添加线
##title()添加标题
##legend 添加图例
##text(x,y,labels)在点添加文字
##axis(side,vect)
###box 添加边框
######
##交互式绘图
x<- rnorm(10)
plot(x)
locator(5,'o',col='red')
locator(5,'p',col='blue')
locator(5,'l',col='green')
locator(5,'n',col='red')
plot(x)
identify(c(4,6),c(1,0),label=c('a','b'),col='red')
####
########三维图像
#image(x,y,z) 
#contour(x,y,z)
#persp(x,y,z)

x<- seq(-10,10,length.out=50)
y<-x 
rotsinc<- function(x,y){
    sinc=function(x){
       y=sin(x)/x 
        y[is.na(y)]=1
         y}
     10*sinc(sqrt(x^2+y^2))
  }
sinc.exp<- expression(z= Sinc(sqrt(x^2+y^2)))
z<- outer(x,y,rotsinc)
oldpar<- par(bg='white')
persp(x,y,z,theta=50,phi=30,expand=0.5,col='lightblue')
title(main=sinc.exp)
#####画出了Sinc(sqrt(x^2+y^2)函数
######使用程序包画3d函数
install.packages('rgl')
library(rgl)
mortality<- read.csv('swedish mortality.csv',T)
attach(mortality)
plot3d(Age,Year,q_male,col='grey',type='l',zlim=1)
#######
###lattice包
install.packages('lattice')
library(lattice)
#historam(~x)直方图
#densityplot(~x)核密度函数
#qqmath(~x)
#qq(~x)
#cloud(z~x*y)3d散点图
#wireframe(z~x*y)3d透视图
library(ggplot2)
data(diamonds)
head(diamonds)
chouyang<- diamonds[sample(nrow(diamonds),1000),]
xyplot(price~carat,data=chouyang,groups=cut,auto.key=list(corner=c(1,0)),type=c('p','smooth'),span=0.7)
xyplot(price~carat,data=chouyang,groups=cut)
##相当简单，type可以指定多种线
bwplot(color~price | cut,data=diamonds)
####|cut 制定用cut分类
histogram(~price|color,data=diamonds,layout=c(2,4))#使用layout更换分组排列
####
x<- seq(-pi,pi,len=20)
y<- seq(-pi,pi,len=20)
g<- expand.grid(x=x,y=y)
g$z<- sin(sqrt(g$x^2+g$y^2))
wireframe(z~x*y,data=g,drape=T,aspect=c(3,1),colorkey=T)
#####ggplot2
sample<- diamonds[sample(nrow(diamonds),200),]
qplot(carat,price,data=sample,shape=cut,color=color)
# geom='points'散点图，绘制方法，在qplot的参数里加geom=啥就行，多个类型用c链接
# geom='smooth' 对三点拟合曲线
# geom='boxplot' 箱线图
# geom='path'  or 'line' 在数据点之间连线，常用于绘制时序图
# geom='histogram' 直方图
# geom='freqploy' 频数多边形
# geom='density' 绘制密度函数
# geom='bar' 离散变量绘制条形图
qplot(carat,price,data=sample,geom=c('point','smooth'),span=0.5)#span改变平滑程度
qplot(carat,data=diamonds,geom='histogram')#简单的直方图
qplot(carat,data=diamonds,geom='histogram',xlim=c(0,3))#调整x轴
qplot(carat,data=diamonds,geom='histogram',xlim=c(0,3),binwidth=0.1,fill=color)
#调整颜色用fill，宽度用binwidth
###################################
sample2<- diamonds[sample(nrow(diamonds),1000),]
p<- ggplot(data=sample2,mapping=aes(x=carat,y=price,color=clarity))
p #这里的只是建立了映射而已
#建立完之后可以开始添加图表（一层一层的）
#geom_abline() 直线
#geom_bar() #条形图
#geom_blank 空白，只显示横纵坐标
#geom_bin2d() 二维热图
#geom_contour() 三维表面图
#geom_density()平滑密度曲线图
#geom_dotplot() #点图
#geom_histogram() #直方图
#geom_hline()/geom_vline() 加水平/垂直线
#geom_jitter() 增加扰动
#geom_line() 曲线图
#geom_map() 数据地图
#geom_path() 路径图
#geom_point() 散点图
#geom_smooth() 平滑取现
#geom_text() 增加文本注释
p+geom_point()+geom_smooth() #因为已经添加过数据集和数据了，所以不需要再添加
##相同写法
d<- ggplot(data=sample2,aes(x=carat,y=price,color=clarity))+geom_point()+geom_smooth()
plot(d)
#####
sample3<- diamonds[sample(nrow(diamonds),100),]
p<- ggplot(data=sample3,aes(x=carat,y=price))
p+geom_point(aes(color=color,shape=cut,size=clarity,alpha=0.5,position='jitter')
####设置坐标轴样式的函数以 sale_x开头
p<- ggplot(data=diamonds,aes(x=carat))
p+geom_histogram()+xlim(0,3)
#scale_alpha_manual 透明度
#scale_color_manual 颜色
#scale_shape_manual 形状
#scale_size_manual 大小
p<- ggplot(data=diamonds,aes(x=carat,fill=color))
p+geom_histogram()+xlim(0,3)+scale_color_manual(values=rainbow(7))
####统计变换
# stat_ecdf  计算累计分布函数并画出曲线图
# stat_denstity 画核密度估计曲线
# stat_qq 绘制qq图
# stat_smooth 添加平滑曲线
# stat_unique 删除重复值
# stat_function 自定义函数并绘图！！这相当重要了
sample4<- diamonds[sample(nrow(diamonds),1000),]
p<- ggplot(sample4,aes(x=carat,y=price))+geom_point()+scale_y_log10()+stat_smooth()
p+scale_color_manual(values=color)
####
#####实战：数据地图
install.packages('maps')
library(maps)
lbj<- read.table('lbj.txt',T,quote="'")
attach(lbj)
state_map<-  map_data("state")
p<- ggplot(lbj,aes(map_id=state))
p+geom_map(aes(fill=AvgPTS),map=state_map)+
expand_limits(x=state_map$long,y=state_map$lat)+
scale_fill_continuous(limits=c(19,max(AvgPTS)),
high='red3',low='yellow',guide='colorbar')+annotate('text',x=xx,y=yy,label=labels)
###添加球队名称
attach(state_map)
state.uni<- unique(region)
xx<-0
yy<-0
for(i in 1:length(state.uni)){
   xx[i]=mean(long[region==state.uni[i]])
   yy[i]=mean(lat[region==state.uni[i]])
}
order<-0 
for(i in 1:length(state.uni)){
   order[i]=which(state==state.uni[i])
 }
labels=Opp[order]
p+annotate('text',x=xx,y=yy,label=labels)
#离散趋势分析
m<- range(price)
m
m[2]-m[1] #极差的算法
var(price)
sd(price)
mad(price)##中位数的离差
#数据的分布分析
library(timeDate)

piandu<- function(x){
  n=length(x)
  m=mean(x)
  sk=(n/((n-1)*(n-2)*sd(x)^3))*sum((x-m)^3)
  sk
 }
piandu(price)
skewness(price)

kurtosis(price)
######
#$########
#######
hist(price,breaks=50,prob=T)
lines(density(price),col='blue')
qqnorm(price)
qqline(price)
###画qq图
set.seed(1110)
s<- sample(price,50)
stem(s)##茎叶图
boxplot(price)
plot(ecdf(price))
x<- min(price):max(price)
lines(x,pnorm(x,mean(price),sd(price)),col='red')
group<- read.csv('09-11股价.csv')
group<- na.omit(group)
summary(group)
