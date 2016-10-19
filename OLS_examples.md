### Some examples with OLS

### Simulated dataset

```R
p=10
n=5000
b=rnorm(p)
X=matrix(nrow=n,ncol=p,data=runif(n*p))
signal=150+X%*%b
error=rnorm(sd=sd(signal),n=n)
y=signal+error

fm<-lm(y~X)
bHat=coef(fm)
summary(fm)

X=cbind(1,X)
bHat_2 = solve(crossprod(X))%*%crossprod(X,y)

plot(b,bHat[-1])
plot(bHat[-1],bHat_2[-1])

Z=matrix(nrow=10000,ncol=1000,rnorm(1000*10000))
C=crossprod(cbind(Z))
system.time(CInv.solve<-solve(C))
system.time(CInv.chol<-chol2inv(chol(C)))
all(round(CInv.solve,8)==round(CInv.chol,8))


bHat_3 = chol2inv(chol(crossprod(X)))%*%crossprod(X,y)


#### Galton dataset

DATA = read.table("~/Dropbox/UAB/Research/BST465/GaltonHeightData.txt",header=T)
head(DATA)
Gender = ifelse(DATA$Gender=="M",1,0)

fm=lm(Height~Gender,data=DATA,contrasts=list(Gender=contr.treatment))
fm$contrasts
coef(fm)
t1=mean(DATA$Height[which(DATA$Gender=="F")])
t2=mean(DATA$Height[which(DATA$Gender=="M")])
t2-t1

Gender2 = 2*Gender
X2 = cbind(1,Gender,Gender2)
y=DATA$Height
solve(crossprod(X2))%*%crossprod(X2,y)
ginv(crossprod(X2))%*%crossprod(X2,y)


x = (DATA$Father+DATA$Mother)/2
PA = x-mean(x)
X = cbind(1,PA,Gender)


coef(lm(Height~PA+Gender,data=DATA))
solve(crossprod(X))%*%crossprod(X,y)

### Run Gibbs sampler
colMeans(B[-c(1:burnIn),])

```
