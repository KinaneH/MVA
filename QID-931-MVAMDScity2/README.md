[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="1100" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **MVAMDScity2** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of QuantLet: MVAMDScity2

Published in: Applied Multivariate Statistical Analysis

Description: Computes the map of the cities by application of multidimensional scaling. Reflects and rotates the figure by 90 degrees.

Keywords: MDS, multi-dimensional, scaling, plot, graphical representation, sas

See also: MVAMDScity1, MVAMDSnonmstart, MVAMDSpooladj, MVAmdscarm, MVAnmdscar1, MVAnmdscar2, MVAnmdscar3

Author: Zografia Anastasiadou
Author[SAS]: Svetlana Bykovskaya
Author[Python]: Matthias Fengler, Tim Dass

Submitted: Tue, October 28 2014 by Felix Jung
Submitted[SAS]: Tue, April 5 2016 by Svetlana Bykovskaya
Submitted[Python]: Tue, April 23 2024 by Tim Dass

Example: Metric MDS solution for the intercity road distances after reflection and 90 degrees rotation.

```

![Picture1](MVAMDScity2_1.png)

![Picture2](MVAMDScity2_matlabOLD.png)

![Picture3](MVAMDScity2_python.png)

![Picture4](MVAMDScity2_sas.png)

### MATLAB Code
```matlab

% ---------------------------------------------------------------------
% Book:         MVA
% ---------------------------------------------------------------------
% Quantlet:     MVAMDScity2
% ---------------------------------------------------------------------
% Description:  MVAMDScity2 computes the map of the cities by application 
%               of multidimensional scaling. Reflects and rotates the 
%               figure by 90
%               Corresponds to example 16.1 in MVA. 
% ---------------------------------------------------------------------
% Usage:        -
% ---------------------------------------------------------------------
% Inputs:       None
% ---------------------------------------------------------------------
% Output:       Map of the cities as a metric MDS solution for the
%               inter-city road distances.
% ---------------------------------------------------------------------
% Example:      -
% ---------------------------------------------------------------------
% Author:       Wolfgang Haerdle, Vladimir Georgescu, Jorge Patron,
%               Song Song
% ---------------------------------------------------------------------

close all
clear
clc

%Intercity road distances
ber=[0 214 279 610 596 237];
dre=[214 0 492 533 496 444];
ham=[279 492 0 520 772 140];
kob=[610 533 520 0 521 687];
mue=[596 496 772 521 0 771];
ros=[237 444 140 687 771 0];

dist=[ber', dre', ham', kob', mue', ros'];

% aa, bb, hh, ii, rr, xx, xx1, xx2 are matrices
%aa,bb,hh,ii are matrices
aa=(dist.^2).*(-0.5);

ii=eye(6,6);
u=ones(1,6);

hh=ii-(1/6*(u'*u));

bb=hh*aa*hh; %Determine the inner product matrix

[v e]=eigs(bb);

g1(:,1)=v(:,1);
g1(:,2)=-v(:,2);

g2(1,1)=e(1,1);
g2(2,2)=e(2,2);
g2(1,2)=0;
g2(2,1)=0;

xx1=g1*(g2.^0.5); %Determine the coordinate matrix

x=cos(pi/2);
y=sin(pi/2);
z=-sin(pi/2);

rr(1,1)=x;
rr(1,2)=z;
rr(2,1)=y;
rr(2,2)=x;

xx2=xx1*rr;
xx=[(xx2(:,1)*(-1))+500 (xx2(:,2))+500];

hold on
scatter(xx(:,1),xx(:,2),'k')
title('Map of German Cities')
xlabel('EAST - WEST - DIRECTION in km')
ylabel('NORTH - SOUTH - DIRECTION in km')
xlim([0 900])
ylim([0 900])
set(gca,'box','on')

text(xx(1,1)+20,xx(1,2),'Berlin','Color','b')
text(xx(2,1)+20,xx(2,2),'Dresden','Color','b')
text(xx(3,1)+20,xx(3,2),'Hamburg','Color','b')
text(xx(4,1)+20,xx(4,2),'Koblenz','Color','b')
text(xx(5,1)+20,xx(5,2),'Muenchen','Color','b')
text(xx(6,1)+20,xx(6,2),'Rostock','Color','b')



```

automatically created on 2024-04-25

### PYTHON Code
```python

# works on numpy 1.25.2 and matplotlib 3.8.0
import numpy as np
import matplotlib.pyplot as plt

ber  = [0, 214, 279, 610, 596, 237]
dre  = [214, 0, 492, 533, 496, 444]
ham  = [279, 492, 0, 520, 772, 140]
kob  = [610, 533, 520, 0, 521, 687]
mue  = [596, 496, 772, 521, 0, 771]
ros  = [237, 444, 140, 687, 771, 0]

dist = np.array([ber, dre, ham, kob, mue, ros])

a = (dist**2) * (-0.5)
i = np.diag([1]*6)
u = [1]*6
u = np.reshape(u, (6, 1))
h = i - (1/6 * (u @ u.T))
b = h @ a @ h
e = np.linalg.eig(b)
xx1 = e[1] @ (np.diag(e[0])**0.5)
xx1 = xx1[:,0:2]

x = np.cos(np.pi/2)
y = np.sin(np.pi/2)
z = -np.sin(np.pi/2)
r = np.array([[x,z],
             [y,x]])
xx2 = np.dot(xx1, r)
xx2[:,0] = xx2[:,0] + 500
xx2[:,1] = xx2[:,1] + 500

cities = ["Berlin", "Dresden", "Hamburg", "Koblenz", "Muenchen", "Rostock"]

fig, ax = plt.subplots(figsize = (7, 7))
ax.scatter(xx2[:, 0], xx2[:, 1], c = "w", edgecolor = "k")
for i in range(len(cities)):
    ax.text(xx2[i, 0], xx2[i, 1], cities[i], c = "b", fontsize = 13)

ax.set_xlim(0, 800)
ax.set_ylim(0, 900)

ax.set_ylabel("NORTH - SOUTH - DIRECTION in km")
ax.set_xlabel("EAST - WEST - DIRECTION in km")

plt.title("Map of German Cities", fontsize = 18)

plt.show()
```

automatically created on 2024-04-25

### R Code
```r


# clear all variables
rm(list = ls(all = TRUE))
graphics.off()

# Intercity road distances
ber  = c(0, 214, 279, 610, 596, 237)
dre  = c(214, 0, 492, 533, 496, 444)
ham  = c(279, 492, 0, 520, 772, 140)
kob  = c(610, 533, 520, 0, 521, 687)
mue  = c(596, 496, 772, 521, 0, 771)
ros  = c(237, 444, 140, 687, 771, 0)

dist = cbind(ber, dre, ham, kob, mue, ros)

# a, b, h, i, r, x, xx1, xx2 are matrices
a    = (dist^2) * (-0.5)
i    = diag(6)
u    = rep(1, 6)
h    = i - (1/6 * (u %*% t(u)))
b    = h %*% a %*% h             # Determine the inner product matrix
e    = eigen(b)
g1   = cbind(e$vectors[, 1], -e$vectors[, 2])
g2   = diag(e$values[1:2])
xx1  = g1 %*% (g2^0.5)           # Determine the coordinate matrix
x    = cos(pi/2)
y    = sin(pi/2)
z    = -sin(pi/2)
r1   = c(x, z)
r2   = c(y, x)
r    = rbind(r1, r2)
xx2  = xx1 %*% r

xx   = cbind((xx2[, 1] * (-1)) + 500, xx2[, 2] + 500)

# Plot: Map of German Cities
plot(xx[, 1], xx[, 2], xlim = c(0, 900), ylim = c(0, 900), xlab = "EAST - WEST - DIRECTION in km", 
    ylab = "NORTH - SOUTH - DIRECTION in km", main = "Map of German Cities", cex.axis = 1.2, 
    cex.lab = 1.2, cex.main = 1.8)
text(xx[, 1], xx[, 2], labels = c("Berlin", "Dresden", "Hamburg", "Koblenz", "Muenchen", 
    "Rostock"), pos = 4, col = "blue") 

```

automatically created on 2024-04-25

### SAS Code
```sas


proc iml;
  * matrix "multiplication" where missing values are propagated;
  start MVMult(A, B);
    C = j(nrow(A), ncol(B), .);
    rows = loc(countmiss(A, "ROW")=0);
    cols = loc(countmiss(B, "COL")=0);
    if ncol(rows)>0 & ncol(cols)>0 then
      C[rows, cols] = A[rows,] * B[,cols];
    return(C);
  finish;
  
  * Intercity road distances;
  ber  = {0, 214, 279, 610, 596, 237};
  dre  = {214, 0, 492, 533, 496, 444};
  ham  = {279, 492, 0, 520, 772, 140};
  kob  = {610, 533, 520, 0, 521, 687};
  mue  = {596, 496, 772, 521, 0, 771};
  ros  = {237, 444, 140, 687, 771, 0};
  
  dist = ber || dre || ham || kob || mue || ros;
  
  * a, b, h, i, r, x, xx1, xx2 are matrices;
  a   = (dist ## 2) * (-0.5);
  u   = j(6,1,1);
  i   = diag(u);
  h   = i - (1/6 * (u * t(u)));
  b   = h * a * h;              * Determine the inner product matrix;
  V   = eigvec(b);
  g1  = V[,1] ||(-V[,2]);
  g2  = diag((eigval(b))[1:2,]);
  xx1 = MVMult(g1,(g2 ## 0.5)); * Determine the coordinate matrix;
  
  pi  = constant("pi");
  x   = cos(pi/2);
  y   = sin(pi/2);
  z   = -sin(pi/2);
  r1  = x || z;
  r2  = y || x;
  r   = r1 // r2;
  xx2 = MVMult(xx1,r);
  
  xx  = (xx2[, 1] * (-1)) + 500 || xx2[, 2] + 500; 
  
  x1  = xx[,1];
  x2  = xx[,2];
  cities = {'Berlin', 'Dresden', 'Hamburg', 'Koblenz', 'Muenchen', 'Rostock'};
  
  create plot var {"x1" "x2" "cities"};
    append;
  close plot;
quit;
 
proc sgplot data = plot;
  title 'Map of German Cities';
  scatter x = x1 y = x2 / datalabel = cities
    datalabelattrs = (color = blue) datalabelpos = right;
  xaxis min = 0 max = 1000 label = 'EAST - WEST - DIRECTION in km';
  yaxis min = 0 max = 1000 label = 'NORTH - SOUTH - DIRECTION in km';
run;




```

automatically created on 2024-04-25