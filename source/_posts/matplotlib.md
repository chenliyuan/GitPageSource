title: matplotlib_pyplot
author: 躲不掉的风
date: 2019-11-05 12:36:15
tags:
---
#### pyplot 
subplot(numRows, numCols, plotnum)   如

    plt.figure()
    plt.subplot(121) #一行两列，处于第一个位置
    plt.title("line")
    #只有一个列表，默认索引为X轴坐标
    p1=plt.plot(range(5),linewidth=2.0,color='g')
    p2=plt.plot([5,4,3,2,1],linewidth=2.0,color='r')

    plt.xlabel("this is x")
    plt.ylabel("this is y")
    plt.legend([p1,p2],['p1label','p2label'])
    #print(line)
    #line.set(color = 'g',linewidth = 2.0)

    plt.subplot(122)
    plt.title("src_img")
    plt.imshow(src_img, cmap='gray')
    plt.savefig("d:/cly/xuanxiu/img/test.png")#不支持jpg
    
    plt.show()#必须加
#### pyplot.legend

loc：位置信息

    0: ‘best'
    1: ‘upper right'
    2: ‘upper left'
    3: ‘lower left'
    4: ‘lower right'
    5: ‘right'
    6: ‘center left'
    7: ‘center right'
    8: ‘lower center'
    9: ‘upper center'
    10: ‘center'
