很多工程项目中部署模型都需要采用caffe,但是原版caffe并不支持双线性插值操作。双线性插值在目标检测和图像分割模型中被经常使用，原版中不支持上采样，这为工程模型的工程部署造成很大麻烦，所以参照博客修改源码，加入插值操作，可实现上采样，但与双线性插值还有点不同，具体采样效果已上传(testimg.jpg被缩放到200x200，a.jpg为上采样效果，大小500x500)。
以下是使用方法与测试代码：                                                                                                                
                                                                                                       
1 我建议使用官方提供的caffe镜像         

2 运行caffe镜像生成容器

3 cd  /workspace                                                                                                          

4 git clone ........                                                                                                             

5 cd caffe-bilinear-upsample  

3.在容器中编译该版本caffe                                                                                                        
    1)make clean                                                                                                              
    2)make -j8                                                                                                                 
    3)make pycaffe
    
4.打开python交互式环境  

5.import  sys                                                                                                                 
   1)sys.path                                                                                                                 
   2)sys.path.pop(2)#数值具体是多少，根据自己情况，目的是删除镜像自带caffe路径）                                                                                  
   3)sys.path.pop(2)#数值具体是多少，根据自己情况，目的是删除镜像自带caffe路径）                                                                                       
   4)sys.path.append('/workspace/caffe-bilinear-upsample/python/')   
   
6 import caffe,cv2                                                                                                            
7 import numpy as np                                                                                                          
8 net=caffe.Net('deploy.prototxt',caffe.TEST)                                                                                  
9 img=cv2.imread('testimg.jpg')                                                                                               
10 img=cv2.resize(img,(200,200))                                                                                             
11 transformer = caffe.io.Transformer({'data': net.blobs['data'].data.shape})                                                  
12 transformer.set_transpose('data',(2,0,1))                                                                                  
13 out=net.forward_all(data=np.asarray(ptransformer.preprocess('data',img)]))                                                    
14 ret=out['interp'][0][0,:,:,:]                                                                                               
15 ret=ret.transpose()
16 ret=ret.swapaxes(1,0)                                                                                                         
17 cv2.imwrite('a.jpg',ret)                                                                                                                                                                                                           

后续会上传pytoch2caffe源码，目前的pytorch2caffe源码还不支持双线性插值。

代码经过进一步修改，算是弥补了一些不足。                                                                                                 
原代码：                                                                                                                            
原图MxM大小经过2倍上采样会得到(2M-1)x(2M-1)大小的特征图，这对不同层的特征融合仍然不友好。                                                             
修改代码：                                                                                                                       
经过修改会得到(2M+1)x(2M+1)大小特征图，再接上个2x2的卷积核，特征图变为2Mx2M大小。可以做不同层的特征融合了。                                                                                                         
经过测试，使用np.random.randn(50,50,3)得到的矩阵，经2倍上采样与opencv上采样2倍相比，平均每个点的误差在0.00001~0.0001之间                   

